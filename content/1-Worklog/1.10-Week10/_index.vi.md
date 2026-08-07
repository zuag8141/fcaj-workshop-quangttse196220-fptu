---
title: "Worklog Tuần 10"
date: "2026-07-31"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Sửa lỗi luồng "mark as paid" (đánh dấu đã thanh toán) trong trang Settlement.
* Sửa lỗi hiển thị avatar trong Sidebar và Dashboard.
* Cập nhật các type và service liên quan đến group dùng trong trang Settlement.
* Test các luồng đã sửa và tích hợp thay đổi vào dự án qua pull request.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Xem lại trang Settlement và tái hiện lỗi "mark as paid" <br> - Phân tích luồng dữ liệu settlement và group trên frontend <br> - Xác định nguyên nhân gốc của bug | 27/07/2026 | 27/07/2026 | |
| 3 | - Sửa luồng "mark as paid" trong trang Settlement với commit `fix-mark-as-paid` <br>&emsp; + Sửa logic cập nhật trạng thái settlement <br>&emsp; + Cập nhật group types và service liên quan <br> - Tạo và merge Pull Request `#12` | 28/07/2026 | 28/07/2026 | |
| 4 | - Sửa lỗi hiển thị avatar với commit `fix-avatar` <br>&emsp; + Cập nhật component Sidebar <br>&emsp; + Cập nhật trang Dashboard <br> - Reapply lại fix-mark-as-paid và fix-avatar sau khi revert | 29/07/2026 | 29/07/2026 | |
| 5 | - Áp dụng fix avatar cuối cùng với commit `fix-avatar-2` <br> - Test luồng mark-as-paid và avatar sau khi sửa <br> - Kiểm tra trang Settlement và Dashboard hoạt động đúng | 30/07/2026 | 30/07/2026 | |
| 6 | - Rà soát các thay đổi đã merge vào nhánh main <br> - Chạy regression test trên các màn hình bị ảnh hưởng <br> - Ghi nhận kết quả và chuẩn bị báo cáo tuần | 31/07/2026 | 31/07/2026 | |

### Kết quả đạt được tuần 10:

* **Kết quả chung:**
  * Sửa thành công lỗi "mark as paid" trong trang Settlement.
  * Sửa lỗi hiển thị avatar ở Sidebar và Dashboard.
  * Tích hợp thay đổi vào dự án qua Pull Request `#12`.

* **Các lỗi đã sửa:**
  * Trang Settlement cập nhật đúng trạng thái "mark as paid".
  * Avatar hiển thị chính xác ở Sidebar và Dashboard.
  * Cập nhật group types và service logic dùng trong trang Settlement.

* **Kiến thức và kinh nghiệm đạt được:**
  * Cải thiện kỹ năng debug state và luồng dữ liệu frontend.
  * Có thêm kinh nghiệm Git: tạo branch, commit, revert và pull request.