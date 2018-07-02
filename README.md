# Student-Course-Registration-Website-using-DynamoDB-hosted-in-AWS

I developed and deployed a simple student course registration website using Lambda, DynamoDB and S3 comcepts in AWS.

S3 – to upload the information of the student and to host website.
 
Lambda Function – trigger whenever an object is created in input S3 bucket to put the data in dynamo table.

Dynamo Db – Stores the student-id and file-path to restore the file from S3 when student want to view their registered courses.

Makesure to give vaild IAM roles to access the S3 and dynamo DB.




