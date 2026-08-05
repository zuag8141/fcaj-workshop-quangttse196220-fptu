---
title: "Worklog Tuần 6"
date: "2026-06-27"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu tuần 6:

* Tìm hiểu tổng quan về các dịch vụ lưu trữ AWS (S3, Glacier, Backup, Storage Gateway, Snow Family).
* Hiểu cách S3 hoạt động: access point, storage class, CORS, static website hosting.
* Thực hành toàn bộ quy trình với S3, Backup, Storage Gateway và file system.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu tổng quan về các dịch vụ lưu trữ AWS: S3, EBS, Backup, Storage Gateway, Snow Family <br> - Học Access Point, Storage Class và mô hình truy cập dữ liệu <br> - Hiểu S3 static website, CORS, Object key, Glacier | 23/06/2026 | 23/06/2026 | <https://youtu.be/hsCfP0IxoaM?si=O3vMWs7Trr1fugJD> <br><br> <https://youtu.be/_yunukwcAwc?si=ZhkTKr-_OkyUNImI> <br><br> <https://youtu.be/mPBjB6Ltl_Q?si=qs6j0n7AeD2Mxwbz> <br><br> <https://youtu.be/YXn8Q_Hpsu4?si=XojTnkR_LLC1KwEv> |
| 3 | **Thực hành:** <br>&emsp; + Tạo S3 bucket <br>&emsp; + Triển khai hạ tầng backup <br>&emsp; + Tạo backup plan và cấu hình thông báo <br>&emsp; + Kiểm tra restore và dọn dẹp tài nguyên backup | 24/06/2026 | 24/06/2026 | <https://000013.awsstudygroup.com> |
| 4 | - Tìm hiểu VMware Workstation <br> - **Thực hành:** <br>&emsp; + Export VM từ on-prem <br>&emsp; + Upload VM lên AWS <br>&emsp; + Import thành EC2 <br>&emsp; + Export ngược lại thành AMI <br>&emsp; + Dọn dẹp môi trường import/export | 25/06/2026 | 25/06/2026 | <https://000014.awsstudygroup.com> |
| 5 | - **Thực hành:** <br>&emsp; + Tạo Storage Gateway <br>&emsp; + Tạo advanced File Share <br>&emsp; + Kết nối File Share từ máy on-prem <br>&emsp; + Dọn dẹp Storage Gateway + File Shares | 26/06/2026 | 26/06/2026 | <https://000024.awsstudygroup.com> |
| 6 | - **Thực hành (lab25):** <br>&emsp; + Tạo FSx file system (SSD/HDD, Multi-AZ) <br>&emsp; + Tạo và cấu hình file shares <br>&emsp; + Kiểm tra và theo dõi hiệu năng <br>&emsp; + Quản lý user sessions + quotas <br> - **Thực hành (lab57):** <br>&emsp; + Tạo bucket, upload dữ liệu, bật static website <br>&emsp; + Cấu hình public access + object permissions <br>&emsp; + Tạo và cấu hình CloudFront distribution <br>&emsp; + Bật versioning & object replication <br> - Dọn dẹp môi trường (lab25), bucket, CloudFront, replication | 27/06/2026 | 27/06/2026 | <https://000025.awsstudygroup.com> <br><br> <https://000057.awsstudygroup.com> |

### Kết quả đạt được tuần 6:

* Trong tuần này, tôi có cái nhìn rõ hơn về hệ sinh thái lưu trữ của AWS, bao gồm S3, Glacier, AWS Backup, Storage Gateway và các hệ thống file. Tôi tập trung nhiều vào thực hành để hiểu quy trình quản lý dữ liệu, backup/restore và cơ chế lưu trữ trong AWS.

* **Lý thuyết đã học:**
  * Khái niệm S3 Storage Class, Access Point và CORS.
  * Kiến thức về Glacier, lifecycle policies và AWS backup.
  * Kiến trúc và hoạt động của AWS Storage Gateway và các dịch vụ file system.
  * Quy trình import/export máy ảo lên AWS.

* **Thực hành:**
  * Backup & restore
  * Import VM on-prem vào AWS
  * Tạo file system Multi-AZ
  * Xây dựng website tĩnh bằng S3, tích hợp CloudFront, bật versioning và replication.
  * Làm việc với Storage Gateway: tạo file share, kết nối từ on-prem, kiểm tra truyền dữ liệu giữa on-prem và AWS.