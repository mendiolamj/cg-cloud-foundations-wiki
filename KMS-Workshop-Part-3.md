# AWS KMS best practices

In this section we are going to implement best practices for AWS KMS.
These best practices are based on the Whitepaper "**[AWS Key Management Service Best Practices](https://d0.awsstatic.com/whitepapers/aws-kms-best-practices.pdf)**"

This section has the following parts:
* Working with Key Policies
* Key Policies and VPC Private Endpoint
* AWS KMS key tagging
---

### Working with Key Policies

Key policies are the primary resource for controlling "who" has access to do "what" with your CMKs.
You have a full description about them in the following [AWS KMS link](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html), in case you want to go deeper - and for the importance of the topic,  you should. 
We are going to work with some practical examples. 

Up to now, the assigned IAM  role ("**KMSWorkshop-InstanceInitRole**") of our working instance allows us to perform many things in AWS KMS.  Following best practices like "**Least Privilege**" and "**Separation of Duties**", it can be, for example, that our instance is meant to be used only for uploading data with server side encryption, but not decrypt it and download it.
Maybe the download and decrypt operation needs to be done from another instance with more specific security constraints. 

How can we comply with these requirements? We will use two main resources:
* **IAM roles** and their policies are helpful to control access to CMKs. 
* **Key policies** are resource based policies that we can use to fine grain the access to the CMKs and tweak it to our needs.

Each key that is generated in AWS KMS, has an initial policy attached. Let's looks at the initial policy set for the key we created with our import material, alias "**ImportedCMK**". Type the following command in your instance terminal (replace "your-key-id" by the "**ImportedCMK**" KeyId or ARN, **Note:** the Alias will not work for this operation):

```
$ aws kms list-key-policies --key-id your-key-id

{
    "PolicyNames": [
        "default"
    ]
}
```
**NOTE:** Remember that if you run into trouble with this command and get errors complaining about "import awscli.clidriver", a versions mismatch with AWS CLI installed by boto3, then use the following command to go back to normal:

```
$ sudo yum downgrade aws-cli.noarch python27-botocore -y
```

Execute the command again and this time it should work. If you don´t have a copy of the KeyId, just issue again the list-aliases command  identify the KeyId of "ImportedCMK". 

```

$ aws kms list-aliases

{
    "Aliases": [
        {
            "AliasArn": "arn:aws:kms:eu-west-1:your-account-id:alias/ImportedCMK", 
            "AliasName": "alias/ImportedCMK", 
            "TargetKeyId": "your-key-id"
        }, 
```

Note that we are able to list key policies because the IAM role assigned of our instance allows this operation.  As you can see, we have a key policy attached to the key, its name is "Default". Let's see what it contains:

```
$ aws kms get-key-policy --key-id your-key-id --policy-name default
{
    "Policy": "{\n  \"Version\" : \"2012-10-17\",\n  \"Id\" : \"key-default-1\",\n  \"Statement\" : [ {\n    \"Sid\" : \"Enable IAM User Permissions\",\n    \"Effect\" : \"Allow\",\n    \"Principal\" : {\n      \"AWS\" : \"arn:aws:iam::your-account-id:root\"\n    },\n    \"Action\" : \"kms:*\",\n    \"Resource\" : \"*\"\n  } ]\n}"
}

```

The default key policy gives the AWS account (root user) that owns the CMK full access to the CMK.
This has policy has two important effects (more information in [this link](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-default)):

* Reduces the risk of the CMK becoming unmanageable. You cannot delete your AWS account's root user, so allowing access to this user reduces the risk of the CMK becoming unmanageable.
  
* Enables IAM policies to allow access to the CMK. Giving the AWS account full access to the CMK does this; it enables you to use IAM policies to give IAM users and roles in the account access to the CMK. It does not by itself give any IAM users or roles access to the CMK, but it enables you to use IAM policies to do so.

Let's modify the permission of the role assigned to the instance, **allowing it only to encrypt, but not decrypt**.
Let's use the console. Open the AWS Console. Navigate to IAM service, left column "Roles" and search for the role currently assigned to the instance: **KMSWorkshop-InstanceInitRole**. 


