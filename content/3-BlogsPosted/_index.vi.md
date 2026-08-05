---
title: "Các bài blog đã dịch"
date: "2026-07-17"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

### [Blog 1 - Cách Generali Malaysia tối ưu hóa hoạt động với Amazon EKS](3.1-Blog1/)

Bài viết giải thích cách Generali Malaysia hiện đại hóa hoạt động bảo hiểm bằng cách áp dụng kiến trúc microservices trên Amazon EKS, đặc biệt là EKS Auto Mode, nhằm tự động hóa việc quản lý node, cân bằng tải, lưu trữ, vá lỗi, nâng cấp và mở rộng tài nguyên. Giải pháp được xây dựng theo AWS Well-Architected Framework và tích hợp các dịch vụ như Amazon GuardDuty, Amazon Inspector, AWS Network Firewall và AWS Secrets Manager để tăng cường bảo mật. Generali cũng sử dụng gắn thẻ tài nguyên, Savings Plans và bộ xử lý AWS Graviton để tối ưu chi phí, trong khi Amazon CloudWatch và Amazon Managed Grafana hỗ trợ giám sát hiệu suất theo từng dự án. Nhờ đó, công ty đã giảm khối lượng công việc vận hành thủ công, nâng cao độ tin cậy của hệ thống và khả năng phản ứng với sự cố, rút ngắn chu kỳ triển khai ứng dụng, tối ưu chi phí hạ tầng và giúp đội ngũ DevOps tập trung nhiều hơn vào phát triển sản phẩm thay vì bảo trì cơ sở hạ tầng.

### [Blog 2 - Cách Scale to Win sử dụng AWS WAF để ngăn chặn các cuộc tấn công DDoS](3.2-Blog2/)

Bài viết giải thích cách Scale to Win bảo vệ nền tảng truyền thông chiến dịch của mình trước các cuộc tấn công DDoS có lưu lượng đạt đỉnh hơn hai triệu yêu cầu mỗi giây bằng cách kết hợp Amazon CloudFront, AWS WAF và AWS Shield Advanced. Công ty định tuyến toàn bộ lưu lượng qua CloudFront, hạn chế quyền truy cập trực tiếp vào Application Load Balancer và sử dụng một secret header dùng chung để ngăn kẻ tấn công vượt qua lớp bảo vệ. Cấu hình AWS WAF của họ phân tách lưu lượng trình duyệt với lưu lượng giao tiếp giữa các hệ thống, áp dụng các quy tắc dựa trên đường dẫn, danh sách địa chỉ IP đáng tin cậy và các giới hạn tốc độ khác nhau. Hệ thống cũng sử dụng CAPTCHA đối với hoạt động đáng ngờ từ trình duyệt, đồng thời vẫn duy trì lưu lượng API và webhook hợp lệ với khối lượng lớn. Scale to Win còn sử dụng logging, Amazon Athena, dấu hiệu nhận dạng lưu lượng và AWS WAF Bot Control để phát hiện các mẫu tấn công độc hại, đồng thời ngăn kẻ tấn công tái sử dụng token CAPTCHA đã được xác thực trên nhiều địa chỉ IP.

### [Blog 3 - Thiết kế kiến trúc cho quá trình phát triển Agentic AI trên AWS](3.3-Blog3/)

Bài viết giải thích cách các kiến trúc AWS có thể được thiết kế lại để hỗ trợ quá trình phát triển Agentic AI, trong đó các AI agent có thể tự động viết, kiểm thử, triển khai và cải tiến mã nguồn thông qua các vòng phản hồi nhanh. Bài viết khuyến nghị sử dụng các công cụ mô phỏng cục bộ như AWS SAM, DynamoDB Local, container và AWS Glue Docker image để kiểm tra thay đổi nhanh chóng, trong khi các tài nguyên đám mây nhẹ và môi trường preview tạm thời được sử dụng cho những bài kiểm thử cần đến các dịch vụ AWS thực tế. Bài viết cũng nhấn mạnh việc xây dựng codebase thân thiện với AI, với sự phân tách rõ ràng giữa các lớp domain, application và infrastructure, các quy tắc dự án cụ thể, hệ thống kiểm thử tự động nhiều lớp, monorepo và tài liệu có thể được máy đọc. Cuối cùng, bài viết đề xuất tích hợp AI agent vào quy trình CI/CD với các biện pháp kiểm soát như kiểm thử bắt buộc, đánh giá tự động, bảo vệ branch và phê duyệt của con người đối với các quyết định có ảnh hưởng lớn, từ đó giúp tăng tốc phát triển mà vẫn đảm bảo độ tin cậy và khả năng quản trị.