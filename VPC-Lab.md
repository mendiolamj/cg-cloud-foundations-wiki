VPC LAB OVERVIEW:

<img width="574" alt="Screen Shot 2023-03-01 at 6 18 51 AM" src="https://user-images.githubusercontent.com/25653204/222124829-959253da-2b09-491e-b048-faea3c787808.png">


## VPC & Subnets
In this section, we will create a new VPC and subnets in it. 
Log in to the AWS Console using the provided Username, Password.
Note: Now and throughout this lab, ensure that in the upper-right of the screen, N. Virginia (US East) is your selected region.
Under Search for services, type in and then click VPC.
In the left-hand VPC Dashboard navigation menu, click Your VPCs.
Click on the Create VPC button.
On the Create VPC page:
For Name tag, provide MyProdVPC.
For IPv4 CIDR block, provide 10.0.0.0/26.
Scroll down and click on the Create VPC button. 
In the left-hand VPC Dashboard navigation menu, click Subnets.
Click on the Create subnet button.
On the Create subnet page:
For VPC ID, choose MyProdVPC.
Under Subnet settings > Subnet 1 of 1, for Subnet name	provide public-subnet-a.
For Availability Zone, choose us-east-1a.
For IPv4 CIDR block, provide 10.0.0.0/28.
Click on the Add new subnet button.
Under Subnet 2 of 2, for Subnet name	provide private-subnet-a.
For Availability Zone, choose us-east-1a.
For IPv4 CIDR block, provide 10.0.0.16/28.
Click on the Add new subnet button.
Under Subnet 3 of 3, for Subnet name	provide public-subnet-b.
For Availability Zone, choose us-east-1b.
For IPv4 CIDR block, provide 10.0.0.32/28.
Click on the Add new subnet button.
Under Subnet 4 of 4, for Subnet name	provide private-subnet-b.
For Availability Zone, choose us-east-1b.
For IPv4 CIDR block, provide 10.0.0.48/28.
Click on the Create subnet button at the bottom of the page. 
You should see a success message for subnet creation and the two subnets should be visible now. 

## Internet Gateway
In this section, we will create an Internet Gateway for our VPC. This will enable connectivity of our VPC to the Internet. 
In the left-hand VPC Dashboard navigation menu, click Internet Gateways.
Click on the Create internet gateway button.
For Name tag, provide prod-igw. 
Click on the Create internet gateway button at the bottom.
Upon successful creation of Internet Gateway, select prod-igw and click on Actions > Attach to VPC. 
For Available VPCs, select MyProdVPC.
Click on the Attach internet gateway button. 
You should see a success message for this action. 


## Public and Private Route Tables
In the left-hand VPC Dashboard navigation menu, click Route Tables.
Click on the Create route table button.
For Name - optional, provide public-rt.
For VPC, choose MyProdVPC.
Click on the Create route table button at the bottom.
Once the public-rt is created successfully, in the Routes tab, click on the Edit routes button.
Under Edit routes, click on the Add route button.
For Destination, provide 0.0.0.0/0. 
For Target, choose Internet Gateway and select prod-igw.
Click on the Save changes button.
Under the Subnet associations tab, click on the Edit subnet associations button. 
Check both the public subnets and click on the Save associations button.

The 2 public subnets now have access to the internet via Internet Gateway through this route table. 

In the left-hand VPC Dashboard navigation menu, click NAT Gateways.
Click on the Create NAT Gateway button. 
For Name - optional, provide myNAT.
For Subnet, select one of the two public subnets from your VPC. 
Click on the Allocate Elastic IP button. 
Scroll down and click on the Create NAT Gateway button. 
Your NAT Gateway will become available in a few minutes.  
In the left-hand VPC Dashboard navigation menu, click Route Tables.
Click on the Create route table button.
For Name - optional, provide private-rt.
For VPC, choose MyProdVPC.
Click on the Create route table button at the bottom.
Once the private-rt is created successfully, in the Routes tab, click on the Edit routes button.
Under Edit routes, click on the Add route button.
For Destination, provide 0.0.0.0/0. 
For Target, choose NAT Gateway and select myNAT.
Click on the Save changes button.
Under the Subnet associations tab, click on the Edit subnet associations button. 
Check both the private subnets and click on the Save associations button.
This route table will help the instances in the private subnet to utilize the NAT Gateway to access the internet.

This part gets completed with successful creation of both route tables and adding the required routes.

## Launch a Public Instance
We will now launch an EC2 instance in the Public Subnet. 
Under Search for services, type in and then click EC2.
In the left-hand EC2 Dashboard navigation menu, click Instances.
Click on the Launch Instances button.
Under Step 1, click Select against Amazon Linux 2 AMI (HVM).
Under Step 2, select t2.micro and click on the Next button at the bottom.
Under Step 3, select MyProdVPC for Network.
Select a Public Subnet in the Subnet dropdown. 
For Auto-assign Public IP, select Enable.
Click on the Next button at the bottom.
Under Step 4, click on the Next button at the bottom.
Under Step 5, add a tag with key = Name & value = Public Instance. Click on the Next button at the bottom.
Under Step 6, Create a new Security Group and it should have rules for port 22 & 80 incoming from anywhere (0.0.0.0/0). Click on the Review & Launch button at the bottom.
Under Step 7, Click on Launch button. 
Create a new key pair, give it a name and download it. After that click on the Launch Instances button. 

## Launch a Private Instance
We will now launch an EC2 instance in the Private Subnet. 
Under Search for services, type in and then click EC2.
In the left-hand EC2 Dashboard navigation menu, click Instances.
Click on the Launch Instances button.
Under Step 1, click Select against Amazon Linux 2 AMI (HVM).
Under Step 2, select t2.micro and click on the Next button at the bottom.
Under Step 3, select MyProdVPC for Network.
Select a Private Subnet in the Subnet dropdown. 
For Auto-assign Public IP, select Disable.
Click on the Next button at the bottom.
Under Step 4, click on the Next button at the bottom.
Under Step 5, add a tag with key = Name & value = Private Instance. Click on the Next button at the bottom.
Under Step 6, Create a new Security Group and it should have rules for port 22 & 80 incoming from anywhere (0.0.0.0/0). Click on the Review & Launch button at the bottom.
Under Step 7, Click on Launch button. 
Create a new key pair, give it a name and download it. After that click on the Launch Instances button. 

## Final Testing: 
Upload the PEM file for your private instance to an S3 bucket (you can create a new bucket). 
Click on the bucket name, click on the Permissions tab and navigate to the Block public access section. Edit it and turn it Off.
Then, make this file public and copy its public URL. (select the file, go to Actions and click on Make public via ACL). Click on the file to get its Object URL. This is the public URL for your file. 
Now, connect to the Public Instance and access google.com. You have to select the Public Instance and click on the Connect button. Then, click Connect again. (You can execute “wget https://google.com”)
You can download the PEM file (of private instance) on the Public instance using this command “wget <public_URL>”

After that SSH into Private Instance and try to access google.com. See the following instructions:
chmod 400 <private_instance.pem>
ssh -i "16Nov2021.pem" ec2-user@<private-ip-of-private-instance>

Verify if the internet is accessible on both the instances! (You can execute “wget https://google.com”).
Now, delete the NAT Gateway and then release the Elastic IP (navigate from the left hand menu to Elastic IP, you will have to wait for a few minutes). Check again if the internet is accessible on both the instances!

The private instance should not be able to access the internet now. Public instance should still be able to access the internet via IGW.  

Now, stop the Public Instance and terminate the Private Instance. 

