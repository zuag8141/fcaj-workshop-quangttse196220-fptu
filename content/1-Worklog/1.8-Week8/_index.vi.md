---
title: "Worklog Tuần 8"
date: "2026-07-11"
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Tìm hiểu hệ thống cơ sở dữ liệu trên AWS: RDS, Aurora, Redshift, ElastiCache.
* Thực hành tạo database subnet group, kiểm tra kết nối, backup & restore.
* Tìm hiểu các dịch vụ phân tích dữ liệu như Kinesis, Glue, Athena, QuickSight.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu cơ sở dữ liệu: RDS, Aurora, Redshift, ElastiCache <br> - Tìm hiểu Multi-AZ architecture, read replicas, backup/restore | 07/07/2026 | 07/07/2026 | <https://youtu.be/OOD2RwWuLRw?si=9JsOs0PNfO1TdAUl> <br><br> <https://youtu.be/qbrobQZrokY?si=ePJjzYXWg3qE_Ca6> <br><br> <https://youtu.be/UvdiRW34aNI?si=8g3FwgsJ3VLT-_nf> |
| 3 | - **Thực hành:** <br>&emsp; + Tạo VPC + SG cho EC2 + RDS <br>&emsp; + Tạo DB subnet group <br>&emsp; + Triển khai EC2 <br>&emsp; + Tạo RDS instance + Backup & Restore | 08/07/2026 | 08/07/2026 | <https://000005.awsstudygroup.com> |
| 4 | - **Thực hành:** <br>&emsp; + Kết nối MSSQL/Oracle <br>&emsp; + Schema Conversion <br>&emsp; + Tạo DMS Task <br>&emsp; + Kiểm tra logs, xử lý lỗi | 09/07/2026 | 09/07/2026 | <https://000043.awsstudygroup.com> |
| 5 | - Tìm hiểu Data Analytics (Kinesis, Glue, Athena, QuickSight) <br> - **Thực hành:** <br>&emsp; + Tạo DynamoDB table <br>&emsp; + Bật autoscaling <br>&emsp; + CRUD test <br>&emsp; + Tạo Global Table và dọn dẹp tài nguyên | 10/07/2026 | 10/07/2026 | <https://000039.awsstudygroup.com> |
| 6 | - **Thực hành (lab35):** <br>&emsp; + Tạo S3 bucket <br>&emsp; + Tạo Kinesis Firehose ingestion + Glue crawler <br>&emsp; + Query data với Athena + tạo QuickSight dashboard <br> - **Thực hành (lab40):** <br>&emsp; + Kiểm tra cost allocation <br>&emsp; + Tagging resources <br>&emsp; + Truy vấn thêm & dọn dẹp tài nguyên | 11/07/2026 | 11/07/2026 | <https://000035.awsstudygroup.com> <br><br> <https://000040.awsstudygroup.com> |

### Kết quả đạt được tuần 8:

* **Tổng quan:**
  * Tuần này nhóm tập trung vào các dịch vụ database và data analytics của AWS, gồm RDS, Aurora, DynamoDB, DMS, Kinesis, Glue, Athena, và QuickSight. Qua thực hành, nắm rõ hơn kiến trúc database, kết nối, backup/restore, autoscaling và quy trình analytics từ ingestion đến visualization.

* **Lý thuyết đã học:**
  * Khái niệm RDS, Aurora architecture, Multi-AZ, read replicas  
  * Backup, snapshot, parameter group, option group  
  * DynamoDB: partition key, sort key, throughput, autoscaling, DAX  
  * Tổng quan Data Analytics: Kinesis Firehose, Glue crawler, Athena, QuickSight  
  * Database Migration: schema conversion, DMS task  

* **Thực hành:**
  * Tạo VPC + security groups cho EC2/RDS  
  * Tạo DB subnet group, triển khai EC2 và RDS MySQL  
  * Backup & Restore  
  * Kết nối MSSQL/Oracle, thực hành Schema Conversion & tạo DMS task  
  * Tạo DynamoDB table, bật autoscaling, CRUD test, tạo Global Table & dọn dẹp  
  * Xây dựng pipeline analytics: Kinesis Firehose -> S3, Glue crawler, Athena query, và QuickSight dashboard  
  * Thực hiện thêm tagging, cost allocation và dọn dẹp tài nguyên