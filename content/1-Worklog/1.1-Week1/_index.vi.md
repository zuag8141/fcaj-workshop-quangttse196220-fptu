---
title: "Worklog Tuần 1"
date: "2026-05-23"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Kết nối, làm quen với các thành viên trong First Cloud AI Journey.
* Hiểu dịch vụ AWS cơ bản, cách dùng console & CLI.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu chương trình First Cloud AI Journey <br> - Đọc quy định và chuẩn bị môi trường học tập | 19/05/2026 | 19/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Tìm hiểu Cloud Computing và AWS <br> - Khám phá các nhóm dịch vụ chính: Compute, Storage, Database, Networking, Security | 20/05/2026 | 20/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Tạo AWS Account <br> - Tìm hiểu AWS Management Console <br> - Cài đặt và cấu hình AWS CLI <br> - **Thực hành:** <br>&emsp; + Đăng nhập AWS Console <br>&emsp; + Cấu hình AWS CLI bằng Access Key | 21/05/2026 | 21/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Tìm hiểu Amazon EC2 <br>&emsp; + Instance Types <br>&emsp; + Amazon Machine Image (AMI) <br>&emsp; + Amazon EBS <br>&emsp; + Security Group <br>&emsp; + Key Pair <br> - Tìm hiểu Elastic IP | 22/05/2026 | 22/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - **Thực hành:** <br>&emsp; + Khởi tạo EC2 Instance <br>&emsp; + Kết nối EC2 bằng SSH <br>&emsp; + Quản lý Security Group <br>&emsp; + Gắn và kiểm tra EBS Volume | 23/05/2026 | 23/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 1:

* Hiểu khái niệm Cloud Computing và những lợi ích của điện toán đám mây so với mô hình hạ tầng truyền thống.

* Nắm được cấu trúc cơ bản của hệ sinh thái AWS và các nhóm dịch vụ chính:
  * Compute
  * Storage
  * Database
  * Networking
  * Security
  * Management & Monitoring

* Đăng ký tài khoản AWS miễn phí để thực hành. Hơi lúng túng ở bước xác minh thanh toán, nhưng xong là vào được console.

* Làm quen với AWS Management Console và biết cách:
  * Tìm kiếm dịch vụ.
  * Truy cập Dashboard.
  * Quản lý Region.
  * Theo dõi tài nguyên đang sử dụng.

* Cài đặt và cấu hình AWS CLI trên máy tính bao gồm:
  * Access Key ID
  * Secret Access Key
  * Default Region
  * Output Format

* Sử dụng AWS CLI để thực hiện một số thao tác cơ bản:
  * Kiểm tra cấu hình (`aws configure list`)
  * Kiểm tra danh tính tài khoản (`aws sts get-caller-identity`)
  * Liệt kê Region
  * Kiểm tra EC2 Instance
  * Quản lý Key Pair

* Hiểu vai trò của Amazon EC2 trong việc cung cấp máy chủ ảo trên nền tảng AWS.

* Nắm được các thành phần chính của EC2:
  * AMI
  * Instance Type
  * EBS Volume
  * Security Group
  * Key Pair
  * Elastic IP

* Tạo được một EC2 `t2.micro` và SSH vào thành công. Lần đầu bị `permission denied` vì file key pair để sai quyền, nhớ ra phải `chmod 400` là chạy.

* Thực hiện thao tác gắn EBS Volume và kiểm tra trạng thái hoạt động của Instance.

* Bắt đầu quen dần với việc quản lý tài nguyên AWS qua cả AWS Management Console và AWS CLI.