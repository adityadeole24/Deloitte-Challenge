# Simple Web Application
## Outline

- [Overview](#overview)
- [Deployment Instructions](#deployment-instructions)
- [3 Tier Architecture](#3-tier-architecture)
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

- You can sign into your application by registering an email address and a password.  Choose **Sign up to explore** to register.  The registration/login experience is run in your AWS account, and the supplied credentials are stored in Amazon Cognito.  
*Note: given that this is a demonstration application, we highly suggest that you do not use an email and password combination that you use for other purposes (such as an AWS account, email, or e-commerce site).*

- Once you provide your credentials, you will receive a verification code at the email address you provided. Upon entering this verification code, you will be signed into the application.

&nbsp;

## 3 Tier Architecture









