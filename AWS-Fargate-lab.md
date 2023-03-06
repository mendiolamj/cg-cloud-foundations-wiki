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

To tell Docker where to save our container image, we can tag it with the URI we just required.

Enter the following command into your CLI to tag your container image with our private ECR repository's URI (replace with the URI we just acquired):

```code
docker tag myimage <URI>
```

Now that we've tagged our image with it's ultimate destination, we need to give the Docker CLI tool limited access to our AWS account so that it can write our image to our repository.

We can do so with the following command:

```
aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin <REGISTRY_URI>
```

Take note of the two fields we need to replace in the above command. We need to specify the region our ECR Repository is located in - `us-west-2`, and we need to replace <REGISTRY_URI> up to the last slash - for example -  `962804699607.dkr.ecr.us-west-2.amazonaws.com/varoon-ecr-repository` (please do not use that).


If you entered the right parameters, you should see Login Succeeded in your terminal.

Now that we're logged in, we can simply use docker push to upload our container image to our repository.

```
docker push <URI>
```

You should then see the image uploaded in ECR:

<img width="1176" alt="Screen Shot 2023-03-05 at 8 16 28 PM" src="https://user-images.githubusercontent.com/25653204/222998540-95720474-7a14-45a9-a848-13961041b8c6.png">

## Creating a Cluster

In order to deploy our container, we need a cluster of computers to run it on. This is where the Amazon Elastic Container Service comes in.

Head to the [Amazon ECS Management Console ](https://console.aws.amazon.com/ecs/v2/clusters?region=us-west-2) and click "Create Cluster".



