---
title: "Blog 2"
date: "2026-07-14"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Scale to Win tăng cường bảo vệ DDoS bằng AWS WAF — tóm tắt

**Nguồn:** Tóm tắt từ bài viết kiến trúc AWS về Scale to Win

Scale to Win đối mặt với lưu lượng DDoS rất lớn trong thời kỳ chiến dịch. Họ đặt trọng tâm vào việc chặn các request ác ý ngay ở lớp edge và ngăn kẻ tấn công vượt qua CDN.

Các biện pháp chính:
- Đặt Amazon CloudFront phía trước Application Load Balancer để edge hấp thụ lưu lượng lớn.
- Triển khai AWS WAF ở lớp CloudFront với rule dựa trên tần suất (rate), CAPTCHA/challenge và kiểm soát bot.
- Ngăn truy cập trực tiếp vào origin bằng cách giới hạn security group của ALB chỉ chấp nhận địa chỉ IP CloudFront và yêu cầu header bí mật do CloudFront thêm vào.
- Kết hợp nhận diện heuristic (mẫu request, header, fingerprint TLS) với giới hạn tần suất phân vùng để tránh chặn nhầm các client hợp lệ dùng IP chia sẻ.

Kết quả: kiến trúc nhiều lớp giảm tải lên tài nguyên khu vực, bảo toàn lưu lượng hợp lệ và cải thiện khả năng ứng phó sự cố.

* Session policy là một IAM policy inline được chỉ định khi tạo hoặc cập nhật Pod Identity association.
* Quyền hiệu quả = intersection (giao) giữa permissions của IAM role và session policy → session policy chỉ có thể thu hẹp, không thể mở rộng quyền.
* Giúp tránh tình trạng over-permissioning khi reuse chung một IAM role cho nhiều workloads có nhu cầu khác nhau.
* Hỗ trợ cả same-account và cross-account (qua IAM role chaining).
* Giảm đáng kể số lượng IAM roles cần quản lý, tránh chạm giới hạn quota IAM trong cluster lớn.
* Cấu hình dễ dàng qua AWS Management Console, AWS CLI hoặc AWS SDK khi tạo association giữa Kubernetes ServiceAccount và IAM role.

Tính năng này đặc biệt hữu ích khi bạn có nhiều ứng dụng chạy trên cùng một IAM role nhưng cần giới hạn quyền khác nhau (ví dụ: một pod chỉ đọc S3 bucket cụ thể, pod khác chỉ gọi một số API nhất định).

...Hình ảnh...

...Link...

...Hướng dẫn...