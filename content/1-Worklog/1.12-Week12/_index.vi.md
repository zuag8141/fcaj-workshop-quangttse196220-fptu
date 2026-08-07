---
title: "Worklog Tuần 12"
date: "2026-08-06"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
### Mục tiêu tuần 12:

* Nghiên cứu và thiết kế kiến trúc AWS cho môi trường phát triển và kiểm thử của dự án.
* Xác định luồng kết nối giữa frontend, backend, storage system và MongoDB Atlas.
* Triển khai frontend và backend của dự án lên AWS theo kiến trúc đã thiết kế.
* Cấu hình networking, security, secret management và quyền truy cập.
* Thiết lập monitoring, alert và theo dõi chi phí AWS.
* Kiểm thử lại hệ thống sau khi triển khai lên AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Phân tích yêu cầu triển khai dự án cho môi trường AWS development/testing <br> - Xác định các thành phần chính: S3 Frontend Bucket, S3 Receipts Bucket, VPC, Public Subnet, Internet Gateway, EC2, Elastic IP, MongoDB Atlas <br> - Phân tích luồng truy cập từ người dùng đến frontend và backend <br> - Chọn AWS Region `ap-southeast-1` để triển khai tài nguyên | 01/08/2026 | 01/08/2026 | |
| 3 | - Nghiên cứu và vẽ sơ đồ kiến trúc tổng thể cho môi trường development/testing <br>&emsp; + Thiết kế frontend đặt trong S3 Frontend Bucket <br>&emsp; + Thiết kế backend triển khai trên EC2 trong Public Subnet <br>&emsp; + Kết nối Public Subnet với Internet Gateway <br>&emsp; + Gán Elastic IP cho EC2 <br>&emsp; + Thiết kế S3 Receipts Bucket để lưu ảnh receipt <br>&emsp; + Minh họa kết nối giữa backend EC2 và MongoDB Atlas <br> - Rà soát và hoàn thiện sơ đồ kiến trúc trước khi triển khai | 02/08/2026 | 02/08/2026 | |
| 4 | - Triển khai hạ tầng mạng và backend trên AWS <br>&emsp; + Tạo VPC và Public Subnet <br>&emsp; + Tạo và gắn Internet Gateway vào VPC <br>&emsp; + Cấu hình route table cho Public Subnet truy cập Internet <br>&emsp; + Tạo Web Security Group và giới hạn cổng truy cập <br>&emsp; + Khởi chạy EC2 instance và gán Elastic IP <br>&emsp; + Cấu hình runtime backend trên EC2 <br>&emsp; + Triển khai source code backend và test API | 03/08/2026 | 03/08/2026 | |
| 5 | - Triển khai frontend và storage system trên AWS <br>&emsp; + Tạo S3 Frontend Bucket để lưu file build của frontend <br>&emsp; + Build và upload frontend application lên S3 <br>&emsp; + Tạo S3 Receipts Bucket để lưu ảnh receipt do người dùng upload <br>&emsp; + Tạo IAM Role và cấp quyền cần thiết cho EC2 truy cập S3 Receipts Bucket <br>&emsp; + Dùng Secrets Manager để quản lý thông tin nhạy cảm của backend <br>&emsp; + Cấu hình kết nối từ EC2 đến MongoDB Atlas <br> - Test luồng API từ frontend đến backend và luồng upload ảnh receipt | 04/08/2026 | 04/08/2026 | |
| 6 | - Cấu hình system management, monitoring và alerting <br>&emsp; + Dùng CloudWatch theo dõi metrics EC2 và backend logs <br>&emsp; + Cấu hình CloudWatch alarms cho các metrics cần thiết <br>&emsp; + Kết nối CloudWatch với SNS để gửi thông báo khi alarm kích hoạt <br>&emsp; + Cấu hình AWS Budgets để theo dõi chi phí sử dụng AWS và gửi cost alert <br> - Kiểm thử toàn bộ hệ thống sau khi triển khai <br> - Ghi nhận vấn đề, sửa lỗi và hoàn thiện tài liệu triển khai | 05/08/2026 | 05/08/2026 | |

