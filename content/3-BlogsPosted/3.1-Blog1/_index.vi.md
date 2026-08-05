---
title: "Blog 1"
date: "2026-07-17"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Cách Generali Malaysia tối ưu hóa hoạt động với Amazon EKS

> **Bài viết gốc:** [Cách Generali Malaysia tối ưu hóa hoạt động với Amazon EKS](https://aws.amazon.com/blogs/architecture/how-generali-malaysia-optimizes-operations-with-amazon-eks/)

**Tác giả:** Antoine Boucherie và Ivan Amemoutou  
**Ngày xuất bản:** 23 tháng 3 năm 2026

## Giới thiệu

Ngành bảo hiểm đang ngày càng ứng dụng điện toán đám mây để mở rộng các dịch vụ số và đáp ứng kỳ vọng của khách hàng về trải nghiệm liền mạch trên nhiều kênh khác nhau.

Generali Malaysia phải giải quyết một số yêu cầu quan trọng:

- Di chuyển các ứng dụng cũ lên đám mây
- Phát triển và mở rộng các dịch vụ bảo hiểm số mới
- Cải thiện tính linh hoạt và khả năng mở rộng của ứng dụng
- Giảm công sức quản lý cơ sở hạ tầng
- Duy trì bảo mật và tuân thủ
- Hỗ trợ tăng trưởng kinh doanh với một đội ngũ vận hành tinh gọn

Để đáp ứng những yêu cầu này, Generali đã áp dụng kiến trúc microservices được container hóa và lựa chọn [Amazon Elastic Kubernetes Service (Amazon EKS)](https://aws.amazon.com/eks/) làm nền tảng container chính cho các ứng dụng đã được hiện đại hóa.

Generali bắt đầu hành trình di chuyển lên AWS vào năm 2019. Amazon EKS được lựa chọn vì dịch vụ này cung cấp khả năng quản lý Kubernetes ở cấp độ doanh nghiệp và tích hợp chặt chẽ với các dịch vụ AWS khác. Đội ngũ DevOps và Cloud của Generali cũng đã có kinh nghiệm vận hành các môi trường Kubernetes.

Hiện nay, Generali đang vận hành các ứng dụng số và một số giải pháp bảo hiểm cốt lõi trên các cụm Amazon EKS. Tổ chức sử dụng EKS Auto Mode cùng nhiều dịch vụ AWS khác để cải thiện hiệu suất, tăng cường bảo mật, tối ưu chi phí cơ sở hạ tầng và giảm khối lượng công việc vận hành.

---

## Tổng quan giải pháp

Generali thiết kế môi trường Amazon EKS theo [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/).

Quá trình triển khai xem xét sáu trụ cột của Well-Architected:

1. Vận hành xuất sắc
2. Bảo mật
3. Độ tin cậy
4. Hiệu quả hiệu suất
5. Tối ưu hóa chi phí
6. Tính bền vững

Việc áp dụng các nguyên tắc này giúp Generali nâng cao khả năng phục hồi của hệ thống thông qua tự động hóa và giám sát, tăng cường bảo mật bằng [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/) cùng các chính sách mạng, tối ưu chi phí bằng cách điều chỉnh tài nguyên phù hợp và tự động mở rộng, đồng thời giảm thiểu việc sử dụng tài nguyên không cần thiết.

![Architecture ](/aws_internship/images/blog1/blog1.png)

Giải pháp mang lại một số lợi ích chính:

- Đơn giản hóa việc quản lý nhiều ứng dụng được container hóa
- Tự động cấp phát và mở rộng node
- Tích hợp các biện pháp kiểm soát bảo mật
- Nâng cao hiệu quả sử dụng tài nguyên
- Đơn giản hóa việc phân bổ chi phí
- Cung cấp khả năng quan sát chi tiết cho nhiều dự án và đơn vị sử dụng
- Giảm công sức bảo trì cơ sở hạ tầng

---

## Vận hành xuất sắc, độ tin cậy và hiệu quả hiệu suất

### Thách thức khi quản lý các ứng dụng được container hóa

Khi Generali mở rộng danh mục các ứng dụng được container hóa, số lượng workload, đơn vị sử dụng và đội ngũ phát triển ứng dụng cũng tăng lên.

Sự tăng trưởng này tạo ra một số thách thức trong vận hành:

- Điều phối và mở rộng thủ công
- Bảo trì cơ sở hạ tầng
- Cấp phát và thay thế node
- Nâng cấp Kubernetes và hệ điều hành
- Cấu hình bảo mật không đồng nhất
- Cấp phát quá nhiều tài nguyên tính toán
- Khó phân bổ chi phí cho từng dự án kinh doanh
- Khối lượng công việc vận hành của đội ngũ DevOps ngày càng tăng

Generali cần mở rộng việc ứng dụng Kubernetes nhưng vẫn duy trì một cơ cấu vận hành tinh gọn.

---

## Amazon EKS Auto Mode

Generali áp dụng [Amazon EKS Auto Mode](https://aws.amazon.com/blogs/containers/under-the-hood-amazon-eks-auto-mode/) để tự động hóa việc quản lý cơ sở hạ tầng Kubernetes.

EKS Auto Mode tự động quản lý:

- Worker node
- Năng lực tính toán
- Load balancer
- Cấu hình lưu trữ
- Cấp phát node
- Mở rộng cluster
- Các add-on mặc định của Amazon EKS
- Vá lỗi hệ điều hành
- Nâng cấp cơ sở hạ tầng cluster

EKS Auto Mode tự động điều chỉnh tài nguyên dựa trên nhu cầu của workload. Dịch vụ lựa chọn năng lực từ các loại phiên bản [Amazon EC2](https://aws.amazon.com/ec2/) đã được phê duyệt và cấu hình trong các EKS node pool.

Theo [mô hình trách nhiệm chung mở rộng dành cho EKS Auto Mode](https://docs.aws.amazon.com/eks/latest/userguide/automode.html), AWS đảm nhận nhiều tác vụ quản lý cơ sở hạ tầng hơn so với các cụm EKS thông thường.

Những tác vụ này bao gồm:

- Vá lỗi hệ điều hành Bottlerocket
- Quản lý các add-on mặc định của EKS
- Thay thế các node đã lỗi thời
- Áp dụng các Amazon Machine Image mới
- Hỗ trợ nâng cấp cluster
- Điều chỉnh năng lực tính toán

Khả năng tự động hóa này giúp đội ngũ DevOps và Cloud của Generali tập trung nhiều hơn vào việc hỗ trợ các nhóm phát triển ứng dụng thay vì thực hiện những công việc bảo trì Kubernetes thường xuyên.

---

## Kiểm soát sự gián đoạn của workload

EKS Auto Mode thường xuyên phát hành các Amazon Machine Image được cập nhật và thay thế các node hiện có bằng những phiên bản mới hơn.

Do việc thay thế node có thể làm gián đoạn các workload đang chạy, Generali đã triển khai các biện pháp kiểm soát gián đoạn.

Những biện pháp này bao gồm:

- Lên lịch bảo trì trong các khung giờ thấp điểm
- Cấu hình [Pod Disruption Budget](https://docs.aws.amazon.com/eks/latest/best-practices/application.html)
- Cấu hình [Node Disruption Budget](https://docs.aws.amazon.com/eks/latest/userguide/create-node-pool.html)
- Duy trì nhiều bản sao của các microservice quan trọng
- Ngăn tất cả pod của cùng một dịch vụ bị dừng đồng thời

Những biện pháp kiểm soát này cho phép nền tảng tiếp nhận các bản cập nhật cơ sở hạ tầng mà vẫn duy trì tính sẵn sàng của ứng dụng.

---

## Các nguyên tắc bảo đảm độ tin cậy của ứng dụng

Generali tuân theo một số nguyên tắc nhằm nâng cao độ tin cậy của các workload Kubernetes:

- Chỉ các microservice không lưu trạng thái mới được vận hành trong các cụm EKS.
- Các pod được xem là những thành phần bất biến.
- Các pod đang chạy được thay thế thay vì chỉnh sửa trực tiếp.
- Helm chart được sử dụng làm cơ chế triển khai tiêu chuẩn.
- Horizontal Pod Autoscaler được sử dụng để mở rộng ứng dụng dựa trên lưu lượng truy cập.
- Trạng thái lâu dài được tách khỏi các container ứng dụng.
- Các ứng dụng quan trọng duy trì nhiều bản sao pod.

Cách tiếp cận này giúp các ứng dụng dễ dàng được mở rộng, thay thế, khôi phục và nâng cấp hơn.

---

# Kiến trúc bảo mật

Generali kết hợp nhiều dịch vụ bảo mật AWS để bảo vệ môi trường EKS:

- [Amazon GuardDuty](https://aws.amazon.com/guardduty/)
- [Amazon Inspector](https://aws.amazon.com/inspector/)
- [AWS Network Firewall](https://aws.amazon.com/network-firewall/)
- [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/)

---

## Amazon GuardDuty Extended Threat Detection

Generali triển khai [Amazon GuardDuty Extended Threat Detection dành cho Amazon EKS](https://aws.amazon.com/blogs/aws/amazon-guardduty-expands-extended-threat-detection-coverage-to-amazon-eks-clusters/) để phát hiện các cuộc tấn công tinh vi gồm nhiều giai đoạn.

GuardDuty phân tích và liên kết các tín hiệu từ:

- Nhật ký kiểm tra của Amazon EKS
- Hành vi runtime của container
- Hoạt động thực thi mã độc
- Hoạt động AWS API
- Các hành vi khai thác container
- Leo thang đặc quyền
- Di chuyển trái phép giữa các tài nguyên

Thay vì xem các phát hiện bảo mật là những sự kiện không liên quan, GuardDuty có thể kết nối chúng thành một chuỗi tấn công tổng thể.

Các hoạt động được phát hiện cũng có thể được ánh xạ với các chiến thuật và kỹ thuật của MITRE ATT&CK.

### Lợi ích

Generali nhận được một số lợi ích từ GuardDuty:

- Giảm thời gian điều tra bảo mật
- Tổng hợp các phát hiện và dòng thời gian tấn công
- Xác định cơ sở hạ tầng bị ảnh hưởng nhanh hơn
- Ưu tiên các rủi ro nghiêm trọng hiệu quả hơn
- Cải thiện khả năng phát hiện các cuộc tấn công nhắm vào container
- Giảm phạm vi ảnh hưởng tiềm ẩn
- Phản ứng sự cố nhanh hơn

---

## Amazon Inspector

Generali sử dụng [Amazon Inspector](https://aws.amazon.com/inspector/) để quản lý các lỗ hổng trong container image.

Tổ chức cũng sử dụng khả năng của Inspector để [ánh xạ Amazon ECR image với các container đang chạy](https://aws.amazon.com/blogs/aws/amazon-inspector-enhances-container-security-by-mapping-amazon-ecr-images-to-running-containers/).

Khả năng này cung cấp những thông tin như:

- Những image nào hiện đang được sử dụng
- Các cụm EKS đang chạy những image đó
- Amazon Resource Name của cluster
- Số lượng pod đang sử dụng từng image
- Thời điểm gần nhất image được sử dụng
- Các phát hiện lỗ hổng liên quan đến image

Thông tin này cho phép đội ngũ bảo mật ưu tiên những lỗ hổng ảnh hưởng đến các workload đang hoạt động, thay vì xử lý mọi image trong kho lưu trữ với cùng một mức độ khẩn cấp.

### Lợi ích

- Ưu tiên dựa trên mức sử dụng container thực tế
- Khả năng quan sát tốt hơn trên các môi trường EKS
- Khắc phục các lỗ hổng liên quan nhanh hơn
- Giảm công sức xử lý các image không được sử dụng
- Quản lý lỗ hổng container chính xác hơn

---

## AWS Network Firewall

Generali sử dụng [AWS Network Firewall](https://aws.amazon.com/network-firewall/) để kiểm soát các kết nối HTTPS đi ra từ những ứng dụng đang chạy trong các cụm EKS.

Kiến trúc này tuân theo phương pháp được trình bày trong bài viết trên AWS Security Blog về [lọc lưu lượng HTTPS đi ra từ Amazon EKS bằng AWS Network Firewall](https://aws.amazon.com/blogs/security/use-aws-network-firewall-to-filter-outbound-https-traffic-from-applications-hosted-on-amazon-eks/).

Các cụm EKS được triển khai trong private subnet. Lưu lượng đi ra được định tuyến qua các endpoint của Network Firewall và NAT gateway trước khi truy cập Internet.

Server Name Indication, hay SNI, được sử dụng để chỉ cho phép kết nối đến những hostname nằm trong danh sách được phê duyệt.

### Lợi ích

- Ứng dụng chỉ có thể truy cập các dịch vụ bên ngoài đã được phê duyệt.
- Chính sách bảo mật được xây dựng dựa trên hostname thay vì các địa chỉ IP có thể thay đổi.
- Các kết nối đi ra không được phép có thể bị chặn.
- Những hostname đã được truy cập có thể được ghi lại và phân tích.
- Các mẫu lưu lượng đi ra có thể được giám sát thông qua [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/).
- Kiến trúc hỗ trợ các yêu cầu về bảo mật và tuân thủ.

---

## AWS Secrets Manager

Thông tin xác thực của ứng dụng, API key, mật khẩu và các giá trị nhạy cảm khác không nên được viết trực tiếp trong các Kubernetes deployment template.

Generali lưu trữ những thông tin nhạy cảm này trong [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/).

Công ty sử dụng [External Secrets Operator](https://aws.amazon.com/blogs/containers/leverage-aws-secrets-stores-from-eks-fargate-with-external-secrets-operator/) trong các cụm EKS.

Operator thực hiện quy trình sau:

1. Truy xuất secret từ AWS Secrets Manager.
2. Tạo hoặc cập nhật các Kubernetes secret tương ứng.
3. Cung cấp những secret này cho các pod.
4. Cung cấp các giá trị thông qua biến môi trường khi cần thiết.
5. Đồng bộ các giá trị mới khi thông tin xác thực được luân chuyển.

### Lợi ích

- Quản lý secret tập trung
- Không hard-code thông tin xác thực
- Cải thiện khả năng kiểm tra và truy vết
- Đồng bộ tự động
- Hỗ trợ luân chuyển thông tin xác thực
- Không cần chỉnh sửa mã nguồn ứng dụng
- Giảm độ phức tạp trong vận hành
- Quản lý secret bên ngoài Kubernetes cluster

---

# Tối ưu hóa chi phí

## Phân bổ chi phí bằng tag

Mặc dù EKS Auto Mode tự động cải thiện hiệu quả sử dụng tài nguyên, Generali vẫn cần xác định chi phí của từng dự án, ứng dụng và đơn vị kinh doanh.

Generali sử dụng [dữ liệu phân bổ chi phí được chia nhỏ của AWS Billing dành cho Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/cost-monitoring.html) cùng với [AWS Billing](https://aws.amazon.com/aws-cost-management/aws-billing/).

Công ty phân tích chi phí Kubernetes cùng với các khoản chi phí AWS khác.

Việc phân bổ chi phí có thể sử dụng các tag liên quan đến Kubernetes như:

```text
aws:eks:cluster-name
aws:eks:deployment
aws:eks:namespace
aws:eks:node
```