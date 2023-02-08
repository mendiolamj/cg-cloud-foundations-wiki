
Here's an overview of the lab:

<img width="588" alt="Screen Shot 2023-01-09 at 1 32 33 AM" src="https://user-images.githubusercontent.com/25653204/211251969-7fa5c3a7-f0d4-4f94-8059-72cec766e262.png">

## Part 1 - Install Terraform and Create Resources on AWS

Our goal is to create an EC2 instance using Terraform to see how IaC with Terraform works.


Terraform relies on plugins called providers to interact with cloud providers, SaaS providers, and other APIs.
Terraform configurations must declare which providers they require so that Terraform can install and use them. Additionally, some providers require configuration (like endpoint URLs or cloud regions) before they can be used.

Basically providers tell Terraform who we want to interact with - in our case, we want to interact with AWS, so we will see this code in the main.tf file that we will be downloading:

```code
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.27"
    }
```

Note that AWS is just one of many providers that we can use with Terraform. We could just as well create cloud resources in GCP (Google Cloud Provider), Microsoft Azure, and many others.  For a full list of providers, see here: https://registry.terraform.io/browse/providers


Now we’ve specified the “who” - which is AWS, we still need to specify the “what” - ie what resource we want to create in AWS.  Since we want to create an EC2 instance in AWS, we will see the following in our main.tf file (that we will download shortly):


```code
resource "aws_instance" "app_server" {
  ami           = "ami-087c17d1fe0178315"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}

```

Resources are the most important element in the Terraform language. Each resource block describes one or more infrastructure objects (in our case an EC2 instance in AWS), such as virtual networks, compute instances, or higher-level components such as DNS records.

The first parameter required to create an EC2 instance is the AMI - amazon machine image,  An Amazon Machine Image (AMI) is a supported and maintained image provided by AWS that provides the information required to launch an instance. 

The second is the instance type.  Note that we are also giving it a tag so that we can name it.


Let's see how to install and apply the Terraform files so that we can create an EC2 resource via Terraform.
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

7. Verify the created resource in the AWS management console. Congrats! You have now seen IaC in action - using 
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
Each input variable accepted by a module (in our case the main.tf module) must be declared using a variable block.

As we did above, we can provide a default value if  no value is passed in and a description along with a type.

Notice that we have both a variables.tf file and a testing.tfvars file - what's the difference between these two?

The difference is this:
variables.tf are files where all variables are declared; these might or might not have a default value variable. 

tfvars are files where the variables are provided/assigned a value. You can clearly see that there is a value being assigned in this file.  This is somewhat similar to setting an environment variable.

Great, so we've seen the file that holds the variable, the file that holds the value, but where do we actually use the variable?  Well, that's in the main.tf file - you can clearly see that here (excerpted from the main.tf file):

```bash
  tags = {
    Name = var.instance_name
  }
```

So, how do we create the resource while also passing in a value for the variable?  Well, simply run this statement:

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