### Kết quả đạt được tuần 12:

* **Kết quả chung:**
  * Tuần này nhóm tập trung nghiên cứu, thiết kế và triển khai kiến trúc AWS cho môi trường phát triển và kiểm thử của dự án.
  * Hoàn thành bản nháp sơ đồ kiến trúc gồm frontend, backend, storage, database, networking, security, monitoring và cost-management.
  * Deploy xong frontend lên S3 và backend lên EC2 ở region `ap-southeast-1`. Lúc đầu upload ảnh bị chặn vì thiếu CORS giữa S3 và API, thêm header là chạy.
  * Hệ thống có thể kết nối tới MongoDB Atlas, lưu ảnh receipt vào S3 và được giám sát qua CloudWatch.

* **Kiến trúc đã thiết kế:**
  * Dùng AWS Region `ap-southeast-1` để triển khai tài nguyên.
  * Dùng S3 Frontend Bucket để lưu file tĩnh của frontend.
  * Dùng S3 Receipts Bucket để lưu ảnh và file receipt do người dùng upload.
  * Triển khai backend trên EC2 trong Public Subnet thuộc VPC.
  * Dùng Internet Gateway để cho phép tài nguyên trong Public Subnet truy cập Internet.
  * Dùng Elastic IP để cung cấp địa chỉ public cố định cho EC2.
  * Dùng Web Security Group để kiểm soát inbound và outbound traffic cho EC2.
  * Kết nối backend trên EC2 với MongoDB Atlas.
  * Dùng IAM Role để cấp quyền cho EC2 truy cập các dịch vụ AWS cần thiết.
  * Dùng Secrets Manager để quản lý thông tin nhạy cảm của ứng dụng.
  * Dùng CloudWatch, SNS và AWS Budgets để giám sát hệ thống và chi phí AWS.

* **Công việc triển khai đã hoàn thành:**
  * Tạo VPC, Public Subnet, Internet Gateway và route table.
  * Tạo và cấu hình Security Group cho backend.
  * Khởi chạy EC2 instance và gán Elastic IP.
  * Cấu hình runtime và triển khai backend lên EC2.
  * Tạo S3 Frontend Bucket và deploy frontend lên S3.
  * Tạo S3 Receipts Bucket để lưu receipt image.
  * Tạo IAM Role và cấp quyền cho EC2 truy cập S3.
  * Cấu hình backend settings và thông tin nhạy cảm.
  * Kết nối backend tới MongoDB Atlas.
  * Test luồng request từ frontend đến backend.
  * Test chức năng upload và retrieve receipt image bằng S3.
  * Cấu hình CloudWatch để thu thập metrics và logs.
  * Kết nối CloudWatch alarms với SNS.
  * Cấu hình AWS Budgets để theo dõi chi phí sử dụng.

* **Luồng vận hành hệ thống:**
  * User truy cập frontend nằm trong S3 Frontend Bucket.
  * Frontend gửi API request đến backend thông qua Elastic IP của EC2.
  * Internet Gateway hỗ trợ kết nối Internet cho EC2 trong Public Subnet.
  * Backend xử lý logic nghiệp vụ và kết nối tới MongoDB Atlas để đọc/ghi dữ liệu.
  * Backend dùng IAM Role để upload và truy xuất ảnh receipt từ S3 Receipts Bucket.
  * Thông tin nhạy cảm của ứng dụng được quản lý bằng Secrets Manager.
  * CloudWatch thu thập metrics và logs từ EC2.
  * SNS gửi thông báo khi hệ thống có alarm.
  * AWS Budgets theo dõi chi phí và gửi cảnh báo khi chi tiêu vượt ngưỡng.

* **Kiến thức và kinh nghiệm đạt được:**
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