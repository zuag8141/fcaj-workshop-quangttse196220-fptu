---
title: "Blog 3"
date: "2026-03-26"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Kiến trúc cho phát triển AI agent trên AWS — tóm tắt

**Nguồn:** Tóm tắt từ bài viết kiến trúc AWS về agentic AI

Khi dùng AI agent để tạo hoặc sửa code, đội phát triển thường gặp trở ngại do vòng phản hồi chậm và môi trường phát triển chặt chẽ. Phát triển agentic cần kiến trúc giúp xác nhận nhanh, thử nghiệm an toàn và cấu trúc rõ ràng.

Gợi ý kiến trúc:
- Ưu tiên mô phỏng cục bộ và môi trường kiểm thử ngắn hạn (SAM local, container) để agent có thể kiểm tra nhanh mà không cần dựng toàn bộ tài nguyên cloud.
- Dùng môi trường ngắn hạn hoặc namespace để chạy thử an toàn.
- Sắp xếp repository và CI/CD rõ ràng để giảm sự mơ hồ cho công cụ tự động.

Lợi ích:
- Vòng lặp phản hồi ngắn hơn cho các lần lặp tự động.
- Giảm chi phí và rủi ro khi kiểm thử thay đổi.
- Tăng khả năng tái tạo và phân tách trách nhiệm giữa con người và agent.

Những mô hình này giúp tích hợp công cụ agentic vào quá trình phát triển an toàn và có kiểm soát.

...Hướng dẫn...