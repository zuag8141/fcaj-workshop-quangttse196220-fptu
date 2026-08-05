---
title: "Blog 1"
date: "2026-07-17"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# How Generali Malaysia Optimizes Operations with Amazon EKS

> **Original article:** [How Generali Malaysia optimizes operations with Amazon EKS](https://aws.amazon.com/blogs/architecture/how-generali-malaysia-optimizes-operations-with-amazon-eks/)

**Authors:** Antoine Boucherie and Ivan Amemoutou  
**Published:** March 23, 2026

## Introduction

The insurance industry is increasingly adopting cloud computing to expand digital services and meet customers’ expectations for seamless experiences across different channels.

Generali Malaysia faced two major requirements:

- Migrating legacy applications to the cloud
- Developing and scaling new digital insurance services
- Improving application portability and scalability
- Reducing infrastructure management effort
- Maintaining security and compliance
- Supporting business growth with a lean operations team

To address these requirements, Generali adopted a containerized microservices architecture and selected [Amazon Elastic Kubernetes Service (Amazon EKS)](https://aws.amazon.com/eks/) as the main container platform for its modernized applications.

Generali began its AWS migration journey in 2019. Amazon EKS was selected because it provides enterprise-grade Kubernetes management and integrates closely with other AWS services. The Generali DevOps and Cloud team also had previous experience operating Kubernetes environments.

Today, Generali hosts digital applications and several core insurance solutions on Amazon EKS clusters. The organization uses EKS Auto Mode and other AWS services to improve performance, strengthen security, optimize infrastructure costs, and reduce operational overhead.

---

## Solution Overview

Generali designed its Amazon EKS environment according to the [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/).

The implementation considers the six Well-Architected pillars:

1. Operational Excellence
2. Security
3. Reliability
4. Performance Efficiency
5. Cost Optimization
6. Sustainability

Applying these principles helps Generali improve system resilience through automation and monitoring, strengthen security through [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/) and network policies, optimize costs through right-sizing and automatic scaling, and minimize unnecessary resource consumption.

![Architecture ](/aws_internship/images/blog1/blog1.png)

The solution provides several major benefits:

- Simplified management of multiple containerized applications
- Automated node provisioning and scaling
- Integrated security controls
- Better resource utilization
- Simplified cost allocation
- Granular observability for multiple projects and tenants
- Reduced infrastructure maintenance effort

---

## Operational Excellence, Reliability, and Performance Efficiency

### Challenges of Managing Containerized Applications

As Generali expanded its portfolio of containerized applications, the number of workloads, tenants, and application teams increased.

This growth created several operational challenges:

- Manual orchestration and scaling
- Infrastructure maintenance
- Node provisioning and replacement
- Kubernetes and operating system upgrades
- Inconsistent security configurations
- Over-provisioned compute resources
- Difficulty allocating costs to individual business projects
- Increasing operational workload for the DevOps team

Generali needed to scale Kubernetes adoption while maintaining a lean operational structure.

---

## Amazon EKS Auto Mode

Generali adopted [Amazon EKS Auto Mode](https://aws.amazon.com/blogs/containers/under-the-hood-amazon-eks-auto-mode/) to automate the management of its Kubernetes infrastructure.

EKS Auto Mode automatically manages:

- Worker nodes
- Compute capacity
- Load balancers
- Storage configuration
- Node provisioning
- Cluster scaling
- Default Amazon EKS add-ons
- Operating system patching
- Cluster infrastructure upgrades

EKS Auto Mode dynamically adjusts resources according to workload demand. It selects capacity from approved [Amazon EC2](https://aws.amazon.com/ec2/) instance types configured in the EKS node pools.

Under the [expanded shared responsibility model for EKS Auto Mode](https://docs.aws.amazon.com/eks/latest/userguide/automode.html), AWS performs more infrastructure management tasks than it does for conventional EKS clusters.

These tasks include:

- Patching the Bottlerocket operating system
- Managing default EKS add-ons
- Replacing outdated nodes
- Applying updated Amazon Machine Images
- Supporting cluster upgrades
- Adjusting compute capacity

This automation allows the Generali DevOps and Cloud team to focus more on application teams rather than routine Kubernetes maintenance.

---

## Controlling Workload Disruptions

EKS Auto Mode regularly releases updated Amazon Machine Images and replaces existing nodes with updated versions.

Because node replacement can interrupt running workloads, Generali implemented disruption controls.

These controls include:

- Scheduling maintenance during off-peak hours
- Configuring [Pod Disruption Budgets](https://docs.aws.amazon.com/eks/latest/best-practices/application.html)
- Configuring [Node Disruption Budgets](https://docs.aws.amazon.com/eks/latest/userguide/create-node-pool.html)
- Maintaining multiple replicas of critical microservices
- Preventing all pods of the same service from being terminated simultaneously

These controls allow the platform to receive infrastructure updates while maintaining application availability.

---

## Application Reliability Principles

Generali follows several principles to improve the reliability of its Kubernetes workloads:

- Only stateless microservices are hosted in the EKS clusters.
- Pods are treated as immutable components.
- Running pods are replaced rather than modified directly.
- Helm charts are used as the standard deployment mechanism.
- Horizontal Pod Autoscaler is used to scale applications according to traffic.
- Persistent state is separated from the application containers.
- Critical applications maintain multiple pod replicas.

This approach makes applications easier to scale, replace, recover, and upgrade.

---

# Security Architecture

Generali combines several AWS security services to protect its EKS environment:

- [Amazon GuardDuty](https://aws.amazon.com/guardduty/)
- [Amazon Inspector](https://aws.amazon.com/inspector/)
- [AWS Network Firewall](https://aws.amazon.com/network-firewall/)
- [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/)

---

## Amazon GuardDuty Extended Threat Detection

Generali implemented [Amazon GuardDuty Extended Threat Detection for Amazon EKS](https://aws.amazon.com/blogs/aws/amazon-guardduty-expands-extended-threat-detection-coverage-to-amazon-eks-clusters/) to identify sophisticated, multi-stage attacks.

GuardDuty analyzes and correlates signals from:

- Amazon EKS audit logs
- Container runtime behavior
- Malware execution
- AWS API activity
- Container exploitation attempts
- Privilege escalation
- Unauthorized movement between resources

Instead of treating security findings as unrelated events, GuardDuty can connect them into a broader attack sequence.

The detected activity can also be mapped to MITRE ATT&CK tactics and techniques.

### Benefits

Generali receives several benefits from GuardDuty:

- Reduced security investigation time
- Consolidated findings and attack timelines
- Faster identification of affected infrastructure
- Better prioritization of critical risks
- Improved detection of attacks targeting containers
- Reduced potential blast radius
- Faster incident response

---

## Amazon Inspector

Generali uses [Amazon Inspector](https://aws.amazon.com/inspector/) to manage vulnerabilities in its container images.

The organization also uses Inspector’s capability to [map Amazon ECR images to running containers](https://aws.amazon.com/blogs/aws/amazon-inspector-enhances-container-security-by-mapping-amazon-ecr-images-to-running-containers/).

This capability provides information such as:

- Which images are currently running
- The EKS clusters in which they are running
- Cluster Amazon Resource Names
- The number of pods using each image
- The last date on which the image was used
- Vulnerability findings related to the image

This information allows the security team to prioritize vulnerabilities affecting active workloads instead of treating every image in the repository with the same urgency.

### Benefits

- Prioritization based on real container usage
- Better visibility across EKS environments
- Faster remediation of relevant vulnerabilities
- Reduced effort spent on unused images
- More accurate container vulnerability management

---

## AWS Network Firewall

Generali uses [AWS Network Firewall](https://aws.amazon.com/network-firewall/) to control outbound HTTPS connections from applications running in its EKS clusters.

The architecture follows the approach described in the AWS Security Blog article about [filtering outbound HTTPS traffic from Amazon EKS with AWS Network Firewall](https://aws.amazon.com/blogs/security/use-aws-network-firewall-to-filter-outbound-https-traffic-from-applications-hosted-on-amazon-eks/).

The EKS clusters are deployed in private subnets. Outbound traffic is routed through Network Firewall endpoints and NAT gateways before reaching the internet.

Server Name Indication, or SNI, is used to permit connections only to hostnames included in an approved allow list.

### Benefits

- Applications can access only approved external services.
- Security policies are based on hostnames rather than changing IP addresses.
- Unauthorized outbound connections can be blocked.
- Accessed hostnames can be recorded and analyzed.
- Outbound traffic patterns can be monitored through [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/).
- The architecture supports security and compliance requirements.

---

## AWS Secrets Manager

Application credentials, API keys, passwords, and other sensitive values should not be hard-coded into Kubernetes deployment templates.

Generali stores this sensitive information in [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/).

The company uses the [External Secrets Operator](https://aws.amazon.com/blogs/containers/leverage-aws-secrets-stores-from-eks-fargate-with-external-secrets-operator/) in its EKS clusters.

The operator performs the following process:

1. Retrieves secrets from AWS Secrets Manager.
2. Creates or updates the corresponding Kubernetes secrets.
3. Makes those secrets available to pods.
4. Exposes the values through environment variables when required.
5. Synchronizes updated values when credentials are rotated.

### Benefits

- Centralized secret management
- No hard-coded credentials
- Improved auditing and traceability
- Automatic synchronization
- Support for credential rotation
- No requirement to modify application code
- Reduced operational complexity
- Secret management outside the Kubernetes cluster

---

# Cost Optimization

## Cost Allocation with Tags

Although EKS Auto Mode automatically improves resource utilization, Generali still needs to understand how much each project, application, and business unit costs.

Generali uses [AWS Billing split cost allocation data for Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/cost-monitoring.html) together with [AWS Billing](https://aws.amazon.com/aws-cost-management/aws-billing/).

The company analyzes Kubernetes costs alongside other AWS expenses.

Cost allocation can use Kubernetes-related tags such as:

```text
aws:eks:cluster-name
aws:eks:deployment
aws:eks:namespace
aws:eks:node