Within the role, locate the policy we attached when working in the second section of the workshop, we named it "**KMSWorkshop-AditionalPermissions**". Click the button "**Edit Policy**". Expand the Actions-Access Level-Write section and remove the check box on "**Decrypt**". Review policy and save it.


#### Least Privilege - Access only from the account

Now this role is able to encrypt but not to decrypt. Furthermore, we want to enforce "**Least Privilege**" access and ensure that the encryption Role, providing capability to encrypt, is only used from our account, and not subject to Cross-Account Role access policies that could grant access to the CMK. For that, we will use a handy Key policy.

We need to identify our current role "**KMSWorkshop-InstanceInitRole**" ARN in order to link it to the key policy. Go back to the console, IAM service, click Roles. Search for the role currently assigned to the instance: **KMSWorkshop-InstanceInitRole** and click on it.  In the upper part of the screen you have the associated ARN. As part of the Role ARN you have your account Id. This is the generic structure: 

```
arn:aws:iam::ACCOUNT-ID-WITHOUT-HYPHENS:role/ROLE-NAME
```
Copy the account Id and write it down, we are going to use it now. 



Go back to the left column click "**Encryption Keys**". 
Identify our CMK whose Alias is "**Imported CMK**" and click on it. You will land on a page with more details about the key itself. Scroll down until you see "**Key Policy**".

Modify the current policy with this one:

```
{
  "Version": "2012-10-17",
  "Id": "key-default-1",
  "Statement": [
    {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::your-acount-id:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "Allow for Use only within our Account",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::your-acount-id:role/KMSWorkshop-InstanceInitRole"
      },
      "Action": "kms:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "kms:CallerAccount": "your-account-id"
        }
      }
    }
  ]
}
```

Make sure you replace "**your-account-id**" for the real values of your account in all the places where it is needed.  With this policy you have restricted the key to be used only within your account.

#### Least Privilege - Access only from a Role

Another type of key policy that can become very useful when enforcing "**Least Privilege**" is the capability to also ensure that this CMK is only called by our role and other roles or user can´t use the key for encryption or decryption. For that, you can use a policy like the one below.

With this policy we will ensure that only instances that have the appropriate role attached are able to use the key. It can become handy to control the instances/services tha can use the CMKs, only allowing it through a specific role. This policy is set as an example of what can be done. **Optionally you can enforce it, but it is not required in the workshop**.


```

{
  "Version": "2012-10-17",
  "Id": "key-default-1",
  "Statement": [
    {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::your-acount-id:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "Allow for Use only within our Account",
      "Effect": "Deny",
      "NotPrincipal": { 
        "AWS": [ "arn:aws:iam::your-acount-id:role/KMSWorkshop-InstanceInitRole", "arn:aws:iam::your-acount-id:root"]
      },
      "Action": "kms:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "kms:CallerAccount": "your-account-id"
        }
      }
    }
  ]
}

```


#### Key Policy - Including conditions

Furthermore,  you can also include conditions over key policies to help you fine tune access and link to several other parameters. 
As an example, conditions can work with **encryption context** to be able to restrict the operations for this KMS. Amazon S3 when calling AWS KMS to generate a Data Key and perform the envelope encryption process, it passes and encryption context to AWS KMS, see below a log from a "**GenerateDataKey**" event:
```
"eventName": "GenerateDataKey",
…
"requestParameters": {
        "keySpec": "AES_256",
        "encryptionContext": {
            "aws:s3:arn": "arn:aws:s3:::kms-workshop/SampleFile-KMS.txt"
        },
```

