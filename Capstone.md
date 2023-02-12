* In our capstone, we will bring together a lot of the concepts that we've explored thus far in the class.



* We will start with an existing React application that simply maintains a todo list - see here: https://github.com/varoonsahgal/rrf-todo/tree/lab3-with-axios


* Our goal will be to Dockerize/containerize the React application and then set it up for CI/CD utilizing AWS services


* The flow will look like something like this:
<img width="773" alt="Screen Shot 2023-01-20 at 12 04 34 PM" src="https://user-images.githubusercontent.com/25653204/213760116-406c4617-8b29-429d-a6d7-65e8ec7df110.png">



* We will load the initial todos that get displayed in the application by hitting an API which utilizes a Lambda that will then retrieve the initial todos from an S3 bucket (which you populate)
 
* The first thing you will need to do is containerize the application by using a Dockerfile and perhaps use Docker compose as well

* Then, setup CI/CD with Codebuild + Codepipeline - upload the image to ECR, and set it up such that every time you update the github repository the Docker image is rebuilt and redeployed.

* Redeployed where though?  Good question -  you will need to deploy your container to an orchestrator - you will use Fargate + EKS.  Set that up as well!

* Finally you will utilize Terraform to manage our infrastructure.  For high availability and disaster recovery, you will deploy our application to 2 regions.  We will also want our S3 buckets to live in 2 different regions as well.  

