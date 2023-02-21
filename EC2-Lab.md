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

The key should go into your Downloads folder on your participant virtual machine.


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

15. Then click on the SSH Client tab: 

<img width="889" alt="Screen Shot 2023-02-21 at 7 51 48 AM" src="https://user-images.githubusercontent.com/25653204/220349498-3d7988ea-1c0a-4bc9-9fab-03d3baab677a.png">


16. From your participant machine, open up a terminal instance, then go into the directory where your key was downloaded by typing "cd ~/Downloads"

17.  Then, run the chmod command that it tells you to run in step 3 - note that your command will look slightly different since the name of your file is different:


<img width="859" alt="Screen Shot 2023-02-21 at 7 56 18 AM" src="https://user-images.githubusercontent.com/25653204/220350497-427f8812-03c9-402e-bae9-544cc6ad4e46.png">

18.  Now, run the command under where it says Example - note that it's easy to copy and paste the command into your terminal window by simply selecting the icon with 2 boxes:

<img width="854" alt="Screen Shot 2023-02-21 at 7 57 20 AM" src="https://user-images.githubusercontent.com/25653204/220350717-559531c3-5a65-4858-9244-0cffb674327a.png">

19. Now that you are connected to the EC2 instance, we are going to install a web server called Apache.  The web server uses packages called httpd, so we will run these commands one by one in your terminal:

```bash
sudo yum install -y httpd
sudo systemctl enable httpd
sudo service httpd start
```


20.  The commands in step 19 should install and start the Apache web server.  Then, to check on the status of it, you can run `sudo service httpd status` and you should see something like this:

<img width="1056" alt="Screen Shot 2023-02-21 at 8 03 32 AM" src="https://user-images.githubusercontent.com/25653204/220352365-f984f4b6-6401-4daf-a1d5-3c03bdd01a3f.png">

21. Navigate back to your EC2 instance and you should see: 
<img width="1452" alt="Screen Shot 2023-02-21 at 8 06 25 AM" src="https://user-images.githubusercontent.com/25653204/220352995-3ab6bdef-ac55-452f-b066-b3fb07215604.png">

On the bottom left where it says `Auto-assigned IP address` select that and paste it into the browser window.  You should then see this page - which is the Apache(httpd) web server that we installed on the machine:


<img width="1423" alt="Screen Shot 2023-02-21 at 8 07 25 AM" src="https://user-images.githubusercontent.com/25653204/220353148-346f4e6e-0e33-40ee-b64a-2c13afe27ad7.png">

22. Stop this instance by going to Instance state -> Stop instance 

23. Now, we will create an AMI from this instance by going to Actions-> Image and templates -> Create Image :   


<img width="1466" alt="Screen Shot 2023-02-21 at 8 13 15 AM" src="https://user-images.githubusercontent.com/25653204/220354319-60666b4c-4179-4acd-9473-eccd64757776.png">



24. Give the image a name -  "You name here image", ie "Jane Doe image".  Leave all of the defaults alone and click the "Create image" button on the bottom right.

25. On the left menu in the EC2 dashboard, you will see a section that says images.  Click on that and then AMIs.  

<img width="1440" alt="Screen Shot 2023-02-21 at 8 19 44 AM" src="https://user-images.githubusercontent.com/25653204/220355694-3f15aefc-f43b-448a-96ea-102c7a9458f0.png">

Then, click on the AMI ID and then the Launch instance from AMI button on the top right:

<img width="1448" alt="Screen Shot 2023-02-21 at 8 20 55 AM" src="https://user-images.githubusercontent.com/25653204/220355949-26d40a78-f1b2-4073-98ed-0df01cfdab5b.png">

26. As before, put in a name, leave the default alone for the section that says "Application and OS Images" - it should have the AMI name that you just created as the default, like so:

<img width="807" alt="Screen Shot 2023-02-21 at 9 15 53 AM" src="https://user-images.githubusercontent.com/25653204/220369455-641ecf7a-823a-4b38-a982-43a1756d9f2d.png">

27. For the key pair section, reuse the same key pair that you used before (do not use the one with the name test as shown in the screenshot below):

<img width="825" alt="Screen Shot 2023-02-21 at 9 17 27 AM" src="https://user-images.githubusercontent.com/25653204/220369829-9a289367-22c1-4139-b3fd-7252195f4ec3.png">

28.  Again, in network settings check the boxes for allow https traffic from the internet and http traffic from the internet:
<img width="813" alt="Screen Shot 2023-02-21 at 9 18 57 AM" src="https://user-images.githubusercontent.com/25653204/220370180-99e72248-e0e3-4a79-867e-3613e8aeb05b.png">


29. For Configure storage and Advanced details sections leave the defaults alone - no changes necessary.

30.  Finally, click the Launch instance orange button on the bottom right.

31.  Now, the question is do you have Apache already installed in the new EC2 instance?  You should by default!  Visit the public IP from a browser and you should see the Apache red Test page.

But remember, we did NOT install Apache manually for this EC2 instance - the reason it came with the instance is because we created our very own AMI with Apache pre-installed.  Pretty cool right? 

32. Go ahead and terminate all of your EC2 instances and deregister your AMI
