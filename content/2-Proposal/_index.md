---
title: "Proposal"
date: 2026-07-31
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Splitly – Group Expense Sharing Platform

## An AWS-Based Solution for Group Expense Management, Bill Splitting, and Settlement Tracking

## 2.1 Executive Summary

Splitly is a group expense management and sharing platform developed to help users track shared expenses, calculate outstanding balances, and manage settlement activities among group members in a transparent and convenient manner.

The system is built using a modern web architecture, with React and Vite for the user interface, Node.js and Express for the backend service, and MongoDB Atlas as the database. The infrastructure is deployed on AWS, using Amazon EC2 to host both the frontend and backend applications, Amazon S3 to store receipts and supporting images, Amazon CloudWatch to monitor the system, and Amazon VPC and Security Groups to provide secure network connectivity.

The platform provides key features such as group management, expense recording and sharing, debt calculation, payment status tracking among members, and electronic receipt storage. The architecture is designed to be scalable and suitable for small- to medium-sized user groups.

---

## 2.2 Problem Statement

### Current Problems

Group expense management is currently still performed mainly through spreadsheets or messages in communication applications. As the number of members and expenses increases, tracking who has paid, who still owes money, and how much must be settled becomes increasingly complicated and prone to errors.

Common difficulties include:

* Difficulty managing multiple expenses within the same group.
* Time-consuming and error-prone calculation of balances among members.
* Lack of a centralized receipt storage system for future verification.
* Lack of a clear mechanism for tracking payment statuses between debtors and recipients.
* Difficulty monitoring system activities and resolving incidents after the application is deployed.

### Proposed Solution

The proposed solution is to build Splitly on the AWS platform to digitalize group expense management. The system provides group management, expense recording, balance calculation, settlement tracking, and receipt storage on a centralized platform. It also uses AWS services to support efficient application deployment, data storage, and system monitoring.

### Benefits and Return on Investment (ROI)

Implementing Splitly provides several benefits, including:

* Reducing the time required to calculate and reconcile balances among members.
* Minimizing errors during expense splitting and settlement processes.
* Improving transparency through centralized transaction history and payment status records.
* Supporting electronic receipt storage for convenient searching and verification.
* Monitoring system activities through Amazon CloudWatch, thereby improving operational visibility and incident handling.

The solution uses MongoDB Atlas together with AWS services such as Amazon EC2, Amazon S3, and Amazon CloudWatch to optimize deployment and operational costs while still meeting the needs of small- to medium-sized user groups.

Compared with manual expense management methods, Splitly saves time, reduces errors, and improves the efficiency of group expense management. The system architecture is also designed for future scalability, providing a foundation for adding new features and supporting a larger number of users.

---

## 2.3 Solution Architecture

The project uses a monolithic application architecture deployed centrally on AWS infrastructure. The system consists of three main layers.

### Presentation Layer – Frontend

* The React/Vite frontend is built into static files, including HTML, CSS, and JavaScript.
* These static files are stored directly on the EC2 server and served by the Nginx web server.
* When users access the website through the Elastic IP address on port 80, Nginx loads and returns the user interface to their browsers.

### Application Layer – Backend

* Both the Node.js backend and the Nginx web server, which serves the frontend, are deployed on the same Amazon EC2 instance.
* The EC2 instance is located in a public subnet within a VPC in the `ap-southeast-1` AWS Region.
* The Web Security Group controls the ports that are allowed to access the EC2 instance. Port 80 is opened for web traffic, while port 22 is opened for SSH administration.
* The Internet Gateway provides two-way connectivity between the EC2 instance and the Internet.
* Nginx also acts as a reverse proxy by routing REST API requests from the `/api` path to the backend application running on port 5000 and managed by PM2.
* The application layer also connects directly to third-party services through the Internet, including the VNPay payment gateway and Gmail SMTP.

### Data Layer

* Business data, including user information, groups, expenses, settlement transactions, disputes, and notifications, is securely stored in MongoDB Atlas.
* Images and payment receipt files uploaded by users are stored separately in the Amazon S3 Receipts Bucket.
* MongoDB stores only file metadata, such as the file name, object key, URL, and processing status, instead of storing the physical files directly. This approach helps optimize storage costs and system performance.

### Security, Monitoring, and Cost Management

