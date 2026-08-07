---
title : "Introduction"
date : 2026-08-01
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Introduction to Splitly

* **Splitly** is a group expense management and bill-splitting platform that helps users track shared expenses, calculate how much each member has to pay, and manage transaction history transparently.

* The system uses a modern web architecture: a **React + Vite** frontend, a **Node.js/Express** backend, and a **MongoDB Atlas** database. It is deployed on AWS to take advantage of the scalability, security, and monitoring of the cloud.

* **Amazon EC2** hosts the backend API and frontend, **Amazon S3** stores invoices and receipts, and **Amazon CloudWatch** collects logs and monitors system status. **Amazon VPC** and **Security Groups** provide secure network connectivity between components.

#### System Overview

The Splitly system consists of the following main components:

* **Frontend Application**

  * Built with React and Vite.
  * Provides the user interface for managing groups, expenses, and settlement statuses.

* **Backend Application**

  * Uses Node.js and Express to provide REST APIs.
  * Handles business logic such as group creation, expense management, settlement calculation, and user authentication.

* **Database**

  * Uses MongoDB Atlas to store users, groups, transactions, and payment history.

* **Cloud Storage**

  * Amazon S3 stores images, electronic invoices, and receipts uploaded by users.

* **Monitoring & Security**

  * Amazon CloudWatch collects logs and monitors the system.
  * Amazon VPC, Security Groups, and AWS IAM control network connectivity and resource access.

The upgraded architecture improves Splitly's **performance**, **security**, and **scalability** by separating frontend and backend:

+ The frontend is hosted on **Amazon S3** and distributed through **Amazon CloudFront**.
+ **Amazon Route 53** manages the domain name.
+ **AWS Certificate Manager** provides SSL/TLS certificates for HTTPS.
+ **AWS WAF** protects the application from malicious requests.
+ The backend keeps running on **Amazon EC2** and connects to **MongoDB Atlas**.

This architecture provides a foundation to scale the backend and add more AWS services in the future without major changes to the whole system.
