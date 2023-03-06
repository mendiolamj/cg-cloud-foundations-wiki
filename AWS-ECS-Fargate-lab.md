# Overview
We will take the Apache httpd Docker image and deploy it to the ECS.

You can pull it to your VM if you don't already have it via `docker pull httpd`.  Then in the instructions below where we ask you to tag the image with the URI - use the httpd image you just pulled.


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


Put in a name for the cluster - like "Group1-cluster"

Under networking, go ahead and select the default VPC and select 2 subnets since we are just doing a training exercises (note that multiple subnets are for redundancy as they live in different availability zones).

Basically, by splitting across multiple subnets, during an outage load can be shifted to launch containers entirely in other subnets (assuming they're in different AZs). This is generally encouraged for all services that support multiple availability zones:

<img width="703" alt="Screen Shot 2023-03-05 at 9 24 52 PM" src="https://user-images.githubusercontent.com/25653204/223005273-d4f9240a-6645-43a5-a132-bcfda7786f46.png">


On the next view, we're asked why type of cluster template we would like to use. As we're going to be using Fargate to deploy our application, we only need the simplest of clusters, so select "Networking Only" and then click "Next Step"


After our cluster has been deployed, you'll be able to see it's managment page. At the moment, it's not doing very much at all, but now's the time where we put our container image to use.


## Creating a Task definition

In order to get our container image deployed on our shiny new cluster, we need to create "Task Definition". A task definition is a way to specify the container information for your application, such as how many containers are part of our task, what resources they will use, how they are linked together, and which host ports they will have access to.

In the pop out menu to the left of the screen, select "Task Definitions" and when the next view has loaded click "Create new Task Definition":

<img width="1547" alt="Screen Shot 2023-03-06 at 9 18 41 AM" src="https://user-images.githubusercontent.com/25653204/223135640-55a9f0df-0afd-4b14-9a09-f2a21784cdc4.png">


Enter in a Task definition family name such as group1_task. Next, set up the container details with a name such as group1container and use the Image URI of your ECR container. For port mappings, enter 80 as the value for our container port with the protocol set to TCP. This will enable us to communicate with our application on port 80.


<img width="709" alt="Screen Shot 2023-03-06 at 9 39 14 AM" src="https://user-images.githubusercontent.com/25653204/223141800-6c69cc3b-f714-4b06-bd13-0bdf0277f963.png">


Finally open the environment variables section and create a variable PORT with the value of 80. This will tell our application that it should listen for requests on that port, which makes sense as its the standard HTTP port:

<img width="722" alt="Screen Shot 2023-03-06 at 9 42 38 AM" src="https://user-images.githubusercontent.com/25653204/223142719-7307f79a-ed04-4746-b2d8-f13cf361cea6.png">

Go ahead and hit next.

Here we can leave the majority of our configuration options as their defaults. For task size, go ahead and set the minimum values to get started: Task CPU set to 0.5 vCPU and Task Memory set to 1 GB:

<img width="1003" alt="Screen Shot 2023-03-06 at 9 46 34 AM" src="https://user-images.githubusercontent.com/25653204/223143734-798147d0-0891-45c5-8a6b-067981b4b020.png">

Leave all the defaults alone on this page.  Go ahead and hit next and on the next page hit Create.


# Running our container

Now that we have a Task, we can now launch it in our Cluster - note that it does not run by default. Back on the Clusters home page, select the Task tab so we can run our task:



<img width="1566" alt="Screen Shot 2023-03-06 at 9 53 58 AM" src="https://user-images.githubusercontent.com/25653204/223145758-ba7cdbb6-6416-48c9-8d9e-33a0ead413a7.png">

In the next page, you can leave all the configuration options as their defaults. 

Go ahead and click create on the bottom.

You'll be taken to an overview of our cluster, and in the table at the bottom you should see your new task in the "Tasks" table with the status of "Provisioning". It may take a moment or two for the container to provision. To check press the refresh button and wait until the status changes to "Running".





## Aside on terminology:
A Task Definition is a collection of 1 or more container configurations. Some Tasks may need only one container, while other Tasks may need 2 or more potentially linked containers running concurrently. The Task definition allows you to specify which Docker image to use, which ports to expose, how much CPU and memory to allot, how to collect logs, and define environment variables.

A Task is created when you run a Task directly, which launches container(s) (defined in the task definition) until they are stopped or exit on their own, at which point they are not replaced automatically. Running Tasks directly is ideal for short-running jobs, perhaps as an example of things that were accomplished via CRON.

A Service is used to guarantee that you always have some number of Tasks running at all times. If a Task's container exits due to an error, or the underlying EC2 instance fails and is replaced, the ECS Service will replace the failed Task. This is why we create Clusters so that the Service has plenty of resources in terms of CPU, Memory and Network ports to use. To us it doesn't really matter which instance Tasks run on so long as they run. A Service configuration references a Task definition. A Service is responsible for creating Tasks.

Services are typically used for long-running applications like web servers. For example, if I deployed my website powered by Node.JS in Oregon (us-west-2) I would want say at least three Tasks running across the three Availability Zones (AZ) for the sake of High-Availability; if one fails I have another two and the failed one will be replaced (read that as self-healing!). Creating a Service is the way to do this. If I had 6 EC2 instances in my cluster, 2 per AZ, the Service will automatically balance Tasks across zones as best it can while also considering CPU, memory, and network resources.





