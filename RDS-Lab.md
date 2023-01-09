
This is an overview of the RDS lab that we will be doing:

<img width="576" alt="Screen Shot 2023-01-09 at 12 45 10 AM" src="https://user-images.githubusercontent.com/25653204/211247227-7a4b1d47-8506-4343-be4e-6e07fae88153.png">

And here are the steps to follow for the lab:

**Subnet Group**
AWS gets to know the subnets in which the 2 RDS nodes should be launched (via Subnet Group). 
1. Under Search for services, type in and then click RDS.
1. In the left-hand RDS Dashboard navigation menu, click Subnet Groups.
1. Delete the Subnet Groups (if there are any). 
1. Click on the Create DB Subnet Group button. 
1. Give the name myRDSSubnetGroup. Provide a description.
1. For VPC, choose MyProdVPC.
1. Select both the Private Subnets of your VPC in the section below. 
1. Click on the Create button. 

**DB Security Group**
This security group protects the traffic coming to the RDS instance.
1. Navigate to EC2 Dashboard > Security Groups. 
1. Create a new Security Group for your RDS.
1. Give the name as RDSsg.
1. Choose the VPC as your myProdVPC. 
1. In the Inbound Rules, 
1. Specify Port as 5432 and source as the Security Group ID of your Public EC2 Instance. You can find this by opening a new tab for EC2 instances.
1. Click on the Create button. 

**DB Instance**
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

```console
sudo yum upgrade -y
sudo amazon-linux-extras install postgresql10 -y
mkdir rds-lab
cd rds-lab
wget https://usaa-aws-resources.s3.amazonaws.com/rds-lab/database.ini
wget https://usaa-aws-resources.s3.amazonaws.com/rds-lab/query_postgres.py
wget https://usaa-aws-resources.s3.amazonaws.com/rds-lab/requirements.txt 
wget https://usaa-aws-resources.s3.amazonaws.com/rds-lab/db_config.py 
pip3 install boto3 
pip3 install --upgrade -r requirements.txt
```

4. Now connect to RDS and create a table and insert data into it. Find the hostname from RDS details. Look for the Endpoint value.  You need to remove the square brackets in the following command:

```console
psql -h [HOSTNAME] -p [PORT] -U [USERNAME] -W -d [DATABASENAME]

create table film (title varchar(100) primary key, genre varchar (50), release_year varchar (5));

insert into film (title, genre, release_year) values ('matrix 1', 'scifi', 1998), ('matrix 2', 'thriller', 2002);

select * from film;

#Exit from psql 
\q
```

5. Update database.ini to show the info of your RDS. You can use vi editor. You will have to edit the hostname & password (provided you followed the instructions above). 
6. Execute the following to connect to RDS via python code. 
python3 ./query_postgres.py --instance master 
7. You should see the data from the table as part of the above python code execution. This part is completed now. 

**Test the Multi-AZ instance Failover**

1. Check the private IP of the RDS instance serving you. Make a note of it. 
```console
ping -a <rds hostname>
```
2. Now, reboot the RDS and choose Reboot with Failover option. Wait for DB to be available again. Check the private IP again and make a note of it. 
```console
ping -a <rds hostname>
```
3. Go ahead and check these IPs in the EC2 dashboard > Network Interfaces. 
4. Stop the EC2 instance now. 
5. Delete the RDS instance & Subnet Group now. 



<img width="603" alt="Screen Shot 2023-01-09 at 12 52 32 AM" src="https://user-images.githubusercontent.com/25653204/211247851-41e773df-88e3-49da-abed-e939ad12a32c.png">


