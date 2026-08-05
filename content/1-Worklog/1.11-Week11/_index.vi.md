---
title: "Worklog Tuần 11"
date: "2026-08-01"
weight: 1
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Phát triển và hoàn thiện hệ thống notification của dự án.
* Xây dựng API quản lý notification inbox và notification preferences.
* Thêm notification cho các hoạt động liên quan đến group, expense, settlement và product update.
* Triển khai luồng gửi complaint cho người dùng.
* Triển khai luồng xử lý complaint cho quản trị viên.
* Test, đồng bộ và tích hợp source code thông qua pull request.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Phân tích yêu cầu của hệ thống notification <br> - Xác định các loại notification mà dự án cần <br> - Rà soát flow thông báo liên quan đến group, expense và settlement <br> - Rà soát cấu trúc module notification và chuẩn bị API cần thiết | 27/07/2026 | 27/07/2026 | |
| 3 | - Implement notification preference và inbox management với commit `feat: add notification preferences and inbox APIs` <br>&emsp; + Thêm API quản lý notification preferences <br>&emsp; + Thêm API lấy danh sách notification của user <br>&emsp; + Hỗ trợ quản lý trạng thái read/unread <br> - Implement debtor payment flow với commit `feat: add debtor settlement and payment sent flow` <br>&emsp; + Cho phép debtor đánh dấu đã gửi payment <br>&emsp; + Sinh notification cho payment submission và settlement confirmation <br> - Tích hợp thay đổi thông qua Pull Request `#14` | 28/07/2026 | 28/07/2026 | |
| 4 | - Thêm notification cho expense và group membership với commit `feat: add expense and group membership notifications` <br>&emsp; + Sinh notification cho các hoạt động liên quan đến expense <br>&emsp; + Sinh notification khi thêm member hoặc thay đổi membership <br> - Thêm weekly settlement reminder notification với commit `feat: add weekly on-demand settlement reminder notifications` <br>&emsp; + Hỗ trợ tạo reminder on-demand <br>&emsp; + Thêm cơ chế nhắc settlement lặp theo tuần | 29/07/2026 | 29/07/2026 | |
| 5 | - Hoàn thiện hệ thống notification với commit `feat: add product update notifications and finalize notification APIs` <br>&emsp; + Thêm notification cập nhật sản phẩm <br>&emsp; + Hoàn tất các API còn lại của module notification <br>&emsp; + Rà soát response, validation và permission <br> - Tích hợp thay đổi qua Pull Request `#15` <br> - Xác minh lại notification inbox và notification preferences sau khi merge | 30/07/2026 | 30/07/2026 | |
| 6 | - Implement chức năng gửi complaint của người dùng với commit `feat(complaint): implement user complaint submission feature` <br>&emsp; + Cho phép user tạo và gửi complaint <br>&emsp; + Lưu thông tin complaint và gắn với user gửi <br>&emsp; + Kiểm tra dữ liệu đầu vào và quyền gửi complaint <br> - Implement luồng xử lý complaint cho quản trị viên với commit `feat(admin): implement complaint handling flow` <br>&emsp; + Cho phép admin xem các complaint cần xử lý <br>&emsp; + Hỗ trợ cập nhật kết quả và trạng thái xử lý complaint <br>&emsp; + Thêm kiểm tra quyền cho các thao tác của admin <br> - Kiểm thử toàn bộ flow complaint từ user submit đến admin xử lý | 31/07/2026 | 31/07/2026 | |

### Kết quả đạt được tuần 11:

* **Kết quả chung:**
  * Tuần này tôi tập trung phát triển backend notification và các tính năng quản lý complaint.
  * Hệ thống notification được mở rộng để hỗ trợ group membership changes, expense creation, payment submission, settlement reminders và product updates.
  * Tôi cũng triển khai hoàn chỉnh flow complaint cho cả user gửi complaint và admin xử lý complaint.
  * Các thay đổi của module notification được tích hợp vào dự án thông qua Pull Request `#14` và `#15`.

* **Tính năng notification đã hoàn thành:**
  * Thêm API quản lý notification preferences.
  * Thêm API quản lý notification inbox.
  * Hỗ trợ lấy notification và quản lý trạng thái read/unread.
  * Thêm flow cho debtor đánh dấu payment đã được gửi.
  * Sinh notification cho settlement và payment-sent.
  * Sinh notification cho expense mới và các hoạt động liên quan đến expense.
  * Sinh notification cho hoạt động group membership.
  * Thêm notification nhắc settlement theo yêu cầu.
  * Thêm notification nhắc settlement định kỳ hàng tuần.
  * Thêm notification cập nhật sản phẩm.
  * Hoàn tất các API chính của module notification.

* **Tính năng complaint đã hoàn thành:**
  * Cho phép user gửi complaint qua hệ thống.
  * Kiểm tra và lưu thông tin complaint.
  * Gắn complaint với user đã gửi.
  * Triển khai flow để admin xem và xử lý complaint.
  * Cho phép admin cập nhật kết quả và trạng thái xử lý complaint.
  * Thêm kiểm tra quyền admin cho các thao tác xử lý complaint.

* **Commit đã đóng góp:**
  * `feat: add notification preferences and inbox APIs`
  * `feat: add debtor settlement and payment sent flow`
  * `feat: add expense and group membership notifications`
  * `feat: add weekly on-demand settlement reminder notifications`
  * `feat: add product update notifications and finalize notification APIs`
  * `feat(complaint): implement user complaint submission feature`
  * `feat(admin): implement complaint handling flow`

* **Pull request đã merge:**
  * Pull Request `#14`: tích hợp notification preferences, inbox APIs và flow debtor settlement/payment-sent.
  * Pull Request `#15`: tích hợp notification cho expense, group membership, settlement reminders và product updates, đồng thời hoàn thiện notification APIs.

* **Kiến thức và kinh nghiệm đạt được:**
  * Hiểu cách thiết kế module notification cho nhiều loại sự kiện khác nhau.
  * Có kinh nghiệm phát triển API cho notification inbox và user notification preferences.
  * Học cách tích hợp notification với business functions như group, expense và settlement.
  * Có kinh nghiệm triển khai các tính năng lặp lại như weekly settlement reminders.
  * Hiểu cách thiết kế quy trình complaint giữa user và admin.
  * Cải thiện kỹ năng kiểm tra quyền và chuyển trạng thái trong backend.
  * Thực hành workflow phát triển với commit, pull request, testing và tích hợp source code.