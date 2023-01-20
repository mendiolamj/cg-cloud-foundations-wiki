* In our capstone, we will bring together a lot of the concepts that we've explored thus far in the class.



* We will start with an existing React application that simply maintains a todo list - see here: https://github.com/varoonsahgal/rrf-todo/tree/lab2-ref-updated


* Our goal will be to Dockerize/containerize the React application and then set it up for CI/CD utilizing AWS services


* The flow will look like something like this:


<img width="775" alt="Screen Shot 2023-01-20 at 11 57 41 AM" src="https://user-images.githubusercontent.com/25653204/213757815-b812c1a5-a91c-47bc-bf93-c69c9fbcee7d.png">


* We will load the initial todos that get displayed in the application by hitting an API which utilizes a Lambda that will then retrieve the initial todos from an S3 bucket (which you populate)
 
* The first thing you will need to do is containerize the application by using a Dockerfile and perhaps use Docker compose as well

* Then, setup CI/CD with Codebuild + Codepipeline - upload the image to ECR, and set it up such that every time you update the github repository the Docker image is rebuilt

* Of course, you will need to deploy our container to an orchestrator - we will use Fargate + EKS.  Set that up as well!

* Finally you will utilize Terraform to manage our infrastructure.  For high availability and disaster recovery, we will deploy our application to 2 regions.  We will also want our S3 buckets to live in 2 different regions as well.  