* **IAM Role:** The EC2 instance is assigned an IAM Role that allows it to upload files to the S3 Receipts Bucket and send monitoring data to CloudWatch. This improves security because Access Keys and Secret Keys do not need to be stored directly on the server.
* **Configuration Management:** Sensitive information, such as the MongoDB URI, JWT secret, Gmail configuration, and VNPay keys, is configured through environment variables in a `.env` file stored securely on the EC2 server.
* **Monitoring:** Amazon CloudWatch is used to collect basic metrics and system logs and to monitor the operating status of the EC2 instance and application. When an error occurs or a monitored metric exceeds its threshold, CloudWatch triggers an alert through Amazon SNS.
* **Cost Management:** AWS Budgets continuously monitors resource costs and automatically sends alerts through email or SNS when spending exceeds the expected project budget.

### 2.3.1 Current Architecture
![Architecture](/aws_internship/images/2-Proposal/Architecture_Final.png)

### AWS Services Used

* **Amazon S3:** Stores receipt files uploaded by users.
* **Amazon EC2:** Runs the backend API and processes the system's business logic.
* **AWS IAM:** Grants the EC2 instance permission to access required AWS resources.
* **Amazon CloudWatch:** Collects logs, monitors the EC2 instance, and detects system incidents.
* **Amazon SNS:** Sends email notifications or alerts generated by CloudWatch and AWS Budgets.
* **AWS Budgets:** Monitors costs and sends alerts when spending exceeds the configured budget.

### Component Design

* **Web Interface and Proxy:** The React/Vite frontend application is built into static files and stored and served directly by the Nginx server running on Amazon EC2. Nginx also acts as a reverse proxy, routing API requests from users to the backend application.

* **Business Logic Processing – Backend:** Amazon EC2, which also hosts the frontend web server, runs the backend API through Node.js and PM2. The backend handles authentication, group management, expenses, settlements, receipts, disputes, and notifications.

* **Data Storage:** MongoDB Atlas stores core data, including users, groups, expenses, settlements, disputes, and notifications.

* **Receipt Storage:** Amazon S3 Receipts Bucket stores images and receipt files uploaded by users, reducing the amount of local storage required on the EC2 instance.

* **Network Connectivity:** Amazon VPC, Public Subnet, Internet Gateway, and Elastic IP provide the basic network infrastructure. They allow the EC2 instance to communicate with users, upload files to Amazon S3, connect to MongoDB Atlas, and access external services such as VNPay and Gmail SMTP.

* **System Protection:** The Web Security Group acts as a firewall that limits the ports and network sources allowed to access the EC2 instance. For example, port 80 is opened for web traffic, while port 22 is used for SSH administration.

* **Configuration and Secret Management:** Sensitive system information, including the MongoDB URI, JWT Secret, Gmail credentials, and VNPay keys, is stored in environment variables through a `.env` file on the EC2 server.

* **Access Permission Management:** An AWS IAM Role grants the EC2 instance permission to access the S3 Receipts Bucket and CloudWatch according to the principle of least privilege, without storing Access Keys on the server.

* **System Monitoring:** Amazon CloudWatch operates in the background to collect application logs, monitor EC2 resource metrics such as CPU, RAM, and disk usage, and create alerts when incidents are detected.

* **Alert Delivery:** Amazon SNS acts as an alert delivery channel, sending emails or notifications from CloudWatch and AWS Budgets to the system administrator.

* **Cost Management:** AWS Budgets continuously monitors AWS infrastructure costs and triggers alerts when actual or forecasted spending reaches or exceeds the configured budget threshold.

### 2.3.2 Proposed Future Architecture

The following architecture represents the proposed future enhancement of the Splitly system. It is not part of the current deployment but is planned as the next stage of infrastructure development.

![Architecture_Update](/aws_internship/images/2-Proposal/Architecture_Update.png)

In the proposed architecture, the frontend application will be separated from the backend server. The React/Vite frontend will be built into static files and stored in an Amazon S3 Frontend Bucket. Amazon CloudFront will distribute the frontend content to users with lower latency and improved availability.

Amazon Route 53 will provide Domain Name System (DNS) management, while AWS WAF will help protect the web application from common web attacks. AWS Certificate Manager will be used to manage SSL/TLS certificates and enable secure HTTPS communication.

The backend application will continue to run on an Amazon EC2 instance located in a Public Subnet within the Amazon VPC. The backend will process business logic, provide REST APIs, connect to MongoDB Atlas, and upload receipt files to a separate Amazon S3 Receipts Bucket.

Amazon CloudWatch will monitor infrastructure metrics and application logs. Amazon SNS will send operational alerts to the administrator, while AWS Budgets will monitor infrastructure costs and notify the project team when spending approaches the configured budget limit.

### 2.3.3 Expected Improvements

Compared with the current architecture, the proposed future architecture provides the following improvements:

