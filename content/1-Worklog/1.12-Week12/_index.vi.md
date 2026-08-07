---
title: "Worklog Tuần 12"
date: "2026-08-07"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
### Mục tiêu tuần 12:

* Chuẩn bị bản build frontend cho việc triển khai lên AWS.
* Hỗ trợ triển khai frontend lên S3.
* Xác minh frontend sau khi triển khai và kết nối với backend.
* Test các luồng chính của hệ thống trên môi trường đã triển khai.
* Ghi nhận và hỗ trợ sửa các vấn đề phát sinh khi triển khai.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Rà soát kế hoạch triển khai frontend và xác định các bước build | 10/08/2026 | 10/08/2026 | |
| 3 | - Build frontend cho production và sửa các lỗi build | 11/08/2026 | 11/08/2026 | |
| 4 | - Hỗ trợ upload bản build frontend lên S3 bucket và xác minh khả năng truy cập | 12/08/2026 | 12/08/2026 | |
| 5 | - Xác minh luồng API frontend-to-backend và luồng upload receipt trên môi trường đã triển khai | 13/08/2026 | 13/08/2026 | |
| 6 | - Kiểm thử toàn bộ hệ thống sau khi triển khai và ghi nhận các vấn đề | 14/08/2026 | 14/08/2026 | |

### Kết quả đạt được tuần 12:

* **Kết quả chung:**
  * Chuẩn bị và build frontend cho việc triển khai production.
  * Hỗ trợ triển khai frontend lên S3.
  * Xác minh frontend sau khi triển khai và kết nối với backend.
  * Test các luồng chính của hệ thống sau khi triển khai.
  * Hiểu rõ hơn cách thiết kế kiến trúc AWS cho môi trường development và testing.
  * Có kinh nghiệm deploy frontend tĩnh trên Amazon S3.
  * Có kinh nghiệm deploy backend trên EC2 và cấu hình networking trong VPC.
  * Hiểu cách dùng Internet Gateway, route table, Elastic IP và Security Group.
  * Học cách dùng IAM Role để cấp quyền cho EC2 mà không cần lưu Access Keys trong source code.
  * Hiểu cách quản lý thông tin nhạy cảm bằng Secrets Manager.
  * Có kinh nghiệm kết nối ứng dụng trên AWS với MongoDB Atlas.
  * Học cách dùng CloudWatch và SNS cho monitoring và alert.
  * Hiểu cách dùng AWS Budgets để theo dõi và kiểm soát chi phí.
  * Có thêm kinh nghiệm kiểm thử và xử lý lỗi sau khi triển khai lên AWS.