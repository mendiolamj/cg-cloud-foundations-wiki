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
In the left-hand RDS Dashboard navigation menu, click Databases.
Click on the Create Database button. 
Choose the Standard create radio-button.
Choose PostgreSQL engine type.
For Availability & Durability, select Multi-AZ DB instance.
Make note of the Master username. 
Provide a strong Master password. Make note of this as well in a notepad. 
For DB Instance class, choose Burstable classes and select db.t3.micro. 
For Storage, choose gp2. 
Under Connectivity, choose myProdVPC in the network. 
Select the Subnet Group you created in the previous step. 
For Security Group, remove the default security group. Add RDSsg.
Under Additional Configuration, 
For Initial database name, provide mydb.
Uncheck Enable Performance Insights. 
Uncheck Enable Enhanced Monitoring. 
Uncheck Enable deletion protection. 
Click on the Create Database button at the bottom. 
Test the Database Connectivity
Once RDS is ready, connect it via the Public instance you had launched earlier.
Start the Public EC2 Instance and Connect to it. 
Execute the following:
