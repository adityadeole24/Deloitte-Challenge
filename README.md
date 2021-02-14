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
  - [Amazon Cognito](#amazon-cognito)
  - [Amazon Cloudfront and Amazon S3](#amazon-cloudfront-and-amazon-s3)
  - [Amazon Cloudwatch](#amazon-cloudwatch)


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

5 Sign into your application 

- The output of the CloudFormation stack creation will provide a CloudFront URL (in the **Outputs** table of the stack details page).  Click the link or copy and paste the CloudFront URL into your browser.

- You can sign into your application by registering an email address and a password.  Choose **Sign up to explore** to register.  The registration/login experience runs in the AWS account, and the supplied credentials are stored in Amazon Cognito.  
*Note: given that this application is only for demonstration purpose, please do not use an email and password combination that you use for other purposes (such as an AWS account, email, or e-commerce site).*

- Once you provide your credentials, you will receive a verification code at the email address you provided. Upon entering this verification code, you will be signed into the application.

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

### IAM

The following IAM role (and included policies) provides Lambda to input data inthe DynamoDb table:

**DynamoDbLambda**  
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










