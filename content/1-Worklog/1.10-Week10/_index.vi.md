---
title: "Worklog Tuần 10"
date: "2026-07-25"
weight: 1
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Tham gia phát triển và cải thiện các chức năng backend của dự án.
* Điều chỉnh logic xác nhận thanh toán để xử lý đúng quyền của người dùng.
* Tách phần cấu hình thông báo thành module độc lập để tăng khả năng bảo trì và mở rộng.
* Thêm giới hạn số thành viên cho nhóm dùng gói free.
* Sửa lỗi, đồng bộ mã nguồn với nhánh `main`, và tạo pull request để tích hợp thay đổi vào dự án.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Xem lại source code backend và phân tích yêu cầu phát triển trong tuần <br> - Phân tích luồng xác nhận thanh toán khoản nợ <br> - Xác định quyền của debtor và creditor trong flow này <br> - Chuẩn bị nhánh phát triển riêng để triển khai và kiểm thử | 20/07/2026 | 20/07/2026 | |
| 3 | - Cập nhật logic xác nhận thanh toán với commit `fix(expenses): restrict settlement confirmation to creditor` <br>&emsp; + Giới hạn quyền xác nhận thanh toán chỉ cho creditor <br>&emsp; + Ngăn người dùng không hợp lệ xác nhận thanh toán <br> - Tạo và merge Pull Request `#8` <br> - Refactor hàm thông báo với commit `refactor(notifications): extract notification preferences into separate module` <br>&emsp; + Tách notification preferences thành module riêng <br>&emsp; + Giảm phụ thuộc giữa các component trong module thông báo <br>&emsp; + Cải thiện khả năng bảo trì và mở rộng của source code <br> - Tạo và merge Pull Request `#9` | 21/07/2026 | 21/07/2026 | |
| 4 | - Thêm giới hạn số thành viên cho nhóm free plan với commit `feat(groups): limit free plan groups to 5 members` <br>&emsp; + Kiểm tra subscription plan của group trước khi thêm member <br>&emsp; + Giới hạn nhóm free plan tối đa 5 thành viên <br>&emsp; + Trả về lỗi phù hợp khi vượt giới hạn <br> - Tạo và merge Pull Request `#10` <br> - Sửa các vấn đề phát sinh trong quá trình phát triển với commit `fixed bug` <br> - Merge nhánh `main` vào nhánh `be-Minh` để cập nhật source code mới nhất và xử lý khác biệt giữa các nhánh <br> - Tạo và merge Pull Request `#11` | 22/07/2026 | 22/07/2026 | |
| 5 | - Kiểm tra lại các thay đổi sau khi merge vào nhánh main <br> - Test các case liên quan đến quyền xác nhận thanh toán <br> - Test giới hạn thành viên của nhóm free plan <br> - Kiểm tra notification preferences vẫn hoạt động đúng sau khi tách module <br> - Kiểm tra lỗi phát sinh trong quá trình merge source code | 23/07/2026 | 23/07/2026 | |
| 6 | - Tổng hợp commit và pull request đã đóng góp trong tuần <br> - Kiểm tra trạng thái merge của Pull Request `#8`, `#9`, `#10`, `#11` <br> - Rà soát source code và đảm bảo các chức năng đã hoàn tất được tích hợp thành công vào dự án <br> - Ghi nhận kết quả và chuẩn bị báo cáo tuần | 24/07/2026 | 24/07/2026 | |

### Kết quả đạt được tuần 10:

* **Kết quả chung:**
  * Tuần này tôi tham gia phát triển backend và đóng góp các thay đổi liên quan đến xác nhận thanh toán, notification preferences và giới hạn thành viên cho nhóm free plan.
  * Tôi hoàn thành các chức năng được giao, sửa lỗi, đồng bộ source code và tạo pull request để tích hợp thay đổi vào nhánh chính của dự án.
  * Tổng cộng có 4 pull request được merge, bao gồm `#8`, `#9`, `#10` và `#11`.

* **Chức năng đã hoàn thành:**
  * Cập nhật quyền xác nhận thanh toán để chỉ creditor được xác nhận settlement.
  * Ngăn người dùng không phải creditor thực hiện hành động xác nhận thanh toán.
  * Tách notification preferences thành module riêng.
  * Cải thiện cấu trúc source code của phần thông báo.
  * Thêm rule giới hạn nhóm free plan tối đa 5 thành viên.
  * Thêm xử lý lỗi khi thêm thành viên vượt quá giới hạn của free plan.
  * Sửa các vấn đề phát sinh trong quá trình phát triển và tích hợp.
  * Đồng bộ nhánh `be-Minh` với nhánh `main`.

* **Commit đã đóng góp:**
  * `fix(expenses): restrict settlement confirmation to creditor`
  * `refactor(notifications): extract notification preferences into separate module`
  * `feat(groups): limit free plan groups to 5 members`
  * `fixed bug`
  * `Merge branch 'main' into be-Minh`

* **Pull request đã merge:**
  * Pull Request `#8`: Giới hạn quyền xác nhận settlement chỉ cho creditor.
  * Pull Request `#9`: Tích hợp refactor notification preferences.
  * Pull Request `#10`: Tích hợp tính năng giới hạn nhóm free plan tối đa 5 thành viên.
  * Pull Request `#11`: Tích hợp bug fix và đồng bộ source code.

* **Kiến thức và kinh nghiệm đạt được:**
  * Hiểu rõ hơn cách triển khai business rule và kiểm tra quyền người dùng trong backend.
  * Học cách tổ chức và tách module để cải thiện tính bảo trì của source code.
  * Hiểu cách giới hạn chức năng hệ thống dựa trên subscription plan của người dùng.
  * Thực hành Git workflow: tạo commit, cập nhật branch, merge branch và quản lý pull request.
  * Có thêm kinh nghiệm kiểm tra và test chức năng sau khi source code được merge vào main.