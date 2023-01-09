
Here's an overview of the lab:

<img width="588" alt="Screen Shot 2023-01-09 at 1 32 33 AM" src="https://user-images.githubusercontent.com/25653204/211251969-7fa5c3a7-f0d4-4f94-8059-72cec766e262.png">

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
