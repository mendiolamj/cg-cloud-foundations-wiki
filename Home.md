1. Under Search for services, type in and then click EC2.
1. In the left-hand EC2 Dashboard navigation menu, click Instances.
1. Click on the Launch Instances button.
1. Under Step 1, click Select against Amazon Linux 2 AMI (HVM).
1. Under Step 2, select t2.micro and click on the Next button at the bottom.
1. Under Step 3, select Default for Network.
1. Select any Subnet in the Subnet dropdown. 
1. For Auto-assign Public IP, select Enable.
1. Click on the Next button at the bottom.
1. Under Step 4, click on the Next button at the bottom.
1. Under Step 5, add a tag with key = Name & value = Test Instance. Click on the Next button at the bottom.
1. Under Step 6, Create a new Security Group and it should have rules for port 22 & 80 incoming from anywhere (0.0.0.0/0). Click on the Review & Launch button at the bottom.
1. Under Step 7, Click on Launch button. 
1. Create a new key pair, give it a name and download it. After that click on the Launch Instances button. 
1. Select the launched instance and press on the Connect button. Execute the following command after you have SSHed into the instance (to install the web server and then start the service). 
1. sudo yum install -y httpd
1. sudo systemctl enable httpd
1. sudo service httpd start
1. In a browser window, try to access the website using the EC2’s Public IP or Public Hostname (on port 80). Ensure that you use the HTTP url.  
1. Stop this instance and then create an AMI of it. Select the instance and explore the Actions > Image & Templates > Create Image button at the top. 
1. Navigate to AMIs from the left hand menu. 
1. Once the AMI is Available, select it and launch another instance from the AMI using the button given above (after this, follow the above steps 5 to 11). 
1. Check if all your changes persist in the new EC2 instance? (i.e. Webserver installation). You can check this by opening the new instance’s Public IP/hostname from a browser. 
1. After completing the above steps, terminate all the EC2 instances. It will disappear from the console after an hour.
1. Look at the menu items on the left side for AMI, Volumes & Snapshots. 
1. Deregister all the AMIs.
1. Delete all the snapshots from your account.
1. Delete all the volumes from your account. 