+ **Frontend and backend separation:** Static frontend files are moved from EC2 to Amazon S3, allowing the EC2 instance to focus on backend processing.

+ **Improved performance:** Amazon CloudFront caches and distributes frontend content through edge locations, reducing website loading time.

+ **Enhanced security:** AWS WAF protects the application from common web attacks, while AWS Certificate Manager supports secure HTTPS connections.

+ **Domain management:** Amazon Route 53 allows users to access the application through a custom domain instead of directly using the Elastic IP address.

+ **Improved scalability:** The frontend and backend can be scaled independently based on system demand.

+ **Reduced EC2 workload:** Serving frontend files through Amazon S3 and CloudFront reduces the amount of traffic and processing handled by the EC2 instance.

+ **Better availability:** CloudFront and Amazon S3 provide a more reliable method for distributing static frontend content.

This architecture provides a foundation for future expansion, including the possible introduction of an Application Load Balancer, Auto Scaling, Amazon Cognito, AWS Lambda, and automated CI/CD deployment.

---

## 2.4 Technical Implementation

The project focuses on deploying the entire application, including both the frontend and backend, on a single Amazon EC2 server together with supporting cloud services. The implementation process is divided into four stages.

### Stage 1 – Architecture Research and Design

The team analyzes the project source code structure, including the separation between the backend and frontend application directories. The team also studies Amazon EC2, VPC, IAM, Amazon S3 for file storage, CloudWatch, SNS, AWS Budgets, MongoDB Atlas, Nginx, and PM2 to design a suitable deployment architecture.

### Stage 2 – Cost Estimation and Feasibility Assessment

AWS Pricing Calculator is used to estimate the costs of the EC2 server, S3 receipt storage, network traffic, and CloudWatch. An EC2 instance type suitable for a student project is selected, and AWS Budgets is configured to control monthly infrastructure costs.

### Stage 3 – Infrastructure Setup and Configuration

The basic network infrastructure is created, including a VPC, Public Subnet, Internet Gateway, and Security Group. An EC2 instance is launched and assigned an Elastic IP address. A single S3 Bucket is created for receipt storage, and an IAM Role is configured and attached to the EC2 instance. MongoDB Atlas, CloudWatch, SNS, and AWS Budgets are also configured.

### Stage 4 – Development, Testing, and Deployment

The source code is cloned from GitHub to the EC2 server, and sensitive environment variables are configured through the `.env` file.

The React/Vite frontend is installed and built, while Nginx is configured to serve the static files and operate as a reverse proxy. The Node.js/Express backend is built and maintained by PM2.

The backend is then connected to MongoDB Atlas, the S3 Receipts Bucket, and third-party services such as VNPay and Gmail. Finally, the team tests the API, receipt upload functionality, monitoring logs, and system operations before putting the application into use.

### Technical Requirements – Updated Version

#### Architecture and Infrastructure

The system is deployed in the AWS Singapore Region, identified as `ap-southeast-1`. The entire application, including the frontend and backend, is stored and operated on a single Amazon EC2 server located in a Public Subnet within a VPC.

#### Technologies

* **Frontend:** React, TypeScript, and Vite.
* **Backend:** Node.js, Express, and TypeScript, providing REST APIs.
* **Web Server and Process Manager:** Nginx is used as the web server and reverse proxy, while PM2 manages the backend process and automatically restarts it when necessary.

#### Source Code Management and Deployment

The source code is managed on GitHub. Deployment activities, including dependency installation and source code building, are performed directly in the EC2 environment through the command line.

#### Data and Storage

Core business data is securely stored in MongoDB Atlas. Static files, including images and receipts uploaded by users, are stored in the Amazon S3 Receipts Bucket to optimize storage usage.

#### Networking and Connectivity

The EC2 server communicates with the Internet through an Internet Gateway and an Elastic IP address. The Security Group is configured to open port 80 for HTTP web access and port 22 for administrator SSH access.

Nginx is responsible for routing traffic by either returning the static frontend files or forwarding API traffic to the backend application running internally on port 5000.

#### Security and Access Control

Sensitive information, including the database URI, JWT Secret, and integration keys, is protected through environment variables stored in a `.env` file.

The EC2 server is assigned an IAM Role that grants permission to access the S3 Receipts Bucket and CloudWatch according to the principle of least privilege.

#### Monitoring and Alerts

Amazon CloudWatch is configured to collect server metrics and application logs. Amazon SNS acts as the notification delivery channel, sending alerts to the administrator's email address when the system experiences errors or resource constraints.

#### Cost Management

