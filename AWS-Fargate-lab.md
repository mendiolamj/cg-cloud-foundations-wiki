# Creating a private repository:

Navigate to the ECR service in the AWS console.

Be sure that Private is selected and click the orange button that says create repository:

<img width="1407" alt="Screen Shot 2023-03-05 at 7 27 22 PM" src="https://user-images.githubusercontent.com/25653204/222996654-4f950ad5-8fbd-4a6e-864a-2e689741bdae.png">

Leave all the defaults alone on the next page - except add a repository name with your breakout room number:

<img width="977" alt="Create repository" src="https://user-images.githubusercontent.com/25653204/222996668-1f5d8de5-cb1c-447c-8f13-5f3059422144.png">


Once your private repository has been created, we'll be taken back to the management page for our repository. You'll notice that our repository has been given a URI. We'll use this value to tell Docker where it should upload your container image.

Copy the URI to your clipboard and then head back to your CLI.  Of course, please be sure to create it the repo in the `us-west-2` region:


<img width="1035" alt="Screen Shot 2023-03-05 at 7 49 13 PM" src="https://user-images.githubusercontent.com/25653204/222996703-dacdf3bc-a2ca-4f71-8629-e92832334d13.png">

# Uploading container image to ECR:


