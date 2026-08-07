---
title: "Week 9 Worklog"
date: "2026-07-18"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Objectives for Week 9:

* Learn about AWS services used for frontend deployment, backend hosting, file storage, system monitoring, and cost management.
* Understand how to design the network infrastructure and security for an application deployed on AWS.
* Analyze the connection flow between the application hosted on AWS and MongoDB Atlas.
* Research and create an overall AWS architecture diagram suitable for the project.
* Learn about solutions for monitoring system performance and controlling AWS costs.

### Tasks to be completed this week:

| Day       | Tasks                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Start date | Completion date | Reference materials |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------- |
| Monday    | - Learn about the overall system architecture on AWS. <br> - Study the AWS services used for frontend deployment, including Route 53, CloudFront, S3 Frontend, and ACM. <br> - Analyze the user access flow through Route 53, CloudFront, and the frontend application. <br> - Learn how ACM provides SSL/TLS certificates for secure HTTPS connections.                                                                                                                                                                                                                                                          | 14/07/2026 | 14/07/2026      |                     |
| Tuesday   | - Learn about AWS networking components, including VPC, Public Subnet, and Internet Gateway. <br> - Learn how Security Groups control inbound and outbound traffic for EC2 instances. <br> - Understand the role of Elastic IP in providing a fixed public IP address for EC2. <br> - Analyze the deployment model of a backend application hosted on an EC2 instance in a Public Subnet.                                                                                                                                                                                                                         | 15/07/2026 | 15/07/2026      |                     |
| Wednesday | - Learn how to use S3 Receipts to store receipt images and files uploaded by users. <br> - Learn about IAM Roles and how to grant EC2 permission to access S3 without storing Access Keys directly in the source code. <br> - Analyze the image upload and retrieval flow between the frontend, the EC2 backend, and S3 Receipts. <br> - Learn how to connect the backend hosted on EC2 to MongoDB Atlas.                                                                                                                                                                                                         | 16/07/2026 | 16/07/2026      |                     |
| Thursday  | - Learn how CloudWatch is used to monitor EC2 metrics, logs, and operational status. <br> - Learn how SNS sends notifications when errors occur or monitoring thresholds are exceeded. <br> - Learn how AWS Budgets monitors AWS spending and sends cost alerts. <br> - Analyze how CloudWatch, SNS, and AWS Budgets can be integrated into the system architecture.                                                                                                                                                                                                                                              | 17/07/2026 | 17/07/2026      |                     |
| Friday    | - **Architecture design practice:** <br>&emsp; + Identify the main components of the system. <br>&emsp; + Draw the frontend access flow through Route 53, CloudFront, and S3 Frontend. <br>&emsp; + Draw the request flow from the frontend to the backend hosted on EC2. <br>&emsp; + Illustrate the connections between EC2, S3 Receipts, and MongoDB Atlas. <br>&emsp; + Add IAM Roles, Security Groups, and other security components to the diagram. <br>&emsp; + Add CloudWatch, SNS, and AWS Budgets to the architecture. <br> - Review and complete the overall AWS architecture diagram for the project. | 18/07/2026 | 18/07/2026      |                     |

### Results achieved in Week 9:

* General results:
  * This week, I focused on researching and designing an AWS architecture for deploying the project.
  * The architecture includes components for frontend distribution, backend hosting, receipt image storage, database connectivity, system monitoring, and cost management.
  * I gained a better understanding of the system's overall operational flow, from the moment a user accesses the application to the point where requests are processed by the backend and data is stored in the relevant services.

* Theoretical knowledge gained:
  * Route 53 is used to manage the domain name and route users to the application.
  * CloudFront is used to distribute frontend content through a Content Delivery Network.
  * S3 Frontend is used to store static files for the frontend application.
  * ACM is used to manage SSL/TLS certificates and enable secure HTTPS access.
  * VPC, Public Subnet, and Internet Gateway provide the network environment for AWS resources.
  * Security Groups control inbound and outbound traffic for EC2 instances.
  * Elastic IP provides a fixed public IP address for an EC2 instance.
  * EC2 is used to deploy and operate the project's backend application.
  * S3 Receipts is used to store receipt images and files uploaded by users.
  * IAM Roles allow EC2 instances to access other AWS services without storing credentials directly in the source code.
  * MongoDB Atlas is used as the database system and is connected to the backend hosted on EC2.
  * CloudWatch supports the monitoring of EC2 metrics, logs, and operational status.
  * SNS supports sending notifications when alerts or system issues occur.
  * AWS Budgets helps monitor AWS spending and sends notifications when costs reach a specified threshold.

* Architecture design practice:
  * Identified and categorized the frontend, backend, storage, database, monitoring, and cost-management components.
  * Designed the frontend access flow: User → Route 53 → CloudFront → S3 Frontend.
  * Designed the backend request flow: Frontend → Elastic IP → EC2.
  * Placed the EC2 instance inside a Public Subnet within the VPC and connected it to the Internet through an Internet Gateway.
  * Used a Security Group to restrict the ports and sources allowed to access the EC2 instance.
  * Designed the flow for uploading and retrieving receipt images from S3 Receipts through an IAM Role.
  * Illustrated the connection between the backend hosted on EC2 and MongoDB Atlas.
  * Added CloudWatch to collect EC2 metrics and application logs.
  * Added SNS to send notifications when system issues occur or monitoring thresholds are exceeded.
  * Added AWS Budgets to monitor and control AWS service costs.
  * Completed the overall AWS architecture diagram for the project deployment.
