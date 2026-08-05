---
title: "Worklog Tuần 3"
date: "2026-06-06"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 3:

* Hiểu các dịch vụ cơ sở dữ liệu trên AWS và lựa chọn dịch vụ phù hợp với từng bài toán.
* Làm quen với Amazon RDS, Amazon DynamoDB và Amazon ElastiCache.
* Tìm hiểu Amazon Lightsail và Lightsail Containers để triển khai ứng dụng đơn giản.
* Thực hành triển khai và kết nối cơ sở dữ liệu trên AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu Amazon Relational Database Service (RDS) <br>&emsp; + Database Engine <br>&emsp; + DB Instance <br>&emsp; + Backup & Snapshot <br>&emsp; + Multi-AZ | 02/06/2026 | 02/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - **Thực hành:** <br>&emsp; + Khởi tạo Amazon RDS <br>&emsp; + Kết nối cơ sở dữ liệu từ ứng dụng hoặc SQL Client <br>&emsp; + Thực hiện các thao tác CRUD cơ bản | 03/06/2026 | 03/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Tìm hiểu Amazon DynamoDB <br>&emsp; + Table <br>&emsp; + Partition Key <br>&emsp; + Sort Key <br>&emsp; + Capacity Mode <br> - Thực hành tạo bảng và thao tác dữ liệu | 04/06/2026 | 04/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Tìm hiểu Amazon ElastiCache <br>&emsp; + Redis <br>&emsp; + Memcached <br> - Tìm hiểu Amazon Lightsail và Amazon Lightsail Containers | 05/06/2026 | 05/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - **Thực hành:** <br>&emsp; + Triển khai ứng dụng trên Amazon Lightsail <br>&emsp; + Làm quen với Lightsail Containers <br>&emsp; + So sánh Lightsail và EC2 | 06/06/2026 | 06/06/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 3:

* Hiểu sự khác biệt giữa cơ sở dữ liệu quan hệ (SQL) và cơ sở dữ liệu NoSQL.

* Nắm được các trường hợp sử dụng của Amazon RDS và các database engine được hỗ trợ như:
  * MySQL
  * PostgreSQL
  * MariaDB
  * Microsoft SQL Server

* Triển khai thành công một Amazon RDS Database Instance và kết nối đến cơ sở dữ liệu từ ứng dụng hoặc công cụ quản lý.

* Hiểu vai trò của Backup, Snapshot và Multi-AZ trong việc đảm bảo tính sẵn sàng và an toàn dữ liệu.

* Hiểu nguyên lý hoạt động của Amazon DynamoDB và mô hình lưu trữ dữ liệu theo Key-Value.

* Thực hiện được các thao tác cơ bản trên DynamoDB:
  * Tạo Table.
  * Thêm dữ liệu.
  * Truy vấn dữ liệu.
  * Cập nhật dữ liệu.
  * Xóa dữ liệu.

* Hiểu vai trò của Amazon ElastiCache trong việc tăng hiệu năng hệ thống thông qua cơ chế lưu trữ dữ liệu trên bộ nhớ đệm.

* Phân biệt được Redis và Memcached cùng các trường hợp sử dụng phổ biến.

* Làm quen với Amazon Lightsail và hiểu được ưu điểm của dịch vụ đối với các ứng dụng nhỏ hoặc website cá nhân.

* Triển khai thành công một ứng dụng đơn giản trên Amazon Lightsail và tìm hiểu quy trình triển khai bằng Lightsail Containers.

* So sánh được sự khác nhau giữa Amazon EC2 và Amazon Lightsail về khả năng cấu hình, chi phí và mức độ quản trị.

* Có khả năng lựa chọn dịch vụ cơ sở dữ liệu và nền tảng triển khai phù hợp với yêu cầu của từng ứng dụng.