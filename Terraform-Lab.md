
Here's an overview of the lab:

<img width="588" alt="Screen Shot 2023-01-09 at 1 32 33 AM" src="https://user-images.githubusercontent.com/25653204/211251969-7fa5c3a7-f0d4-4f94-8059-72cec766e262.png">

## Part 1 - Install Terraform and Create Resources on AWS

Here are the steps to take:

1. Navigate to IAM Users > CloudLearner.
1. Navigate to the Security Credentials tab and click on the Create access key button (if you don’t have it already with you). 
1. Download and save the generated file on your laptop. This file contains secret access keys for your CloudLearner user. 
1. Make sure you have removed the IAM role from your Public EC2 instance. 
1. On the Public EC2 instance, configure your credentials for AWS CLI. Execute: 


```console
aws configure 
Specify the Access Key ID & Secret Access Key from the file you have. 
Specify us-east-1 for region.
Specify json for output format. 
```
6. Do the necessary setup on the Public EC2 instance to run Terraform code. Execute the following: 

```console
aws sts get-caller-identity

sudo yum install -y yum-utils

sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo

sudo yum -y install terraform

terraform -help

terraform -install-autocomplete

source ~/.bashrc

mkdir tf-lab

cd tf-lab

wget https://usaa-aws-resources.s3.amazonaws.com/tf-lab/main.tf 

ls 

terraform init

terraform validate

terraform apply
```

7. Verify the created resource in the AWS management console. 
8. Now destroy this resource. Execute:

```console
terraform destroy
rm main.tf
```

---------------------------------------------

## Part 2 - Variables Testing

Let's take a look at variables in Terraform.  But first, let's talk about why we would need them.


Suppose we want to give our EC2 instance a tag name - would it be a good idea to hardcode that name directly inside our Terraform module?  Yes, we did actually hardcode the tag name in our Terraform module earlier - but the answer is no - that is typically not a good idea.

Think about why.🤔

Well it basically makes our Terraform module less reusable and composable if we hardcode the instance name there.

So, instead of hardcoding variable we can use variables so that our modules are more reusable and composable.  

If you're familiar with traditional programming languages, it can be useful to compare Terraform modules to function definitions - and if terraform modules are like function definitions then:
* Input variables are like function arguments.
* [Output values](https://developer.hashicorp.com/terraform/language/values/outputs) are like function return values.
* [Local values](https://developer.hashicorp.com/terraform/language/values/locals) are like a function's temporary local variables


So, we can say that our main.tf file is essentially like a function definition - in the new main.tf file that you will be downloading, you will see why it looks a bit like a function definition.

First, delete the older main.tf file. 

Go ahead and download these files: 


```console
wget https://usaa-aws-resources.s3.amazonaws.com/tf-lab/02+-+with+input+%26+output+file/variables.tf

wget https://usaa-aws-resources.s3.amazonaws.com/tf-lab/02+-+with+input+%26+output+file/main.tf

wget https://usaa-aws-resources.s3.amazonaws.com/tf-lab/02+-+with+input+%26+output+file/outputs.tf

wget https://usaa-aws-resources.s3.amazonaws.com/tf-lab/03+-+with+tfvars/testing.tfvars 
```


Take a look at the variables.tf file - you will see that this is where we define the variable:

```bash
variable "instance_name" {
  description = "Value of the Name tag for the EC2 instance"
  type        = string
  default     = "1ExampleAppServerInstance"
}
```


Execute: 
```console
terraform apply -var-file="testing.tfvars"
```
------------------------------------------------------------------------------

## Bonus Milestone
Delete all the files from the tf-lab folder and then download this file. 

```console
wget https://usaa-aws-resources.s3.amazonaws.com/tf-lab/vpc/main.tf 
```

Execute this once and create the VPC. Then, destroy it. 

Afterwards, create a variables.tf file and a test.tfvars file. 

You should specify the VPC CIDR and all the subnet CIDRs using these variable files. At last, execute it by passing the CIDR values from the test.tfvars file. 

Execute terraform destroy and delete the resources.


