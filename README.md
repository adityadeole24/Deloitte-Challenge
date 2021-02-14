# Simple Web Application
## Outline

- [Overview](#overview)
- [Instructions](#instructions)
- [Architecture](#architecture)
- [Web Application Architecture](#web-application-architecture)
- [Implementation details](#implementation-details)
  - [Amazon DynamoDB](#amazon-dynamodb)
  - [Amazon API Gateway](#amazon-api-gateway)
  - [AWS Lambda](#aws-lambda)
  - [AWS IAM](#aws-iam)
  - [Amazon Cognito](#amazon-cognito)
  - [Amazon Cloudfront and Amazon S3](#amazon-cloudfront-and-amazon-s3)
  - [Amazon Cloudwatch](#amazon-cloudwatch)
- [Running your web application locally](#running-your-web-application-locally)
- [Considerations for demo purposes](#considerations-for-demo-purposes)
- [Known limitations](#known-limitations)
- [Additions, forks, and contributions](#additions-forks-and-contributions)
- [Extensions](#extensions)
- [Questions and contact](#questions-and-contact)

&nbsp;

## Overview

The goal of this interactive web application is to take data from the end user and store it in the database. The provided Cloudformation template automates the creation and entire deployment of this web application. The template includes following components:

**Database components**

*  Amazon DynamoDB offers fast, predictable performance for the key-value lookups needed in this web app, and enormous scalability. In this implementation, unique identifiers are created for each data input, user's first name and last name along with time description.

**Application components**

* Serverless service backend – Amazon API Gateway powers the interface layer between the frontend and backend, and invokes serverless compute with AWS Lambda. 
* Web application frontend - This is a static website hosted on S3 which is distributed by cloudfront. It integrates directly with the Amazon API gateway. 










