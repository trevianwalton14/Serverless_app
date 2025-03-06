# Serverless App – Pet CuddleOTron

In this project I implemented a simple serverless application using S3, API Gateway, Lambda, Step Functions, SNS and SES. 

<a href="https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=PetCuddleOTron.drawio&dark=0#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1XaiFFMQqV2qtnzZS3es_kFJoqqA6sbXG%26export%3Ddownload"> Design</a>

Text Based Instructions

 ## Stage 1: Configure Simple Email Service and Simple Notification Service
 
The Pet Cuddle O Tron will send and receive email reminders from within the application. SES starts off in sandbox mode which means I must explicitly verify which addresses I want to use the service. I used two different addresses that the application will send from. 

I used twaltonsmiles@gmail.com as my sending address and twaltonaws@gmail.com as my receiving address. 

## Stage 2: Add an email Lambda function to use SES to send emails for the serverless architecture

In this stage I added a Lambda function to use SES to send emails and an execution role to provide the Lambda function the permissions needed to interact with SES.

I created an IAM Role which the email lambda reminder will use to interact with other AWS services. The IAM provides access to SES, SNS and Logging permissions to whatever assumes the role. 

Next, I created a lambda function for the serverless application to create an email and then send it using SES.

The code will import some libraries that will be used during its execution. It makes a connection to the SES service using the standard Boto3 client. The function then sends an email using data that’s passed to it by the state machine. When the lambda function is invoke by the state machine it’s given object and inside this object will be the destination email address to send to and a message. 

## Stage 3: Implement and Configure the State Machine

The state machine manages the flow of the application. The state machine waits a certain amount of time and then uses a lambda function to send a notification to the customer. 

I created an IAM Role that the state machine will use to interact with other AWS services. The IAM provides access to SNS, Logging and invoke a Lambda function.

I created a State Machine. The state machine starts and then waits for a certain amount of time based on the Timer State. Then the emails state invokes a lambda function. The next state finishes the execution.

## Stage 4: Implement the API Gateway, API and supporting lambda function

The API gateway is going to take in the data from the customer local machine browser and invoke a Lambda function.  The API Ggateway will provide the lambda function with the data that the client has sent. 

The code accepts the data in and performs some checks, if it fails it provides a 400 response back to the client. If the lambda function got all the information it needed then it will execute the state machine. 

I created the API Gateway. 

## Stage 5: Implement the static frontend application and test functionality
I created an S3 bucket where the frontend components of the serverless architecture where it will run from. I enabled static website hosting so that it can function as a frontend hosting point for the application itself. The frontend will load the html, css, and js files I added to the bucket and communicate with the API gateway .  

