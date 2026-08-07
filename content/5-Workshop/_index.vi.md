---
title: "Workshop"
date: 2026-08-03
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
# Triển khai ứng dụng Splitly trên AWS bằng CloudFormation và EC2

#### Tổng quan

Trong workshop này, chúng ta sẽ triển khai ứng dụng **Splitly** lên AWS bằng **AWS CloudFormation** để tự động tạo hạ tầng cần thiết.

Sau khi hạ tầng được khởi tạo, chúng ta sẽ kết nối đến máy chủ **Amazon EC2** thông qua **AWS Systems Manager Session Manager** và triển khai cả backend lẫn frontend của ứng dụng.

Các công việc chính trong workshop:

+ Triển khai hạ tầng bằng **AWS CloudFormation**.
+ Kết nối EC2 bằng **Session Manager**.
+ Clone mã nguồn Splitly từ GitHub.
+ Cài đặt dependencies và build backend.
+ Chạy backend bằng **PM2**.
+ Build frontend.
+ Cấu hình **Nginx** để phục vụ frontend và chuyển tiếp API đến backend.
+ Kiểm tra toàn bộ hệ thống.
+ Xóa CloudFormation stack sau khi hoàn thành để tránh phát sinh chi phí.

#### Nội dung

1. [Tổng quan về Workshop](5.1-Workshop-overview/)
2. [Các bước chuẩn bị](5.2-Prerequiste/)
3. [Deploy mã nguồn và cấu hình Web Server](5.3-DeployCode-WebServer/)
4. [Kiểm tra hệ thống](5.4-Test/)
5. [Dọn dẹp tài nguyên](5.5-Cleanup/)