We could use that to add a condition in AWS KMS Key Policy like "**kms:EncryptionContextKey**". 
There is a full example in the [documentation here](https://docs.aws.amazon.com/kms/latest/developerguide/policy-conditions.html#conditions-kms-encryption-context-keys). Make sure you click the link and check that you understand how the key policy is enforced using encryption context as condition.

Finally, let's try to add another layer of security via **MFA**. In the key policy we might request that the users that are going to use the CMK have passed through a MFA process. 
MFA is enforced through a condition as seen below:

```
{
  "Version": "2012-10-17",
  "Id": "key-default-1",
  "Statement": [
    {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::your-acount-id:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "Allow for Use only within our Account",
      "Effect": "Allow",
      "Principal": { 
        "AWS": [ "arn:aws:iam::your-acount-id:user/userA",]
      },
      "Action":  [
    "kms:DeleteAlias",
    "kms:DeleteImportedKeyMaterial",
    "kms:PutKeyPolicy",
    "kms:ScheduleKeyDeletion"
     ], 
      "Resource": "*",
      "Condition": {
        "NumericLessThan":{"aws: MultiFactorAuthAge":"300"}       }
    }
  ]
}
```

In this key policy, we don´t allow certain very sensitive operations to take place, unless the user or the role has gone through a MFA authentication process in the last 5 minutes (300 seconds). Make sure you understand the policy. After the examples seen by now, you should be able to understand how it works. We will not enforce as in the workshop we are working with roles and not with users. However the overall concept is the same.

---


### VPC Endpoints and Key Policies

Resources can communicate with AWS KMS through a VPC private endpoint.
A VPC endpoint enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. 

When you use a VPC endpoint, **communication between your VPC and AWS KMS is conducted entirely within the AWS network**.
You can even specify the VPC endpoint in [AWS KMS API operations](https://docs.aws.amazon.com/kms/latest/APIReference/) and [AWS CLI commands](https://docs.aws.amazon.com/kms/latest/APIReference/).
In order not to make the workshop too extensive **it is not required to set up the VPC Endpoint**. We will mention it, so you know what you can do with it.

As a reference: the VPC endpoint can  easily be created from the console, you can follow the steps in this [link to the documentation](https://docs.aws.amazon.com/kms/latest/developerguide/kms-vpc-endpoint.html).
Once an endpoint is created you can enforce communication through it with commands as seen below, where parameter "**--endpoint-url**" is added. 

```
$ aws kms list-keys --endpoint-url https://vpce-xxxxxxxxa-xxxxxx.kms.your-region.vpce.amazonaws.com
```


VPC endpoints would allows us to establish more restrictive conditions to the operations we allow on AWS KMS and its CMKs.

For example, change the key policy of a CMK to allow only certain operations from the VPC endpoint.
A sample policy would be like the one below. Please ensure you understand the conditions applied before continuing.

```
{
  "Version": "2012-10-17",
  "Id": "key-default-1",
  "Statement": [
    {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::your-account-id:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "Allow for Use only within our VPC",
      "Effect": "Deny",
      "Principal": {
        "AWS": "arn:aws:iam::your-account-id:role/KMSWorkshop-InstanceInitRole"
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*"
      ],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:sourceVpce": "vpce-1xxxxxxxx1"
        }
      }
    }
  ]
}
```

With that key policy, an extra layer of protection the key would be established, making AWS KMS only available internally within AWS network for certain sensitive operations.


---



### Key Tagging

Tagging is an important strategy for managing CMKs in AWS KMS.
You can add, change, and delete tags for customer managed CMKs. Each tag consists of a tag key and a tag value that you define.
You can add tags to a CMK when you first create them. Then, add, edit, and delete tags at any time. 

To add a tag to the CMK we have been working with, you can use the console or the CLI. Let's tag our CMK "**ImportedCMK**", with a project it may belong to, just an a example.

```
$ aws kms tag-resource --key-id your-key-id --tags TagKey=project,TagValue=kmsworkshop
```

The other usual operations with tags are also available: list tags for resource and untag the resource.
Let's list the resource tags:

```
$ aws kms list-resource-tags --key-id your-key-id
```

Add a few more tags to the CMK and try to remove the tags (untag) with commad "untag-resource".
For information on the command, please use [this section of the AWS KMS documentation](https://docs.aws.amazon.com/kms/latest/developerguide/tagging-keys.html).



This section of the Workshop is now completed. Please, navigate to [the next Section, Monitoring AWS KMS](https://github.com/aws-samples/aws-kms-workshop/blob/master/Section-4-Monitoring-AWS-KMS.md).



