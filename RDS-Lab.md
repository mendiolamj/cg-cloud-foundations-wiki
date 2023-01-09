Subnet Group
AWS gets to know the subnets in which the 2 RDS nodes should be launched (via Subnet Group). 
1. Under Search for services, type in and then click RDS.
1. In the left-hand RDS Dashboard navigation menu, click Subnet Groups.
1. Delete the Subnet Groups (if there are any). 
1. Click on the Create DB Subnet Group button. 
1. Give the name myRDSSubnetGroup. Provide a description.
1. For VPC, choose MyProdVPC.
1. Select both the Private Subnets of your VPC in the section below. 
1. Click on the Create button. 

DB Security Group
This security group protects the traffic coming to the RDS instance.
1. Navigate to EC2 Dashboard > Security Groups. 
1. Create a new Security Group for your RDS.
1. Give the name as RDSsg.
1. Choose the VPC as your myProdVPC. 
1. In the Inbound Rules, 
1. Specify Port as 5432 and source as the Security Group ID of your Public EC2 Instance. You can find this by opening a new tab for EC2 instances.
1. Click on the Create button. 

DB Instance
1. In the left-hand RDS Dashboard navigation menu, click Databases.
1. Click on the Create Database button. 
1. Choose the Standard create radio-button.
1. Choose PostgreSQL engine type.
1. For Availability & Durability, select Multi-AZ DB instance.
1. Make note of the Master username. 
1. Provide a strong Master password. Make note of this as well in a notepad. 
1. For DB Instance class, choose Burstable classes and select db.t3.micro. 
1. For Storage, choose gp2. 
1. Under Connectivity, choose myProdVPC in the network. 
1. Select the Subnet Group you created in the previous step. 
1. For Security Group, remove the default security group. Add RDSsg.
1. Under Additional Configuration, 
* For Initial database name, provide mydb.
* Uncheck Enable Performance Insights. 
* Uncheck Enable Enhanced Monitoring. 
* Uncheck Enable deletion protection. 
14. Click on the Create Database button at the bottom. 


**Test the Database Connectivity**
1. Once RDS is ready, connect it via the Public instance you had launched earlier.
1. Start the Public EC2 Instance and Connect to it. 
1. Execute the following:

`sudo yum upgrade -y
sudo amazon-linux-extras install postgresql10 -y
mkdir rds-lab
cd rds-lab
wget https://usaa-aws-resources.s3.amazonaws.com/rds-lab/database.ini
wget https://usaa-aws-resources.s3.amazonaws.com/rds-lab/query_postgres.py
wget https://usaa-aws-resources.s3.amazonaws.com/rds-lab/requirements.txt 
wget https://usaa-aws-resources.s3.amazonaws.com/rds-lab/db_config.py 
pip3 install boto3 
pip3 install --upgrade -r requirements.txt`




