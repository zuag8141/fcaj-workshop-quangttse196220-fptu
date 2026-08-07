---
title: "Worklog Tuần 4"
date: "2026-06-13"
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---
### Mục tiêu tuần 4:

* Tìm hiểu cách triển khai VPC Peering giữa nhiều VPC.
* Tìm hiểu cách cấu hình Transit Gateway cho kết nối liên VPC trên quy mô lớn.
* Thực hành các lab nâng cao liên quan đến định tuyến và DNS giữa các VPC.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu nền tảng của VPC Peering: cách hoạt động, giới hạn, hành vi định tuyến <br> - Tìm hiểu khái niệm peering bên trong VPC | 09/06/2026 | 09/06/2026 | https://000019.awsstudygroup.com |
| 3 | - **Thực hành:** <br>&emsp; + Tạo EC2 instances ở nhiều VPC và cấu hình định tuyến <br>&emsp; + Kiểm tra NACL và Security Group khi dùng peering <br>&emsp; + Hoàn thiện cấu hình peering | 10/06/2026 | 10/06/2026 | https://000019.awsstudygroup.com |
| 4 | - Tìm hiểu Transit Gateway <br> - **Thực hành:** <br>&emsp; + Triển khai TGW <br>&emsp; + Thiết lập định tuyến giữa các VPC và TGW | 11/06/2026 | 11/06/2026 | https://000020.awsstudygroup.com |
| 5 | - **Thực hành:** <br>&emsp; + Tạo kết nối Transit Gateway <br>&emsp; + Tạo TGW Route Table <br>&emsp; + Gán route của VPC vào TGW <br>&emsp; + Kiểm tra kết nối giữa nhiều VPC qua TGW | 02/10/2025 | 02/10/2025 | https://000020.awsstudygroup.com |
|   | - Dọn dẹp toàn bộ tài nguyên lab <br> - Hệ thống lại kiến thức về Peering và Transit Gateway | 12/06/2026 | 12/06/2026 | https://000020.awsstudygroup.com |
| 6 |  | 13/06/2026 | 13/06/2026 | |

### Kết quả đạt được tuần 4:

* **Tổng kết:**
  * Trong tuần này, nhóm tìm hiểu các mô hình kết nối nhiều VPC, bao gồm VPC Peering và Transit Gateway. Qua thực hành, nắm rõ hơn cách hoạt động của kết nối liên VPC, cách định tuyến qua TGW và vai trò của từng thành phần trong kiến trúc đa VPC.

* **Lý thuyết đã học:**
  * Cách VPC Peering hoạt động và cách định tuyến giữa hai VPC.
  * Kiến trúc và khả năng của Transit Gateway (TGW) để kết nối nhiều VPC theo mô hình hub-and-spoke.
  * Cách cấu hình route table trong các VPC được kết nối bằng Peering hoặc Transit Gateway.

* **Thực hành:**
  * Tạo kết nối VPC Peering và cập nhật route table để cho phép lưu lượng giữa các VPC.
  * Cấu hình Transit Gateway, tạo attachment TGW và thiết lập định tuyến giữa các mạng đã kết nối.
  * Kiểm tra kết nối giữa các subnet và VPC để xác minh cấu hình.