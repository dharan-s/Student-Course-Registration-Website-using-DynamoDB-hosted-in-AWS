# Student-Course-Registration-Website-using-DynamoDB-hosted-in-AWS

I developed and deployed a simple student course registration website using Lambda, DynamoDB and S3 comcepts in AWS.

S3 – to upload the information of the student and to host website.
 
Lambda Function – trigger whenever an object is created in input S3 bucket to put the data in dynamo table.

Dynamo Db – Stores the student-id and file-path to restore the file from S3 when student want to view their registered courses.

Makesure to give vaild IAM roles to access the S3 and dynamo DB.

Steps:
1. Create a S3 bucket.
   Create an S3 bucket with any name you like and any region that you’d like (I used Oregon/us-west-2.)
2. Create a dynamoDB instance.
3. Give permission the IAM user to query in Dynamo table.
4. Create the index lambda function
5. Store the html file in S3.
6. Run it



