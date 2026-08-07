---
title: "Week 6 Worklog"
date: "2026-06-27"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Objectives for Week 6:

* Learn the overview of AWS storage services (S3, Glacier, Backup, Storage Gateway, Snow Family).
* Understand how S3 works: access point, storage class, CORS, static website hosting.
* Practice the entire workflow with S3, Backup, Storage Gateway, and File Systems.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Start Date | End Date   | Reference                                                                                                                                                                                                                              |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | - Learn the overview of AWS storage services: S3, EBS, Backup, Storage Gateway, Snow Family <br> - Study Access Point, Storage Class, and data access models <br> - Understand S3 static website, CORS, Object key, Glacier                                                                                                                                                                                                                                                                                                                           | 23/06/2026 | 23/06/2026 | <https://youtu.be/hsCfP0IxoaM?si=O3vMWs7Trr1fugJD> <br><br> <https://youtu.be/_yunukwcAwc?si=ZhkTKr-_OkyUNImI> <br><br> <https://youtu.be/mPBjB6Ltl_Q?si=qs6j0n7AeD2Mxwbz> <br><br> <https://youtu.be/YXn8Q_Hpsu4?si=XojTnkR_LLC1KwEv> |
| 3   | **Hands-on:** <br>&emsp; + Create S3 bucket <br>&emsp; + Deploy backup infrastructure <br>&emsp; + Create backup plan and set notification <br>&emsp; + Test restore & clean up backup resources                                                                                                                                                                                                                                                                                                                                                      | 24/06/2026 | 24/06/2026 | <https://000013.awsstudygroup.com>                                                                                                                                                                                                     |
| 4   | - Learn VMware Workstation <br> - **Hands-on:** <br>&emsp; + Export VM from on-prem <br>&emsp; + Upload VM to AWS <br>&emsp; + Import as EC2 <br>&emsp; + Export back as AMI <br>&emsp; + Clean up import/export environment                                                                                                                                                                                                                                                                                                                          | 25/06/2026 | 25/06/2026 | <https://000014.awsstudygroup.com>                                                                                                                                                                                                     |
| 5   | - **Hands-on:** <br>&emsp; + Create Storage Gateway <br>&emsp; + Create advanced File Share <br>&emsp; + Connect File Share from on-prem machine <br>&emsp; + Clean up Storage Gateway + File Shares                                                                                                                                                                                                                                                                                                                                                  | 26/06/2026 | 26/06/2026 | <https://000024.awsstudygroup.com>                                                                                                                                                                                                     |
| 6   | - **Hands-on (lab25):** <br>&emsp; + Create FSx file system (SSD/HDD, Multi-AZ) <br>&emsp; + Create & configure file shares <br>&emsp; + Test & monitor performance <br>&emsp; + Manage user sessions + quotas <br> - **Hands-on (lab57):** <br>&emsp; + Create bucket, upload data, enable static website <br>&emsp; + Configure public access + object permissions <br>&emsp; + Create & configure CloudFront distribution <br>&emsp; + Enable versioning & object replication <br> - Clean up environment (lab25), bucket, CloudFront, replication | 27/06/2026 | 27/06/2026 | <https://000025.awsstudygroup.com> <br><br> <https://000057.awsstudygroup.com> |

### Week 6 Achievements:

* **Summary:**  
  * This week, I gained a solid understanding of the AWS storage ecosystem, including S3, Glacier, AWS Backup, Storage Gateway, and various file systems. I focused heavily on hands-on practice to understand how data management, backup–restore workflows, and storage mechanisms operate in AWS.

* **Theory learned:**  
  * Concepts of S3 Storage Classes, Access Points, and CORS.
  * Knowledge about Glacier, lifecycle policies, and AWS backup concepts.
  * Architecture and operation of AWS Storage Gateway and AWS file system services.
  * Processes for importing/exporting virtual machines to AWS.

* **Hands-on labs:**  
  * Backup & restore  
  * Import on-prem VM into AWS  
  * Create Multi-AZ file system  
  * Built a static website using S3, integrated CloudFront, enabled versioning and replication.
  * Worked with Storage Gateway: created a file share, connected from on-prem, and tested data transfer between on-prem and AWS.
