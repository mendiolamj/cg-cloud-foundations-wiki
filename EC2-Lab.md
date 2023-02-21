_Here's a high level summary of what we want to accomplish with this lab:_

<img width="579" alt="Screen Shot 2023-01-08 at 11 21 15 PM" src="https://user-images.githubusercontent.com/25653204/211240643-e63c48ff-565e-470d-8259-c53e6b95aec1.png">

_And here are the steps to follow to complete the lab:_

1. Under Search for services, type in and then click EC2.
2. In the left-hand EC2 Dashboard navigation menu, click Instances.
3. Click on the orange Launch Instances button.

<img width="1251" alt="Screen Shot 2023-02-20 at 9 22 55 PM" src="https://user-images.githubusercontent.com/25653204/220231469-a843e690-234e-47d9-b91f-73b74bf146f7.png">

4. Put in a name - you can call it "put your name here's EC2 instance" - for example "Andrew Jones' EC2 instance"

<img width="914" alt="Screen Shot 2023-02-20 at 9 24 36 PM" src="https://user-images.githubusercontent.com/25653204/220231682-2bcbc94d-750b-44be-9030-21d5f01acf04.png">

5. Under "Application and OS Images (Amazon Machine Image)", leave the defaults alone - so it should look something like this (with Amazon Linux and the Amazon Linux 2 AMI selected):

<img width="841" alt="Screen Shot 2023-02-20 at 9 25 47 PM" src="https://user-images.githubusercontent.com/25653204/220231853-7fc9c415-d174-45d0-a472-e973a5536240.png">


6. Under instance type, leave the default alone, so it looks like this:
<img width="856" alt="Screen Shot 2023-02-20 at 9 27 00 PM" src="https://user-images.githubusercontent.com/25653204/220232018-b2d0ec31-05f0-4c7a-865e-8441b6f24ac0.png">

7. Under key pair, go ahead and put in "put your name here key pair" - so "Andrew Smith key pair" for example.  The key pair is how we connect to our EC2 instance.  We do not want to let just anyone connect to anyone's EC2 instance, so think of the key pair like house keys for your EC2 instance:

<img width="864" alt="Screen Shot 2023-02-20 at 9 31 36 PM" src="https://user-images.githubusercontent.com/25653204/220232565-7a3f8a55-b4da-4902-b237-9eb5b0ec1f84.png">


8. Under Network Settings, leave all of the defaults alone EXCEPT you should check the boxes that say "Allow HTTPs traffic from the internet" and "Allow HTTP traffic from the internet". So it should look like this:

<img width="829" alt="Screen Shot 2023-02-21 at 7 19 50 AM" src="https://user-images.githubusercontent.com/25653204/220343383-37c8e110-42ee-4057-8d4d-0f789204e209.png">

9.  Leave all the other defaults alone for the "Configure Storage" and "Advanced details" sections - so no changes are necessary there:
<img width="848" alt="Screen Shot 2023-02-21 at 7 21 55 AM" src="https://user-images.githubusercontent.com/25653204/220343775-6faea675-24f9-4796-b833-29272235beb2.png">

10. For the summary, leave all the defaults alone as well:


<img width="840" alt="Screen Shot 2023-02-21 at 7 28 15 AM" src="https://user-images.githubusercontent.com/25653204/220345059-c4baef62-760d-4f8e-87c1-f579cf3744b8.png">


11.  Finally, click the Launch Instance button at the bottom right to launch your EC2 instance!:

<img width="843" alt="Screen Shot 2023-02-21 at 7 29 15 AM" src="https://user-images.githubusercontent.com/25653204/220345269-78814e9d-3555-4030-908d-b5f7b34d22b5.png">

12. Once you launch, in the EC2 dashboard, click Instances on the left hand menu:

<img width="1251" alt="Screen Shot 2023-02-21 at 7 36 08 AM" src="https://user-images.githubusercontent.com/25653204/220346575-1b0a2bb8-883e-4841-8e57-5ab7d9cb8c67.png">

13.  Then, click on the instance ID and you should get a page that looks like this:

<img width="1222" alt="Screen Shot 2023-02-21 at 7 37 01 AM" src="https://user-images.githubusercontent.com/25653204/220346746-6c9f1885-7c36-438a-861a-4b3dc8cb9d8c.png">



14. Go ahead and click on the Connect button

15. Then click on the EC2 Instance Connect tab: 

<img width="956" alt="Screen Shot 2023-02-21 at 7 37 48 AM" src="https://user-images.githubusercontent.com/25653204/220346893-93d27c14-d369-418b-8626-32992a770bed.png">

16. Once that connects, you should get a page that looks like this: 



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
