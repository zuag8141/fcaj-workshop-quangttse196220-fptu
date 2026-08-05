---
title: "Worklog Tuần 5"
date: "2026-06-20"
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---
### Mục tiêu tuần 5:

* Hiểu các thành phần cốt lõi của EC2 và cách tính toán hoạt động trên AWS.
* Nắm được Auto Scaling, EBS, Instance Store, User Data và Metadata.
* Thực hành backup, Storage Gateway và triển khai EC2 cho các tình huống liên quan đến lưu trữ.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu EC2, instance types, AMI, key pair <br> - Hiểu EBS, Instance Store, User Data, Metadata | 16/06/2026 | 16/06/2026 | <https://youtu.be/-t5h4N6vfBs?si=GeVdhO9IEDjzzS_D> <br><br> <https://youtu.be/e7XeKdOVq40?si=T3I4pgPoEfVytcU3> <br><br> <https://youtu.be/yAR6QRT3N1k?si=GQghyBwLCpijrDON> <br><br> <https://youtu.be/hKr_TfGP7NY?si=gR2MqaLAFrqL-KBo> <br><br> <https://youtu.be/6IHNDJ85aoQ?si=M0puk6DJpliO7ahf> <br><br> <https://youtu.be/_v_43Wi7zjo?si=qNDVWzKcQFNO2mGh> <br><br> <https://youtu.be/Ew3QRaKJQSA?si=xNvXvD8yFhnSMJby> |
| 3 | - Hiểu Auto Scaling của EC2 và cách mở rộng VM <br> - Tìm hiểu các dịch vụ lưu trữ và compute (EFS/FSx, Lightsail, MGN overview) | 17/06/2026 | 17/06/2026 | <https://youtu.be/bbLcPitXJSY?si=eyVnxvL9ho0LpUYy> <br><br> <https://youtu.be/hFVYG8WqfU0?si=9Px4wmR4IRZxk15n> |
| 4 | - **Thực hành:** <br>&emsp; + Triển khai AWS Backup <br>&emsp; + Tạo backup plan <br>&emsp; + Kiểm tra restore & dọn dẹp <br>&emsp; + Xóa tài nguyên backup | 18/06/2026 | 18/06/2026 | <https://000013.awsstudygroup.com> |
| 5 | - **Thực hành:** <br>&emsp; + Tạo S3 bucket cho Storage Gateway <br>&emsp; + Tạo EC2 cho Storage Gateway <br>&emsp; + Tạo Storage Gateway + File Share <br>&emsp; + Dọn dẹp Storage Gateway | 19/06/2026 | 19/06/2026 | <https://000024.awsstudygroup.com> |
| 6 | - **Thực hành:** <br>&emsp; + Tạo bucket, upload dữ liệu <br>&emsp; + Bật static website hosting <br>&emsp; + Cấu hình public access block <br>&emsp; + Cấu hình CloudFront và kiểm tra website <br>&emsp; + Dọn dẹp website + CloudFront + bucket | 20/06/2026 | 20/06/2026 | <https://000057.awsstudygroup.com> |

### Kết quả đạt được tuần 5:

* Trong tuần này, tôi học cách EC2 hoạt động, các kiểu storage của instance, Auto Scaling và cơ chế backup. Tôi cũng thực hành Storage Gateway và triển khai website tĩnh trên S3.

* **Lý thuyết đã học:**
  * Kiến trúc EC2, AMI, key pair
  * EBS và Instance Store
  * User Data / Metadata
  * EC2 Auto Scaling
  * Storage Gateway và nền tảng của AWS Backup

* **Thực hành:**
  * Tạo backup plan và kiểm tra restore
  * Tạo Storage Gateway + file share
  * Triển khai website tĩnh bằng S3 + CloudFront