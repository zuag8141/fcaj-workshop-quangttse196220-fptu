---
title: "Workshop"
date: 2026-08-03
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
# Deploying the Splitly Application on AWS Using CloudFormation and EC2

#### Overview

In this workshop, we will deploy the **Splitly** application on AWS using **AWS CloudFormation** to provision the infrastructure automatically.

Once the infrastructure is ready, we will connect to the **Amazon EC2** instance through **AWS Systems Manager Session Manager** and deploy both the backend and frontend of the application.

Main activities in this workshop:

+ Deploy the infrastructure with **AWS CloudFormation**.
+ Connect to the EC2 instance using **Session Manager**.
+ Clone the Splitly source code from GitHub.
+ Install dependencies and build the backend.
+ Run the backend using **PM2**.
+ Build the frontend.
+ Configure **Nginx** to serve the frontend and forward API requests to the backend.
+ Test the whole system.
+ Delete the CloudFormation stack after finishing to avoid unnecessary charges.

#### Content

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Prerequiste](5.2-Prerequiste/)
3. [Deploy Code and Web Server](5.3-DeployCode-WebServer/)
4. [System Testing](5.4-Test/)
5. [Resource Cleanup](5.5-Cleanup/)
