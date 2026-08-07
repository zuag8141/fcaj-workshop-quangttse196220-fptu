---
title: "Worklog Tuần 7"
date: "2026-07-04"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Mục tiêu tuần 7:

* Tìm hiểu hệ thống IAM đầy đủ: user, group, role, policy, permission boundary.
* Hiểu cơ chế xác thực và phân quyền trên AWS, cách viết JSON policy và cách policy evaluation hoạt động.
* Làm quen với AWS Organizations, Organizational Units (OU) và Service Control Policies (SCP).
* Thực hành các lab IAM + Organizations để hiểu cách quản lý tài khoản ở quy mô lớn.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu nền tảng IAM: user, group, role, policy <br> - Hiểu policy evaluation: explicit deny, implicit deny, allow | 30/06/2026 | 30/06/2026 | <https://youtu.be/tsobAlSg19g?si=9f3mlIWPtrCcNuKg> <br><br> <https://youtu.be/N_vlJGAqZxo?si=e8oiWCObco95CoKh> |
| 3 | - **Thực hành:** <br>&emsp; + Tạo user/group/role <br>&emsp; + Gán inline & managed policies <br>&emsp; + Kiểm tra quyền truy cập S3/EC2 theo các policy khác nhau <br> - Tìm hiểu permission boundaries và session policies | 01/07/2026 | 01/07/2026 | <https://000028.awsstudygroup.com> |
| 4 | - Tìm hiểu AWS Organizations: cấu trúc OU, mô hình multi-account <br> - Hiểu khái niệm SCP, deny list và allow list | 02/07/2026 | 02/07/2026 | <https://youtu.be/5oQY8Rogz9Y?si=h8DlUb8ZLI4HbbvM> <br><br> <https://youtu.be/NW1xrMkNMjU?si=dhT0T3y2JYVK8QwT> |
| 5 | - **Thực hành:** <br>&emsp; + Tạo Organization + OU <br>&emsp; + Áp dụng SCP deny EC2 / deny S3 <br>&emsp; + Kiểm tra hiệu lực SCP kết hợp với IAM policies <br>&emsp; + Sắp xếp lại OU, xóa SCP | 03/07/2026 | 03/07/2026 | <https://000030.awsstudygroup.com> <br><br> <https://000044.awsstudygroup.com/> |
| 6 | - **Hoạt động nhóm:** <br>&emsp; + Trao đổi ý tưởng workshop <br>&emsp; + Lên kế hoạch thực hiện <br>&emsp; + Phân chia công việc cho workshop | 04/07/2026 | 04/07/2026 | |

### Kết quả đạt được tuần 7:

* **Tổng kết:**
  * Trong tuần này, nhóm học nền tảng quản lý quyền truy cập trên AWS, bao gồm IAM và Organizations. Qua thực hành, hiểu cách policy evaluation hoạt động, cách viết JSON policy và cách SCP áp dụng trong môi trường nhiều tài khoản.

* **Lý thuyết đã học:**
  * Khái niệm User – Group – Role – Policy và cách evaluation hoạt động
  * Inline policy, managed policy, permission boundary
  * Cấu trúc AWS Organizations, OU
  * Khái niệm SCP và sự khác biệt so với IAM policies
  * Tổng quan về Landing Zone & Control Tower

* **Thực hành:**
  * Tạo user/group/role và kiểm tra các mức quyền truy cập khác nhau
  * Viết JSON policies và kiểm tra hành vi allow/deny
  * Tạo Organization, OU và áp dụng SCP
  * Xác minh kết hợp SCP + IAM policy trong thực tế