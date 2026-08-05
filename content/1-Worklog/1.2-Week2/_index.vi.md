---
title: "Worklog Tuần 2"
date: "2026-05-30"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Hiểu cơ chế quản lý danh tính và phân quyền trên AWS với IAM.
* Nắm được các thành phần mạng cơ bản trong Amazon VPC.
* Làm quen với Amazon S3 và triển khai website tĩnh.
* Hiểu vai trò của Route 53 và CloudFront trong việc phân phối nội dung.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu AWS Identity and Access Management (IAM) <br>&emsp; + User <br>&emsp; + Group <br>&emsp; + Role <br>&emsp; + Policy <br> - **Thực hành:** <br>&emsp; + Tạo IAM User <br>&emsp; + Gán quyền bằng Policy | 26/05/2026 | 26/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Tìm hiểu Amazon Virtual Private Cloud (VPC) <br>&emsp; + CIDR <br>&emsp; + Public/Private Subnet <br>&emsp; + Internet Gateway <br>&emsp; + Route Table | 27/05/2026 | 27/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Tìm hiểu Amazon S3 <br>&emsp; + Bucket <br>&emsp; + Object <br>&emsp; + Bucket Policy <br> - **Thực hành:** <br>&emsp; + Tạo S3 Bucket <br>&emsp; + Upload dữ liệu <br>&emsp; + Host Static Website | 28/05/2026 | 28/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Tìm hiểu Amazon Route 53 <br> - Tìm hiểu Amazon CloudFront <br> - Tìm hiểu Lambda@Edge và các trường hợp sử dụng | 29/05/2026 | 29/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - **Thực hành:** <br>&emsp; + Triển khai Website tĩnh trên Amazon S3 <br>&emsp; + Phân phối nội dung bằng CloudFront <br>&emsp; + Kiểm tra khả năng truy cập và hiệu năng | 30/05/2026 | 30/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 2:

* Hiểu mô hình quản lý danh tính và phân quyền của AWS thông qua IAM.

* Nắm được chức năng của các thành phần trong IAM:
  * User
  * Group
  * Role
  * Policy

* Tạo thành công IAM User và áp dụng chính sách phân quyền phù hợp cho từng tài khoản.

* Hiểu nguyên tắc **least privilege** và tầm quan trọng của việc giới hạn quyền truy cập.

* Hiểu cấu trúc mạng cơ bản trong Amazon VPC, bao gồm:
  * VPC
  * CIDR Block
  * Public Subnet
  * Private Subnet
  * Route Table
  * Internet Gateway

* Hiểu vai trò của Amazon S3 trong việc lưu trữ dữ liệu và triển khai website tĩnh.

* Tạo và quản lý S3 Bucket thành công, bao gồm:
  * Upload và xóa Object.
  * Cấu hình Bucket Policy.
  * Thiết lập Static Website Hosting.

* Hiểu nguyên lý hoạt động của Amazon CloudFront và lợi ích của CDN trong việc giảm độ trễ khi truy cập nội dung.

* Tìm hiểu chức năng của Amazon Route 53 trong việc quản lý DNS và định tuyến tên miền.

* Có kiến thức cơ bản về Lambda@Edge và khả năng xử lý request tại Edge Location.

* Triển khai thành công website tĩnh bằng Amazon S3 kết hợp Amazon CloudFront và kiểm tra khả năng truy cập từ trình duyệt.

* Bước đầu hiểu cách kết hợp nhiều dịch vụ AWS để xây dựng một hệ thống web có khả năng mở rộng và phân phối nội dung hiệu quả.