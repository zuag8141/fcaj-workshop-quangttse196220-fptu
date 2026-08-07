---
title: "Week 8 Worklog"
date: "2026-07-11"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Objectives for Week 8:

* Learn about database systems on AWS: RDS, Aurora, Redshift, ElastiCache.
* Practice building a database subnet group, test connectivity, backup & restore.
* Learn data analytics services such as Kinesis, Glue, Athena, QuickSight.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                    | Start Date | Completion Date | Reference Materials                                                                                                                                                        |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | - Learn about Databases: RDS, Aurora, Redshift, ElastiCache <br> - Learn about Multi-AZ architecture, read replicas, backup/restore                                                                                                                                                                                                     | 07/07/2026 | 07/07/2026 | <https://youtu.be/OOD2RwWuLRw?si=9JsOs0PNfO1TdAUl> <br><br> <https://youtu.be/qbrobQZrokY?si=ePJjzYXWg3qE_Ca6> <br><br> <https://youtu.be/UvdiRW34aNI?si=8g3FwgsJ3VLT-_nf> |
| 3   | - **Practice:** <br>&emsp; + Create VPC + SG for EC2 + RDS <br>&emsp; + Create DB subnet group <br>&emsp; + Deploy EC2 <br>&emsp; + Create RDS instance + Backup & Restore                                                                                                                                                              | 08/07/2026 | 08/07/2026 | <https://000005.awsstudygroup.com>                                                                                                                                         |
| 4   | - **Practice:** <br>&emsp; + Connect to MSSQL/Oracle <br>&emsp; + Schema Conversion <br>&emsp; + Create DMS Task <br>&emsp; + Inspect logs, troubleshoot                                                                                                                                                                                | 09/07/2026 | 09/07/2026 | <https://000043.awsstudygroup.com>                                                                                                                                         |
| 5   | - Learn about Data Analytics (Kinesis, Glue, Athena, QuickSight) <br> - **Practice:** <br>&emsp; + Create DynamoDB table <br>&emsp; + Enable autoscaling <br>&emsp; + CRUD test <br>&emsp; + Create Global Table and clean up resources                                                                                                 | 10/07/2026 | 10/07/2026 | <https://000039.awsstudygroup.com>                                                                                                                                         |
| 6   | - **Practice (lab35):** <br>&emsp; + Create S3 bucket <br>&emsp; + Create Kinesis Firehose ingestion + Glue crawler <br>&emsp; + Query data with Athena + Create QuickSight dashboard <br> - **Practice (lab40):** <br>&emsp; + Check cost allocation <br>&emsp; + Tagging resources <br>&emsp; + Additional queries & resource cleanup | 11/07/2026 | 11/07/2026 | <https://000035.awsstudygroup.com> <br><br> <https://000040.awsstudygroup.com>                                                                                             |

### Week 8 Achievements:

* **Overview:**  
  * This week, I focused on AWS database and data analytics services, including RDS, Aurora, DynamoDB, DMS, Kinesis, Glue, Athena, and QuickSight. I gained a solid understanding of database architecture, connectivity, backup/restore, autoscaling, as well as the end-to-end data analytics pipeline from ingestion -> ETL -> query -> visualization.
  
* **Theory Learned:**
  * Concepts of RDS, Aurora architecture, Multi-AZ, read replicas  
  * Backup, snapshot, parameter group, option group  
  * DynamoDB: partition key, sort key, throughput, autoscaling, DAX  
  * Overview of Data Analytics: Kinesis Firehose, Glue crawler, Athena, QuickSight  
  * Database Migration concepts: schema conversion, DMS task  
  
* **Lab Practice:**
  * Created VPC + security groups for EC2/RDS  
  * Created DB subnet group, deployed EC2 and RDS MySQL  
  * Performed Backup & Restore  
  * Connected to MSSQL/Oracle, practiced Schema Conversion & created DMS task  
  * Created DynamoDB table, enabled autoscaling, CRUD test, created Global Table & cleanup  
  * Built analytics pipeline: Kinesis Firehose -> S3, Glue crawler, Athena queries, and QuickSight dashboard  
  * Performed additional tasks: tagging, cost allocation, and resource cleanup
