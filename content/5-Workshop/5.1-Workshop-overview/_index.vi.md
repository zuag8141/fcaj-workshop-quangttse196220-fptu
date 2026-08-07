---
title : "Giới thiệu"
date : 2026-08-01
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Giới thiệu về Splitly

* **Splitly** là nền tảng quản lý và chia sẻ chi phí nhóm, giúp người dùng theo dõi các khoản chi tiêu chung, tính toán số tiền mỗi thành viên cần trả và quản lý lịch sử giao dịch một cách minh bạch.

* Hệ thống dùng kiến trúc web hiện đại: frontend **React + Vite**, backend **Node.js/Express** và cơ sở dữ liệu **MongoDB Atlas**. Ứng dụng được triển khai trên AWS để tận dụng khả năng mở rộng, bảo mật và giám sát.

* **Amazon EC2** lưu trữ backend API và frontend, **Amazon S3** lưu hóa đơn và biên lai, còn **Amazon CloudWatch** thu thập log và giám sát hệ thống. **Amazon VPC** và **Security Group** đảm bảo kết nối mạng an toàn giữa các thành phần.

#### Tổng quan về hệ thống

Hệ thống Splitly gồm các thành phần chính:

* **Frontend Application**

  * Xây dựng bằng React + Vite.
  * Cung cấp giao diện quản lý nhóm, khoản chi và trạng thái thanh toán.

* **Backend Application**

  * Dùng Node.js/Express để cung cấp REST API.
  * Xử lý nghiệp vụ như tạo nhóm, quản lý expense, tính toán settlement và xác thực người dùng.

* **Database**

  * Dùng MongoDB Atlas để lưu người dùng, nhóm, giao dịch và lịch sử thanh toán.

* **Cloud Storage**

  * Amazon S3 lưu hình ảnh, hóa đơn điện tử và biên lai người dùng tải lên.

* **Monitoring & Security**

  * Amazon CloudWatch thu thập log và giám sát hệ thống.
  * Amazon VPC, Security Group và AWS IAM kiểm soát kết nối mạng và quyền truy cập tài nguyên.

Kiến trúc nâng cấp giúp Splitly cải thiện **hiệu suất**, **bảo mật** và **khả năng mở rộng** nhờ tách riêng frontend và backend:

+ Frontend được lưu trên **Amazon S3** và phân phối qua **Amazon CloudFront**.
+ **Amazon Route 53** quản lý tên miền.
+ **AWS Certificate Manager** cung cấp chứng chỉ SSL/TLS cho HTTPS.
+ **AWS WAF** bảo vệ ứng dụng khỏi các yêu cầu độc hại.
+ Backend tiếp tục chạy trên **Amazon EC2** và kết nối với **MongoDB Atlas**.

Kiến trúc này tạo nền tảng để mở rộng backend và tích hợp thêm dịch vụ AWS trong tương lai mà không cần thay đổi lớn.
