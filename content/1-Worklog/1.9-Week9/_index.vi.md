---
title: "Worklog Tuần 9"
date: "2026-07-18"
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 9:

* Tìm hiểu các dịch vụ AWS dùng cho frontend, backend, lưu trữ file, giám sát hệ thống và quản lý chi phí.
* Hiểu cách thiết kế hạ tầng mạng và bảo mật cho ứng dụng triển khai trên AWS.
* Phân tích luồng kết nối giữa ứng dụng trên AWS và MongoDB Atlas.
* Vẽ sơ đồ kiến trúc AWS tổng thể phù hợp với dự án.
* Tìm hiểu giải pháp giám sát hiệu năng hệ thống và kiểm soát chi phí AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu kiến trúc tổng thể hệ thống trên AWS <br> - Tìm hiểu các dịch vụ cho frontend: Route 53, CloudFront, S3 Frontend, ACM <br> - Phân tích luồng người dùng qua Route 53, CloudFront và frontend <br> - Tìm hiểu cách ACM cấp chứng chỉ SSL/TLS cho HTTPS | 14/07/2026 | 14/07/2026 | |
| 3 | - Tìm hiểu các thành phần mạng AWS: VPC, Public Subnet, Internet Gateway <br> - Tìm hiểu Security Groups cho EC2 <br> - Hiểu vai trò của Elastic IP <br> - Phân tích mô hình backend chạy trên EC2 ở Public Subnet | 15/07/2026 | 15/07/2026 | |
| 4 | - Tìm hiểu cách dùng S3 Receipts để lưu ảnh và file upload <br> - Tìm hiểu IAM Roles để EC2 truy cập S3 mà không lưu Access Keys trong source code <br> - Phân tích luồng upload/retrieve ảnh giữa frontend, backend EC2 và S3 Receipts <br> - Tìm hiểu kết nối backend EC2 với MongoDB Atlas | 16/07/2026 | 16/07/2026 | |
| 5 | - Tìm hiểu cách CloudWatch giám sát metrics, logs và trạng thái EC2 <br> - Tìm hiểu SNS gửi thông báo khi có lỗi hoặc vượt ngưỡng giám sát <br> - Tìm hiểu AWS Budgets theo dõi chi tiêu và cảnh báo chi phí <br> - Phân tích cách tích hợp CloudWatch, SNS và AWS Budgets vào kiến trúc | 17/07/2026 | 17/07/2026 | |
| 6 | - **Thực hành thiết kế kiến trúc:** <br>&emsp; + Xác định các thành phần chính của hệ thống <br>&emsp; + Vẽ luồng truy cập frontend qua Route 53, CloudFront và S3 Frontend <br>&emsp; + Vẽ luồng request từ frontend đến backend trên EC2 <br>&emsp; + Minh họa kết nối giữa EC2, S3 Receipts và MongoDB Atlas <br>&emsp; + Bổ sung IAM Roles, Security Groups và các thành phần bảo mật khác <br>&emsp; + Bổ sung CloudWatch, SNS và AWS Budgets vào sơ đồ <br> - Hoàn thiện sơ đồ kiến trúc AWS tổng thể cho dự án | 18/07/2026 | 18/07/2026 | |

### Kết quả đạt được tuần 9:

* **Kết quả chung:**
  * Tuần này tôi tập trung nghiên cứu và thiết kế kiến trúc AWS cho việc triển khai dự án.
  * Kiến trúc bao gồm các thành phần cho phân phối frontend, host backend, lưu trữ ảnh, kết nối database, giám sát hệ thống và quản lý chi phí.
  * Tôi hiểu rõ hơn luồng hoạt động của hệ thống, từ lúc người dùng truy cập đến khi request được xử lý và dữ liệu được lưu ở các dịch vụ tương ứng.

* **Kiến thức đã học:**
  * Route 53 dùng để quản lý domain và điều hướng người dùng.
  * CloudFront dùng để phân phối nội dung frontend qua CDN.
  * S3 Frontend dùng để lưu file tĩnh của ứng dụng frontend.
  * ACM dùng để quản lý chứng chỉ SSL/TLS và bật HTTPS an toàn.
  * VPC, Public Subnet và Internet Gateway tạo môi trường mạng cho tài nguyên AWS.
  * Security Groups kiểm soát inbound và outbound traffic của EC2.
  * Elastic IP cung cấp địa chỉ public cố định cho EC2.
  * EC2 dùng để triển khai và vận hành backend của dự án.
  * S3 Receipts dùng để lưu ảnh và file người dùng upload.
  * IAM Roles cho phép EC2 truy cập dịch vụ AWS khác mà không lưu credential trực tiếp.
  * MongoDB Atlas là database của hệ thống và được backend trên EC2 kết nối tới.
  * CloudWatch hỗ trợ giám sát metrics, logs và trạng thái EC2.
  * SNS hỗ trợ gửi thông báo khi có cảnh báo hoặc sự cố.
  * AWS Budgets giúp theo dõi chi tiêu và gửi cảnh báo khi vượt ngưỡng.

* **Thực hành thiết kế kiến trúc:**
  * Xác định và phân loại các thành phần frontend, backend, storage, database, monitoring và cost-management.
  * Thiết kế luồng frontend: User → Route 53 → CloudFront → S3 Frontend.
  * Thiết kế luồng backend: Frontend → Elastic IP → EC2.
  * Đặt EC2 trong Public Subnet thuộc VPC và kết nối Internet qua Internet Gateway.
  * Dùng Security Group để giới hạn cổng và nguồn truy cập EC2.
  * Thiết kế luồng upload và retrieve ảnh từ S3 Receipts thông qua IAM Role.
  * Minh họa kết nối giữa backend trên EC2 và MongoDB Atlas.
  * Thêm CloudWatch để thu thập metrics và application logs.
  * Thêm SNS để gửi thông báo khi hệ thống có sự cố.
  * Thêm AWS Budgets để giám sát và kiểm soát chi phí.
  * Hoàn thiện sơ đồ kiến trúc AWS tổng thể cho việc triển khai dự án.