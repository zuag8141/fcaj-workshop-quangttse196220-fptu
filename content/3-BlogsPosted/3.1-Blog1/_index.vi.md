---
title: "Blog 1"
date: "2026-07-17"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# Tóm tắt: Generali Malaysia tối ưu vận hành bằng Amazon EKS

**Nguồn:** Tóm tắt từ bài case study của AWS (Generali Malaysia)

Generali đã container hóa ứng dụng và chọn Amazon EKS để giảm chi phí vận hành, cải thiện bảo mật và tăng khả năng mở rộng. Họ tập trung vào tự động hóa quản lý hạ tầng để nhóm vận hành có thể làm việc nhiều giá trị hơn.

Ý chính:
- Áp dụng các nguyên tắc Well‑Architected (vận hành, bảo mật, độ tin cậy, hiệu năng, tối ưu chi phí, bền vững).
- Dùng các tính năng quản lý (ví dụ EKS Auto Mode) để giảm công việc bảo trì node, vá OS và nâng cấp.
- Thiết lập controls chống gián đoạn (Pod Disruption Budgets, lịch bảo trì, nhiều bản sao) để duy trì sẵn sàng khi nâng cấp.
- Kết hợp các dịch vụ bảo mật (GuardDuty, Inspector, Network Firewall, Secrets Manager) để bảo vệ workload.
- Gắn tag và phân bổ chi phí để theo dõi chi phí Kubernetes theo đội/ dự án.

Lợi ích:
- Giảm công sức quản lý Kubernetes.
- Cải thiện an ninh và ưu tiên xử lý lỗ hổng.
- Tối ưu sử dụng tài nguyên và minh bạch chi phí.