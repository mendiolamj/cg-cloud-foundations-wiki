Important parts of this lab are: 
Creating the EKS cluster in the existing VPC. 
Creation of Fargate Profiles.
Creation of Service Account to interact with AWS services.
Launching a pod in the correct Fargate profile. 
Connecting to an AWS service from the pod. 
----------------------------------------

1. Generate Access Keys for IAM user CloudLearner (if not done already). 
1. Connect to your Public Instance and execute “aws configure”
1. Run “aws sts get-caller-identity” and verify that you are using IAM user CloudLearner.
1. Execute: 

```console
mkdir eks-lab

cd eks-lab/

wget https://usaa-aws-resources.s3.amazonaws.com/eks-lab/cluster-fargate-3.yaml

vi cluster-fargate-3.yaml
```

5. Update the VPC id and Private Subnet IDs & their CIDRs (according to the VPC you have created in your account) in this file and then save it. Be careful about the AZs mentioned in the file. 
6. In a new tab, create a NAT Gateway in one of the Public Subnets. Assign Elastic IP to it as well. 
7. Update the Private Route Table to have a route for this NAT Gateway (for target 0.0.0.0/0). All the routes in the private Route Table should show as ACTIVE. You can update the older 0.0.0.0/0 route itself.
8. Install kubectl  - [Installing kubectl - Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)  (look for Linux tab)
9. Install eksctl - [The eksctl command line utility - Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html) (look for Linux tab)
10. Execute:
```console
eksctl create cluster -f cluster-fargate-3.yaml
```
11. Open CloudFormation & EKS in new browser tabs to review the activities happening there.
12. Wait for the EKS cluster to be ready. Constantly review the progress in AWS Management Console. Check the Fargate profiles which get created as part of the EKS cluster.  2 Fargate Profiles should be created. (Refer: EKS Cluster > Configuration tab > Compute tab). It may take 20 to 30 minutes for the complete cluster creation. 
13. Once you get the success message for Create Cluster command, execute “kubectl run nginx --image=nginx” to create a pod. 
14. Check the running pods using “kubectl get pods --all-namespaces --output wide”
15. Wait for this newly created pod to come to Running state. Execute above command again to check the progress. Once done, check the available Nodes in the EKS Cluster (in EKS Dashboard). 
16. Execute “kubectl create namespace dev” to create a new namespace.
17. Execute “kubectl run nginx-dev --image=nginx --namespace dev” to create a new pod in the dev namespace 
18. Check the running pods using “kubectl get pods --all-namespaces --output wide”
19. Wait for this newly created pod to come to Running state. 
20. You can verify the running fargate nodes and their details by executing “kubectl describe nodes” 
21. A service account needs to be created in EKS so that it could help pods to interact with AWS services (via an IAM role). 
22. Execute: 
```console
eksctl utils associate-iam-oidc-provider --cluster fargate-cluster-3 --approve --region us-east-1

# make sure there are no additional spaces than what is mentioned here
eksctl create iamserviceaccount --namespace dev --name ddb-sa-2 --cluster fargate-cluster-3 --attach-policy-arn arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess --approve --region us-east-1
```

23. Check the details of the service account. Execute: 
kubectl get sa ddb-sa-2 -o yaml --namespace dev
24. Execute following to create a new pod which uses the created service account: 
```console
wget https://usaa-aws-resources.s3.amazonaws.com/eks-lab/my-pod.yaml

#Open and review this yaml file. As this is part of dev namespace, it will be placed in fp-dev fargate profile.

kubectl apply -f my-pod.yaml 

kubectl get pods --all-namespaces --output wide
```

25. Verify the status of the pod using the above command. Once the pod is in running state, move ahead. 
26. Execute “kubectl exec -it my-pod --namespace dev -- bash” to login to a pod.
27. In a new browser tab, navigate to DynamoDB dashboard and create 2 tables (names & attributes don’t matter).

Following needs to be executed inside the pod.

Execute following: 
```console
apt-get update && apt-get install curl unzip less -y

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

./aws/install

aws sts get-caller-identity
```
# you should see the reference to the IAM role related to the created service account.

```console
aws dynamodb list-tables --region us-east-1
```

You should see the names of DynamoDB tables (which you created in the previous steps). The pod is able to interact with AWS service using the IAM role. 

--------------------

You can now delete the EKS cluster. 
Execute “eksctl delete cluster --name fargate-cluster-3”


1. Delete the NAT Gateway.
1. Delete the RDS you had created.
1. Delete the ElastiCache cluster you had created.
1. Release all the Elastic IPs. (see VPC Dashboard > Elastic IP)

1. Delete the Public EC2 instance. Wait for 5 minutes. 

1. Delete the VPC which you had created at the start of training. 

1. Delete all the DynamoDB tables, S3 buckets, IAM roles, IAM users, IAM policies you have created. 

1. Delete any other resource you had created in the AWS account.