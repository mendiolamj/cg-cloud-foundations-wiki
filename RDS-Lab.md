his is an overview of the RDS lab that we will be doing:

You will learn how to create an environment to run your MySQL database (we call this environment an instance), connect to the database, run some SQL queries and delete the DB instance.  We will do this using [Amazon Relational Database Service (Amazon RDS)](https://aws.amazon.com/rds/mysql/) and everything done in this lab is [Free Tier](https://aws.amazon.com/free/) eligible.

When you [click here](https://console.aws.amazon.com/console/home?region=us-east-1), the AWS management console will open in a new browser window, so you can keep this step-by-step guide open. When this screen loads, find RDS under Database and click to open the Amazon RDS Console.

###  **Steps to Create a MySQL Database instance**:

_In this step, we will use Amazon RDS to create a MySQL DB Instance with db.t2.micro DB instance class, 20 GB of storage, and automated backups enabled with a retention period of one day. As a reminder, all of this is free tier eligible._

1. In the top right corner of the Amazon RDS console, select the Region in which you want to create the DB instance. (Eg: us-west-2.
2. In the Create database section, choose Create database.
3. You now have options to select your engine.  For this tutorial, click the MySQL icon, leave the default value of edition and engine version, and 
   select the Free Tier template.
4. You will now configure your DB instance. The list below shows the example settings you can use for this lab:
  
 **Settings:**

* _**DB instance identifier: Type a name for the DB instance that is unique for your account in the Region that you selected. For this tutorial, we will name it rds-mysql-10minTutorial.**_
* _**Master username: Type a username that you will use to log in to your DB instance. We will use masterUsername in this example.**_
* _**Master password: Type a password that contains from 8 to 41 printable ASCII characters (excluding /,", and @) for your master user password.**_
* _**Confirm password: Retype your password**_

5. **Instance specifications:**
* DB instance class: Select db.t2.micro --- 1vCPU, 1 GIB RAM. This equates to 1 GB memory and 1 vCPU. To see a list of supported instance classes, see [Amazon RDS Pricing](https://aws.amazon.com/rds/pricing/).
* Storage type: Select General Purpose (SSD). For more information about storage, see [Storage for Amazon RDS](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Storage.html).
* Allocated storage: Select the default of 20 to allocate 20 GB of storage for your database. You can scale up to a maximum of 64 TB with Amazon RDS for MySQL.
* Enable storage autoscaling: If your workload is cyclical or unpredictable, you would enable storage autoscaling to enable RDS to automatically scale up your storage when needed. This option does not apply to this tutorial.
* Multi-AZ deployment: Note that you will have to pay for Multi-AZ deployment. Using a Multi-AZ deployment will automatically provision and maintain a synchronous standby replica in a different Availability Zone. For more information, see [High Availability Deployment](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html).

6. You are now on the Connectivity section where you can provide information that RDS needs to launch your MySQL DB instance. The list below shows settings for our example DB instance.