# Simple Web Application
## Outline

- [Overview](#overview)
- [Deployment Instructions](#deployment-instructions)
- [3 Tier Architecture](#3-tier-architecture)
- [Why Serverless](#why-serverless)
- [Web Application Architecture](#web-application-architecture)
- [Implementation details](#implementation-details)
  - [Amazon DynamoDB](#amazon-dynamodb)
  - [Amazon API Gateway](#amazon-api-gateway)
  - [AWS Lambda](#aws-lambda)
  - [AWS IAM](#aws-iam)
  - [Amazon Cognito and SNS Topic](#amazon-cognito-and-sns-topic)
  - [Amazon Cloudfront and Amazon S3](#amazon-cloudfront-and-amazon-s3)
  - [Amazon Cloudwatch](#amazon-cloudwatch)
- [Extentions](#extentions)


&nbsp;

## Overview

The goal of this interactive web application is to take data from the end user and store it in the database. The provided Cloudformation template automates the creation and entire deployment of this web application. The template includes following components:

**Database components**

*  Amazon DynamoDB offers fast, predictable performance for the key-value lookups needed in this web app, and offers enormous scalability for any future extentions. In this implementation, unique identifiers are created for each data input, user's first name and last name along with time description.

**Application components**

* Serverless service backend – Amazon API Gateway powers the interface layer between the frontend and backend, and invokes serverless compute with AWS Lambda. 
* Web application frontend - This is a static website hosted on S3 which is distributed by cloudfront. It integrates directly with the Amazon API gateway. 

&nbsp;

---

&nbsp;

## Deployment Instructions

To deploy this web application please follow the below steps:

1 Log-in to the [AWS console](https://console.aws.amazon.com/)

2 Open the AWS CloudFormation console and create a new stack. 

3 Continue through the CloudFormation wizard steps

- Name your stack, e.g. MyWebApp
- Provide a project name, e.g. webapp
- After reviewing, check the blue box for creating IAM resources.

4 Choose **Create stack**. This stack may take up to 15 minutes to complete.

5 Once the stack is created, go to the [API Gateway console](https://ap-southeast-2.console.aws.amazon.com/apigateway/main/apis?region=ap-southeast-2). Select the "webapp-dev-WebAppApi". Copy the API Gateway ID and paste into the index.html available in S3Objects folder and save the file. 

**Copy API Gateway ID**

![](https://github.com/adityadeole24/Deloitte-Challenge/blob/main/Readme-images/API%20gateway%20ID.png)

**Paste API Gateway ID in index.html**

![](https://github.com/adityadeole24/Deloitte-Challenge/blob/main/Readme-images/API%20gateway%20ID.png)

6 Upload index.html and error.html into the s3 bucket manually

7 Go to Cloudfront. Copy the domain name and paste it in the browser. Application user interface will open. 

8 Input your first name and last name. Click the button "Call API"

9 Check the status in the Dynamo Db table.


&nbsp;

---

&nbsp;

## 3 Tier Architecture

A 3 tiered architecture consists of mainly 3 layers. The Presentation Layer, Application Layer, and the Data Layer. Each layer has its own set of responsibilities and uses communication methods to interact with the other layers.

The **Presentation Layer** is basically the layer with which the end-user interacts. In this web application users interact to a single page application.

The **Application layer** consists of the business logic of the project. 

The final layer is the **Data Layer** which has the responsibility of persisting data and providing the Application Layer with necessary data. In this web application user data is persisted in Amazon DynamoDb.

**Architecture diagram**
![](https://github.com/adityadeole24/Deloitte-Challenge/blob/main/Readme-images/3%20Tier%20Architecture.png)

&nbsp;

---

&nbsp;

## Why Serverless

A serverless application is an application in which the developer doesn’t have to manage any servers. It is a cloud computing approach in which AWS manages the servers and manages the scaling of the servers dynamically without the developer having to do anything. There are no operating systems to manage, no effort needs to be given to scale our application and most importantly there is no fixed cost. You pay as you go.

&nbsp;

---

&nbsp;

## Web Application Architecture

&nbsp;

---

&nbsp;

## Implementation Details

&nbsp;

### Amazon DynamoDB

The backend of this web application leverages Amazon DynamoDB to enable dynamic scaling and the ability to add features if extentions are to be added in the future. The application creates one table in DynamoDB; the table name will match the "ResourcePrefix" and "EnvironmentName" when creating the stack in CloudFormation. DynamoDB's primary key consists of a partition (hash) key. An optional sort (range) key can be added if required. 

&nbsp;

### Amazon API Gateway

Amazon API Gateway acts as the interface layer between the frontend (Amazon CloudFront, Amazon S3) and AWS Lambda, which calls the backend (DynmoDb).

&nbsp;

### AWS Lambda

AWS Lambda is used to input user data into the DynamoDB database. In this application,Ihave user "Inputdata" Lambda Function which creats a DynamoDb table objectand inputs the user data into the table. 

&nbsp;

### AWS IAM

The following IAM role (and included policies) provides Lambda to input data inthe DynamoDb table:

**InputdataFunctionLambda**  
*AWS managed policy*  

AWSLambdaBasicExecutionRole  

*Inline policy*  

DataPolicy  
&nbsp;&nbsp;&nbsp;&nbsp;dynamodb:PutItem 

&nbsp;&nbsp;&nbsp;&nbsp;dynamodb:Query 

&nbsp;&nbsp;&nbsp;&nbsp;dynamodb:UpdateItem

&nbsp;&nbsp;&nbsp;&nbsp;dynamodb:GetItem

&nbsp;&nbsp;&nbsp;&nbsp;dynamodb:Scan 

&nbsp;&nbsp;&nbsp;&nbsp;dynamodb:DeleteItem 

&nbsp;

### Amazon Cognito and SNS Topic

Amazon Cognito handles user account authentication and authorization for this web application. The user visits the website and during the sign-in process Amazon SNS topic sends an email request to the user to verify if the user is geniuine or not.

&nbsp;

### Amazon CloudFront and Amazon S3
Amazon CloudFront hosts the web application frontend that users interface with.  This includes web assets like pages. CloudFormation pulls these resources from S3.

&nbsp;

### Amazon Certificate Manager
Amazon Certificate Manager provides SSL/TLS certificates to seure network communication ans establish identity of this web application over the internet.

&nbsp;

### Amazon CloudWatch

The capabilities provided by CloudWatch are not exposed to the end users of the web app, rather the developer can use CloudWatch logs, alarms, and graphs to track the usage and performance of this web application.

&nbsp;

---

&nbsp;

## Extentions









