* In our capstone, we will bring together a lot of the concepts that we've explored thus far in the class.



* We will start with an existing React application that simply maintains a todo list - see here: https://github.com/varoonsahgal/rrf-todo/tree/lab3-with-axios


* Our goal will be to Dockerize/containerize the React application and then set it up for CI/CD utilizing AWS services. The challenge for you is to write the Docker File and Terraform steps. You can consider Capstone 1 for reference! 


* The flow will look like something like this:
<img width="773" alt="Screen Shot 2023-01-20 at 12 04 34 PM" src="https://user-images.githubusercontent.com/25653204/213760116-406c4617-8b29-429d-a6d7-65e8ec7df110.png">



* We will load the initial todos that get displayed in the application by hitting an API which utilizes a Lambda that will then retrieve the initial todos from an S3 bucket (which you populate)
 
* The first thing you will need to do is containerize the application by using a Dockerfile.

* Then, setup CI/CD with Codebuild + Codepipeline - upload the image to ECR, and set it up such that every time you update the github repository the Docker image is rebuilt and redeployed.

* Redeployed where though?  Good question -  you will need to deploy your container to an orchestrator - you will use Fargate + EKS.  Set that up as well!

* Finally you will utilize Terraform to manage our infrastructure.  

_For high availability and disaster recovery, you will deploy our application to 2 AZ or regions.  We will also want our S3 buckets to live in 2 different regions/AZ as well.  _



Notes:
* For the React application, you will just have to update the endpoint on line 11 here: https://github.com/varoonsahgal/rrf-todo/blob/lab3-with-axios/src/actions.js and replace that endpoint with whatever is generated via AWS API gateway. 
* You may run into some CORS issues but resolve by setting the appropriate headers - note that you can also see there's a fake mock server locally in server/db.json file as well

Cross-Origin Resource Sharing (CORS) is a security feature that restricts the ability of web pages to make requests to resources located on a different domain. If you are getting CORS errors when trying to access an API hosted on Amazon API Gateway, you can try the following steps to fix the issue:

Configure API Gateway to return the necessary CORS headers:
Open the Amazon API Gateway console.
Select the desired API.

Go to the "Actions" menu, and select "Enable CORS".
In the CORS configuration, add the necessary "Access-Control-Allow-Origin" header, which specifies the domain that is allowed to make CORS requests to the API. For example, you can set it to "*" to allow requests from any domain, or you can specify a specific domain name.
Save the changes.

Enable CORS for the specific resource and method:
In the Amazon API Gateway console, go to the "Resources" section of your API.
Select the desired resource and method.
Go to the "Actions" menu and select "Enable CORS".
Configure the CORS options as necessary.
Save the changes.

Deploy the API:
In the Amazon API Gateway console, go to the "Actions" menu and select "Deploy API".
Select a deployment stage, or create a new one.
Save the changes.

After completing these steps, the API should return the necessary CORS headers and allow CORS requests from the specified domain. Keep in mind that changes to the API may take a few minutes to propagate.
Note: Make sure that your client-side code is correctly sending the "Origin" header with each request, as this header is used by the API to determine whether to allow the request.
