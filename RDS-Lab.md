
This is an overview of the RDS lab that we will be doing:

You will learn how to create an environment to run your MySQL database (we call this environment an instance), connect to the database, run some SQL queries and delete the DB instance.  We will do this using [Amazon Relational Database Service (Amazon RDS)](https://aws.amazon.com/rds/mysql/) and everything done in this lab is [Free Tier](https://aws.amazon.com/free/) eligible.

When you [click here](https://console.aws.amazon.com/console/home?region=us-east-1), the AWS management console will open in a new browser window, so you can keep this step-by-step guide open. When this screen loads, find RDS under Database and click to open the Amazon RDS Console.

###  **Steps to Create a MySQL Database instance**:

_In this step, we will use Amazon RDS to create a MySQL DB Instance with db.t2.micro DB instance class, 20 GB of storage, and automated backups enabled with a retention period of one day. As a reminder, all of this is free tier eligible._

1. In the top right corner of the Amazon RDS console, select the Region in which you want to create the DB instance. (Please use us-west-2)
2. In the Create database section, choose Create database.
3. You now have options to select your engine.  For this tutorial, click the MySQL icon, leave the default value of edition and engine version, and 
   select the Free Tier template.
4. You will now configure your DB instance. The list below shows the example settings you can use for this lab:
  
 **Settings:**

* _**DB instance identifier: Type a name for the DB instance that is unique for your account in the Region that you selected. For this tutorial, you can use rds-mysql-Exercise-YourName.**_
* _**Master username: Type a username that you will use to log in to your DB instance. You can "YourNameRDS" in this example.**_
* _**Master password: Type a password that contains from 8 to 41 printable ASCII characters (excluding /,", and @) for your master user password.**_
* _**Confirm password: Retype your password**_

5. **Instance specifications:**
* DB instance class: Select db.t2.micro --- 1vCPU, 1 GIB RAM. This equates to 1 GB memory and 1 vCPU. To see a list of supported instance classes, see [Amazon RDS Pricing](https://aws.amazon.com/rds/pricing/).
* Storage type: Select General Purpose (SSD). For more information about storage, see [Storage for Amazon RDS](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Storage.html).
* Allocated storage: Select the default of 20 to allocate 20 GB of storage for your database. You can scale up to a maximum of 64 TB with Amazon RDS for MySQL.
* Enable storage autoscaling: If your workload is cyclical or unpredictable, you would enable storage autoscaling to enable RDS to automatically scale up your storage when needed. This option does not apply to this tutorial.
* Multi-AZ deployment: Note that you will have to pay for Multi-AZ deployment. Using a Multi-AZ deployment will automatically provision and maintain a synchronous standby replica in a different Availability Zone. For more information, see [High Availability Deployment](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html).

6. You are now on the Connectivity section where you can provide information that RDS needs to launch your MySQL DB instance. The list below shows settings for our example DB instance.

**Connectivity**

* Virtual Private Cloud (VPC): Select Default VPC. For more information about VPC, see [Amazon RDS and Amazon Virtual Private Cloud (VPC)](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.RDSVPC.html).

**Additional connectivity configurations**
* Subnet group: Choose the default subnet group. For more information about subnet groups, see [Working with DB Subnet Groups](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html#USER_VPC.Subnets).
* Public accessibility: Choose Yes. This will allocate an IP address for your database instance so that you can directly connect to the database from your own device.
* VPC security groups: Select Create new VPC security group. This will create a security group that will allow connection from the IP address of the device that you are currently using to the database created.
* Availability zone: Choose No preference. See [Regions and Availability Zones](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html) for more details.
* Port: Leave the default value of 3306.

**In the Additional configurations section:**

**Database options**

* Database name: Type a database name that is 1 to 64 alpha-numeric characters. If you do not provide a name, Amazon RDS will not automatically create a database on the DB instance you are creating.
* DB parameter group: Leave the default value. For more information, see [Working with DB Parameter Groups](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html).
* Option group: Leave the default value. Amazon RDS uses option groups to enable and configure additional features. For more information, see [Working with Option Groups](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithOptionGroups.html).

**Encryption**
* This option is not available in the Free Tier. For more information, see [Encrypting Amazon RDS Resources](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html). 

**Backup**

* Backup retention period: You can choose the number of days to retain the backup you take. For this tutorial, set this value to 1 day.
* Backup window: Use the default of No preference.

**Monitoring**

* Enhanced Monitoring: Select Disable enhanced monitoring to stay within the free tier. Enabling enhanced monitoring will give you metrics in real time for the operating system (OS) that your DB instance runs on. For more information, see [Viewing DB Instance Metrics](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Monitoring.html).

**Maintenance**

**Auto minor version upgrade: Select Enable auto minor version upgrade to receive automatic updates when they become available.**
**Maintenance Window: Select No preference.**
**Deletion protection**
**Clear Enable deletion protection for this tutorial. When this option is enabled, you're prevented from accidentally deleting the database.**

7. Click **Create Database.** 
8. Your DB Instance is now being created.  Click **View Your DB Instances.**


**CONNECTING TO THE DB Instance:**

In order to connect to the DB Instance and access your MYSQL Database - follow the below steps:

1. Launch an EC2 machine [Refer EC2 Lab](https://github.com/varoonsahgal/cg-cloud-foundations/wiki/EC2-Lab) 
2. SSH to the EC2 machine.
3. On AMI command line, first-run system update using this syntax : `yum apt update`
4. Now, install MySQL client on Amazon Linux 2 using this syntax- `yum install mysql`
5. Check installed MySQL/MariaDB version using this syntax– `mysql --version`
6. Type in the following syntax to access your database:

`$ mysql -h {hostname} -P 3306 -u username -p {databasename}`

`Password: {your password}`

> hostname: the name of the MySQL server that you are assigned to, for example, mysql4.safesecureweb.com
> databasename: the name of your MySQL database
> password: the password you use to access your MySQL database

7. On successful connection the terminal opens to MySQL Command Line. 
8. Run some MySQL queries as mentioned below:

* To create a database with a name employee using this query - `create database employee;`
* SELECT Database is used in MySQL to select a particular database to work with. This query is used when multiple databases are available with MySQL Server. `use employee; ` 
* Create a table - `CREATE TABLE employee_table(  
    id int NOT NULL AUTO_INCREMENT,  
    name varchar(45) NOT NULL,  
    occupation varchar(35) NOT NULL,  
    age int NOT NULL,  
    PRIMARY KEY (id)  
);  `

*  If we want to store single records for all fields - 

`INSERT INTO employee_table (id, name, occupation, age) VALUES (101, 'Peter', 'Engineer', 32);`  

* If we want to store multiple records, use the following statements where we can either specify all field names or don't specify any field -

`INSERT INTO People VALUES  `
`(102, 'Joseph', 'Developer', 30),  `
`(103, 'Mike', 'Leader', 28),  `
`(104, 'Stephen', 'Scientist', 45); `

* We can use the below query to show the records of the employee_table:
`SELECT * FROM employee_table;  `

9. Type **exit** to close the SQL command line. 

10. Once you have tested the SQL command, close all the terminals on the EC2 machine. 

11. Delete the EC2 machine - 

_Before you terminate an instance, verify that you won't lose any data by checking that your Amazon EBS volumes won't be deleted on termination and that you've copied any data that you need from your instance store volumes to persistent storage, such as Amazon EBS or Amazon S3._

> Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.

> In the navigation pane, choose Instances.

> Select the instance, and choose Instance state, Terminate instance.

> Choose Terminate when prompted for confirmation.



**### Delete the DB Instance**

_You can easily delete the MySQL DB Instance from the Amazon RDS console. It is a best practice to delete instances that you are no longer using so that you don’t keep getting charged for them._

1. Go back to your Amazon RDS Console. Select **Databases**, choose the instance that you want to delete, and then select **Delete** from the **Actions** dropdown menu.
2. You are asked to create a final snapshot and to confirm the deletion. For our example, do not create a final snapshot, acknowledge that you want to delete the instance, and then click **Delete.** 

_Note: Deleting your DB Instance may take a few minutes_