AWS Budgets continuously monitors the cost of AWS resources and automatically sends alerts when actual or forecasted spending approaches the configured budget limit.

#### Non-Functional Requirements

The system is deployed in the Singapore Region to optimize connectivity and reduce network latency for users in Vietnam.

The single-EC2 architecture is suitable for the budget of a student project and supports vertical scaling by increasing the CPU and RAM configuration when the number of users or system workload increases.

---

## 2.5 Timeline

The project is implemented through four main stages over approximately three months to ensure that development, testing, and deployment activities are performed systematically.

### Stage 1 – Requirement Analysis and System Design

**Week 5 – Week 6**

* Analyze the business requirements of the group expense management system.
* Design the overall AWS architecture.
* Design the MongoDB Atlas database.
* Develop the user interface design and backend architecture.

### Stage 2 – Feature Development

**Week 7 – Week 8**

* Develop the frontend using React and Vite.
* Develop the API using Node.js and Express.
* Integrate MongoDB Atlas.
* Develop the following features:

  * Authentication
  * Group Management
  * Expense Management
  * Settlement
  * Receipt Upload

### Stage 3 – AWS Deployment

**Week 9 – Week 10**

* Launch an Amazon EC2 instance.
* Configure the VPC, Security Group, and Elastic IP.
* Deploy the backend and frontend applications to EC2.
* Create an Amazon S3 Bucket for receipt storage.
* Configure an IAM Role.
* Set up CloudWatch monitoring.

### Stage 4 – Testing and Completion

**Week 11 – Week 12**

* Perform functional testing.
* Perform API testing.
* Test receipt upload functionality.
* Verify logging and monitoring.
* Optimize AWS costs.
* Complete the project documentation and report.

---

## 2.6 Budget Estimation

The system is designed for small-scale educational and testing purposes. Therefore, the project prioritizes the use of AWS Free Tier services and other services with the lowest possible costs.

### Estimated Infrastructure Costs

* **Amazon EC2:** USD 0.00 per month
  A `t3.micro` instance is used under the AWS Free Tier allowance of 750 hours per month. The instance runs both the Nginx web server and the Node.js backend.

* **Amazon S3 Standard:** USD 0.10 per month
  The estimate assumes 5 GB of storage for the Receipts Bucket and approximately 2,000 PUT and GET requests.

* **Amazon CloudWatch:** USD 0.03 per month
  This cost covers basic EC2 monitoring metrics and application log storage.

* **Amazon SNS:** USD 0.00 per month
  The system is expected to send approximately 100 email alerts per month, which is within the Free Tier allowance.

* **Amazon VPC:** USD 0.00 per month
  This includes one VPC, one Public Subnet, an Internet Gateway, a Route Table, and a Security Group.

* **Elastic IP:** USD 3.65 per month
  AWS charges USD 0.005 per hour for public IPv4 addresses, including an Elastic IP address attached to an EC2 instance.

* **AWS IAM:** USD 0.00 per month
  IAM Roles and access permissions are provided without additional cost.

**Estimated total cost:** Approximately USD 3.78 per month, equivalent to USD 45.36 over 12 months.

During the development period, the team plans to maximize the use of the AWS Free Tier and store security configuration directly on the server through the `.env` file to minimize operating costs.

After the system becomes stable, actual costs will be continuously monitored through AWS Budgets. The cost estimation may be recalculated using AWS Pricing Calculator if actual user traffic exceeds the initial estimate.

---

## 2.7 Risks

### Risk Matrix

* **EC2 instance failure:** High impact, low probability.
* **MongoDB Atlas connection failure:** High impact, low probability.
* **Receipt upload failure:** Medium impact, low probability.
* **Secret information exposure:** High impact, medium probability.
* **Incorrect Security Group configuration:** High impact, low probability.
* **Exceeding the AWS Free Tier budget:** Medium impact, medium probability.

### Mitigation Strategies

* Configure CloudWatch and SNS to monitor the status of the EC2 instance.
* Configure AWS Budgets to send alerts when costs exceed the defined threshold.
* Use IAM Roles instead of Access Keys when accessing AWS services.
* Configure Security Groups according to the principle of opening only the required ports.
* Periodically verify the connection between Amazon EC2 and MongoDB Atlas.

### Contingency Plan

* Restart or redeploy the EC2 instance from the source code when an incident occurs.
* Recover receipt files from Amazon S3.
* Restore the application configuration and source code from the GitHub repository.
* Use MongoDB Atlas Backup if a database failure occurs.
* Adjust service configurations or resource limits when infrastructure costs exceed the budget.
