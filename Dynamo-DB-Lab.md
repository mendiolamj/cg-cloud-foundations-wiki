
Here's an overview of the lab: 

<img width="586" alt="Screen Shot 2023-01-09 at 1 03 59 AM" src="https://user-images.githubusercontent.com/25653204/211248888-81d3d906-6c94-4563-a4e5-4a71f9c35486.png">

Here are the actual steps for the lab: 

1. Create a new IAM role. Name it DDRole. 
1. It should be assumed by EC2. 
1. Allocate it AmazonDynamoDBFullAccess policy.
1. Also, select DND_permission_boundary as the Permissions Boundary.
1. Allocate this IAM role to your Public EC2 instance. For this, select your public EC2 instance. Go to Actions > Security > Modify IAM Role. 
1. Connect to the Public Instance and execute the following:

```console
mkdir dynamo-lab
cd dynamo-lab
aws sts get-caller-identity 

wget https://usaa-aws-resources.s3.amazonaws.com/dynamodb-lab/create_table.py

wget https://usaa-aws-resources.s3.amazonaws.com/dynamodb-lab/items.json

wget https://usaa-aws-resources.s3.amazonaws.com/dynamodb-lab/bulk_load_table.py

```


7. Use python code from your EC2 instance to:
a. Create DynamoDB table
b. Insert data into this table. Execute following: 

```console
python3 ./create_table.py 

python3 ./bulk_load_table.py 
```

Verify the inserted data from Management Console (DynamoDB Dashboard > Tables). Select the “battle-royale” table and click on the View Items button.

Delete the DynamoDB table and the IAM role. Also, detach the IAM role from the EC2 instance.


