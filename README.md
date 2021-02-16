# Simple Web Application
## Outline

- [Overview](#overview)
- [Deployment Instructions](#deployment-instructions)
- [3 Tier Architecture](#3-tier-architecture)
- [Why Serverless](#why-serverless)
- [Web Application Architecture](#web-application-architecture)
- [Implementation details](#implementation-details)
  - [AWS WAF](#aws-waf)
  - [AWS Cloudfront](#aws-cloudfront)
  - [AWS S3](#aws-s3)
  - [AWS API Gateway](#aws-api-gateway)
  - [AWS Lambda](#aws-lambda)
  - [AWS DynamoDB](#aws-dynamodb)
  - [AWS Cloudwatch](#aws-cloudwatch)
  - [AWS IAM](#aws-iam)
  - [AWS SNS Topic](#aws-sns-topic)
 - [Extensions](#extensions)

&nbsp;

## Overview

The goal of this web application is to take input data from the end user and store it in the database. The provided Cloudformation template automates the creation and entire deployment of this web application. The template includes following components:

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

2 Open the [AWS Cloudformation console](https://ap-southeast-2.console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/) and create a new stack using Web.Yaml file available in this repository. 

3 Continue through the CloudFormation wizard steps

- Name your stack, e.g. MyWebApp
- Provide a project name, e.g. webapp
- After reviewing, check the blue box for creating IAM resources.

4 Choose **Create stack**. This stack may take up to 15 minutes to complete.

5 Once the stack is created, go to the [API Gateway console](https://ap-southeast-2.console.aws.amazon.com/apigateway/main/apis?region=ap-southeast-2). Select the "webapp-dev-WebAppApi". Copy the API Gateway ID and paste into the index.html available in S3Objects folder here: [index.html](https://github.com/adityadeole24/Deloitte-Challenge/blob/main/S3Objects/index.html) and save the file. 

**Copy API Gateway ID**

![](https://github.com/adityadeole24/Deloitte-Challenge/blob/main/Readme-images/API%20gateway%20ID.png)

**Paste API Gateway ID in index.html**

![](https://github.com/adityadeole24/Deloitte-Challenge/blob/main/Readme-images/Index.html.PNG)

6 Download [index.html](https://github.com/adityadeole24/Deloitte-Challenge/blob/main/S3Objects/index.html) and [error.html](https://github.com/adityadeole24/Deloitte-Challenge/blob/main/S3Objects/error.html) in local machine. 

6 Go to [S3 Console](https://s3.console.aws.amazon.com/s3/home?region=ap-southeast-2#). Upload index.html and error.html into the "webapp-dev-webapps3" bucket manually. 

7 Go to  [Cloudfront Distributions](https://console.aws.amazon.com/cloudfront/home?region=ap-southeast-2#). Copy the domain name and paste it in the browser. Application user interface will open. 

8 Input your first name and last name. Click the button "Call API"

9 Go to [DynamoDb Console](https://ap-southeast-2.console.aws.amazon.com/dynamodb/home?region=ap-southeast-2#tables:) . Select "webapp-dev-ddbtable". Check the user input data in the "items" tab. 


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

A serverless application is an application in which the developer doesn’t have to manage any servers. It is a cloud computing approach in which AWS manages the servers and manages the scaling of the servers dynamically without the developer having to do anything. There are no operating systems to manage, no effort needs to be given to scale the application and most importantly there is no fixed cost. You pay as you go.

&nbsp;

---

&nbsp;

## Web Application Architecture

![](https://github.com/adityadeole24/Deloitte-Challenge/blob/main/Readme-images/High%20level%20Architecture.png)

&nbsp;

---

&nbsp;

## Implementation Details

&nbsp;

### AWS WAF

AWS WAF is a web application firewall which helps to protect the web application by blocking common attack patterns such as SQL injection and cross-site scripting. You can define rules to filter out specific traffic patterns. For this web applciation, WAF is deployed at Cloudfront and API level. 

&nbsp;

### AWS CloudFront

AWS CloudFront is a fast content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to customers globally with low latency, high transfer speeds, all within a developer-friendly environment. AWS cloudfront hosts the web application frontend that users interface with. AWS CloudFront syncs with AWS WAF to protect against multiple types of attacks including network and application layer DDoS attacks. 

&nbsp;

### AWS S3

AWS S3 stores our static application data and delivers the data to the end users via AWS cloudfront

&nbsp;

### AWS API Gateway

AWS API Gateway acts as the interface layer between the frontend (AWS CloudFront, AWS S3) and AWS Lambda, which calls the backend (AWS DynmoDb). This application utilizes REST API request/response model where a end user sends a request to input data and the lambda function responds to write the in dynamodb table synchronously. 

&nbsp;

### AWS Lambda

AWS Lambda is used to input user data into the DynamoDB database. In this application, "Inputdata" Lambda function will create a DynamoDb table object and inputs the user data into the table. 

&nbsp;

### AWS DynamoDB

The backend of this web application leverages Amazon DynamoDB to enable dynamic scaling and the ability to add features if extentions are to be added in the future. The application creates one table in DynamoDB; the table name will match the "ResourcePrefix" and "EnvironmentName" when creating the stack in CloudFormation. DynamoDB's primary key consists of a partition (hash) key. An optional sort (range) key can be added if required. 

&nbsp;

### AWS Cloudwatch

The capabilities provided by CloudWatch are not exposed to the end users of the web app, rather the developer can use CloudWatch logs, alarms, and graphs to track the usage and performance of this web application.

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

&nbsp;&nbsp;&nbsp;&nbsp;dynamodb:DeleteItem 

&nbsp;

### AWS SNS 

A AWS SNS topic is deployed in sync with the CloudWatch Alarms. The developer can choose to alert and send notification if any CloudWatch Alarms are out of threshold.

&nbsp;

---

&nbsp;

## Extentions

The following extensions can be made to this application.

### Automated Code Deployment Pipeline using AWS CodeCommit, AWS CodeBuild and AWS CodePipeline

For automated code deployment, the code can be hosted in AWS CodeCommit. The idea here is that AWS CodePipeline will build the web application using AWS CodeBuild. After successfully building, CodeBuild copies the build artifacts into a S3 bucket where the web application assets will maintained (web graphics, etc.).

![](https://github.com/adityadeole24/Deloitte-Challenge/blob/main/Readme-images/High%20level%20Architecture%20with%20CodePipeline.png)









