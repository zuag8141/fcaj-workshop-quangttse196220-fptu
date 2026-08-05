---
title: "Bản đề xuất"
date: 2026-07-31
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Splitly – Group Expense Sharing Platform  
## Giải pháp AWS tập trung cho quản lý, chia sẻ và quyết toán chi phí nhóm

## 2.1 Executive Summary

Splitly là nền tảng quản lý và chia sẻ chi phí nhóm được phát triển nhằm hỗ trợ người dùng theo dõi các khoản chi tiêu chung, tính toán công nợ và quản lý quá trình thanh toán giữa các thành viên một cách minh bạch và thuận tiện.

Hệ thống được xây dựng theo kiến trúc web hiện đại với React + Vite cho giao diện người dùng, Node.js/Express cho dịch vụ backend và MongoDB Atlas làm cơ sở dữ liệu. Hạ tầng được triển khai trên AWS, sử dụng Amazon EC2 để chạy ứng dụng backend và Frontend, Amazon S3 để lưu trữ hóa đơn và hình ảnh chứng từ, Amazon CloudWatch để giám sát hệ thống, cùng Amazon VPC và Security Group nhằm đảm bảo kết nối mạng an toàn.

Nền tảng cung cấp các chức năng chính như quản lý nhóm, ghi nhận và chia sẻ chi phí, tính toán công nợ, theo dõi trạng thái thanh toán giữa các thành viên và lưu trữ hóa đơn điện tử. Kiến trúc được thiết kế theo hướng dễ mở rộng và phù hợp với các nhóm người dùng nhỏ đến vừa.

---

## 2.2 Problem Statement

### Vấn đề hiện tại

Việc quản lý chi tiêu nhóm hiện nay vẫn chủ yếu được thực hiện thủ công thông qua bảng tính hoặc tin nhắn trong các ứng dụng trò chuyện. Khi số lượng thành viên và khoản chi tăng lên, việc theo dõi ai đã thanh toán, ai còn nợ và số tiền cần quyết toán trở nên phức tạp và dễ xảy ra sai sót.

Một số khó khăn thường gặp bao gồm:

* Khó quản lý nhiều khoản chi trong cùng một nhóm.
* Việc tính toán công nợ giữa các thành viên mất nhiều thời gian và dễ nhầm lẫn.
* Thiếu hệ thống lưu trữ hóa đơn tập trung để đối chiếu khi cần.
* Không có cơ chế theo dõi trạng thái thanh toán rõ ràng giữa người nợ và người nhận tiền.
* Khó theo dõi hoạt động của hệ thống và xử lý sự cố khi ứng dụng được triển khai.

### Giải pháp

Giải pháp xây dựng Splitly trên nền tảng AWS nhằm số hóa việc quản lý chi tiêu nhóm. Hệ thống cung cấp các chức năng quản lý nhóm, ghi nhận chi phí, tính toán công nợ, theo dõi thanh toán và lưu trữ hóa đơn trên một nền tảng tập trung, đồng thời tận dụng các dịch vụ AWS để đảm bảo khả năng triển khai, lưu trữ và giám sát hệ thống hiệu quả.

### Lợi ích và hoàn vốn đầu tư (ROI)

Việc triển khai Splitly mang lại nhiều lợi ích, bao gồm:

* Giảm thời gian tính toán và đối chiếu công nợ giữa các thành viên.
* Hạn chế sai sót trong quá trình chia chi phí và thanh toán.
* Tăng tính minh bạch nhờ lịch sử giao dịch và trạng thái thanh toán được lưu trữ tập trung.
* Hỗ trợ lưu trữ hóa đơn điện tử, giúp dễ dàng tra cứu và kiểm chứng khi cần.
* Giám sát hoạt động của hệ thống thông qua Amazon CloudWatch, góp phần nâng cao khả năng vận hành và xử lý sự cố.

Giải pháp tận dụng MongoDB Atlas cùng các dịch vụ AWS như Amazon EC2, Amazon S3 và Amazon CloudWatch, giúp tối ưu chi phí triển khai và vận hành trong khi vẫn đáp ứng nhu cầu của các nhóm người dùng nhỏ đến vừa. So với phương pháp quản lý thủ công, Splitly giúp tiết kiệm thời gian, giảm sai sót và nâng cao hiệu quả quản lý chi tiêu nhóm. Đồng thời, kiến trúc của hệ thống được thiết kế theo hướng dễ mở rộng, tạo nền tảng cho việc bổ sung các tính năng và mở rộng quy mô trong tương lai.

---

## 2.3 Solution Architecture

Dự án sử dụng kiến trúc nguyên khối (Monolithic) cho phần ứng dụng, được triển khai tập trung trên hạ tầng đám mây AWS với 3 tầng chính:

### Presentation Layer – Frontend

* Frontend React/Vite được build thành các file tĩnh (HTML, CSS và JavaScript).
* Các file tĩnh này được lưu trữ trực tiếp trên máy chủ EC2 và được phục vụ bởi Web Server Nginx.
* Khi người dùng truy cập website thông qua Elastic IP (cổng 80), Nginx sẽ tải và trả về giao diện cho trình duyệt.

### Application Layer – Backend

* Cả Backend (Node.js) và Web Server Nginx (phục vụ Frontend) đều được triển khai chung trên một máy chủ Amazon EC2 instance.
* EC2 nằm trong Public Subnet thuộc VPC tại Region ap-southeast-1.
* Web Security Group kiểm soát các cổng được phép truy cập vào EC2 (mở cổng 80 cho web traffic và 22 cho quản trị SSH).
* Internet Gateway tạo đường kết nối 2 chiều giữa EC2 và Internet.
* Nginx cũng đóng vai trò là một Reverse Proxy để định tuyến các request gọi REST API (từ đường dẫn `/api`) vào Backend đang chạy trên cổng 5000 (quản lý bởi PM2).
* Tầng ứng dụng này cũng tích hợp trực tiếp với các dịch vụ bên thứ 3 qua Internet như cổng thanh toán VNPay và Gmail SMTP.

### Data Layer

* Dữ liệu nghiệp vụ như thông tin người dùng, nhóm, chi phí, giao dịch thanh toán, khiếu nại và thông báo được lưu trữ an toàn trên hệ quản trị cơ sở dữ liệu bên ngoài là MongoDB Atlas.
* Ảnh hoặc file biên lai thanh toán được tải lên và lưu trữ riêng trong Amazon S3 Receipts Bucket.
* MongoDB chỉ lưu trữ các metadata (như tên file, object key, URL hoặc trạng thái xử lý), thay vì lưu trực tiếp file vật lý nhằm tối ưu chi phí và hiệu suất.

### Security, Monitoring and Cost Management

* **IAM Role:** EC2 được gắn IAM Role để cấp quyền đẩy file lên S3 Receipts Bucket và gửi dữ liệu cho CloudWatch, đảm bảo an toàn tuyệt đối do không cần phải lưu trữ Access Key/Secret Key cứng trên máy chủ.
* **Quản lý cấu hình:** Các thông tin nhạy cảm (như MongoDB URI, JWT secret, cấu hình GMAIL, khóa VNPay) được thiết lập thông qua các biến môi trường (file `.env`) lưu trữ bảo mật ngay trên máy chủ EC2.
* **Giám sát (Monitoring):** Amazon CloudWatch được sử dụng để thu thập các metric cơ bản và log hệ thống, theo dõi trạng thái hoạt động của EC2 và ứng dụng. Khi có sự cố lỗi hoặc thông số vượt ngưỡng, CloudWatch sẽ kích hoạt cảnh báo qua Amazon SNS.
* **Quản lý chi phí:** AWS Budgets liên tục theo dõi chi phí tài nguyên và sẽ tự động gửi cảnh báo (qua email hoặc SNS) nếu chi tiêu vượt mức ngân sách dự kiến của dự án.

### 2.3.1 Kiến trúc hiện tại
![Architecture](/aws_internship/images/2-Proposal/Architecture_Final.png)

### Dịch vụ AWS sử dụng

* **Amazon S3:** Lưu trữ các file biên lai do người dùng tải lên.
* **Amazon EC2:** Chạy backend API và xử lý nghiệp vụ của hệ thống.
* **AWS IAM:** Cấp quyền cho EC2 truy cập các tài nguyên AWS cần thiết.
* **Amazon CloudWatch:** Thu thập log, theo dõi EC2 và phát hiện sự cố.
* **Amazon SNS:** Gửi email hoặc thông báo cảnh báo từ CloudWatch và AWS Budgets.
* **AWS Budgets:** Theo dõi chi phí và cảnh báo khi vượt ngân sách.

### Thiết kế thành phần

* **Giao diện web & Proxy:** Ứng dụng Frontend (React/Vite) được build thành các file tĩnh và được lưu trữ, phục vụ trực tiếp bởi máy chủ Nginx chạy trên Amazon EC2. Nginx đồng thời làm nhiệm vụ Reverse Proxy để định tuyến các luồng request API từ người dùng vào Backend.

* **Xử lý nghiệp vụ (Backend):** Amazon EC2 (cùng máy chủ với giao diện web) chạy backend API (qua Node.js/PM2), đảm nhiệm xử lý xác thực, quản lý nhóm, chi phí, thanh toán, biên lai, khiếu nại và thông báo.

* **Lưu trữ dữ liệu:** MongoDB Atlas lưu dữ liệu cốt lõi bao gồm thông tin người dùng, nhóm, chi phí, settlement, dispute và notification.

* **Lưu trữ biên lai:** Amazon S3 (Receipts Bucket) lưu giữ hình ảnh và file biên lai do người dùng tải lên, giúp giảm tải dung lượng lưu trữ cục bộ cho EC2.

* **Kết nối mạng:** Amazon VPC, Public Subnet, Internet Gateway và Elastic IP tạo hạ tầng mạng cơ sở, hỗ trợ EC2 kết nối với người dùng, đẩy file lên S3, kết nối với MongoDB Atlas và gọi các dịch vụ bên ngoài (như cổng thanh toán VNPay, Gmail SMTP).

* **Bảo vệ hệ thống:** Web Security Group đóng vai trò tường lửa, giới hạn các cổng và nguồn mạng được phép truy cập vào EC2 (ví dụ: chỉ cho phép cổng 80 cho web traffic và 22 cho quản trị SSH).

* **Quản lý cấu hình & Secret:** Các thông tin nhạy cảm của hệ thống (MongoDB URI, JWT Secret, thông tin Gmail, khóa VNPay) được lưu trữ qua các biến môi trường (tệp `.env`) trực tiếp trên máy chủ EC2.

* **Quản lý quyền truy cập:** AWS IAM Role cấp quyền (theo nguyên tắc đặc quyền tối thiểu) cho máy chủ EC2 để truy cập vào S3 Receipts Bucket và CloudWatch một cách an toàn mà không cần lưu trữ Access Key trên máy chủ.

* **Giám sát hệ thống:** Amazon CloudWatch hoạt động ngầm để thu thập log ứng dụng, theo dõi trạng thái tài nguyên (CPU, RAM, Disk) của EC2 và tạo cảnh báo khi phát hiện sự cố.

* **Gửi cảnh báo:** Amazon SNS làm nhiệm vụ trung chuyển, gửi email hoặc tin nhắn thông báo từ CloudWatch và AWS Budgets đến quản trị viên.

* **Quản lý chi phí:** AWS Budgets liên tục theo dõi chi phí sử dụng hạ tầng AWS và kích hoạt cảnh báo khi mức tiêu dùng đạt hoặc vượt ngưỡng ngân sách dự kiến.

### 2.3.2 Kiến trúc đề xuất trong tương lai

Hình dưới đây mô tả kiến trúc nâng cấp được đề xuất cho hệ thống Splitly trong tương lai. Kiến trúc này chưa nằm trong phạm vi triển khai hiện tại nhưng được định hướng là giai đoạn phát triển tiếp theo của hệ thống.

![Architecture_Update](/aws_internship/images/2-Proposal/Architecture_Update.png)

Trong kiến trúc đề xuất, ứng dụng frontend sẽ được tách khỏi máy chủ backend. Frontend React/Vite được build thành các file tĩnh và lưu trữ trong Amazon S3 Frontend Bucket. Amazon CloudFront được sử dụng để phân phối nội dung frontend đến người dùng, giúp giảm độ trễ và cải thiện tốc độ tải trang.

Amazon Route 53 được sử dụng để quản lý tên miền và định tuyến người dùng đến hệ thống. AWS WAF được đặt trước CloudFront nhằm hỗ trợ bảo vệ ứng dụng khỏi một số hình thức tấn công web phổ biến. AWS Certificate Manager được sử dụng để quản lý chứng chỉ SSL/TLS, cho phép hệ thống cung cấp kết nối HTTPS an toàn.

Ứng dụng backend tiếp tục được triển khai trên một Amazon EC2 instance nằm trong Public Subnet thuộc Amazon VPC. Backend chịu trách nhiệm xử lý nghiệp vụ, cung cấp REST API, kết nối với MongoDB Atlas và tải các file biên lai lên Amazon S3 Receipts Bucket.

Amazon S3 Receipts Bucket được sử dụng riêng để lưu trữ hình ảnh và file biên lai do người dùng tải lên. Việc tách riêng Frontend Bucket và Receipts Bucket giúp hệ thống quản lý dữ liệu rõ ràng hơn và áp dụng các chính sách truy cập phù hợp cho từng loại tài nguyên.

Amazon CloudWatch được sử dụng để thu thập metric hạ tầng và log ứng dụng. Amazon SNS chịu trách nhiệm gửi cảnh báo vận hành đến quản trị viên, trong khi AWS Budgets theo dõi chi phí sử dụng tài nguyên và gửi thông báo khi mức chi tiêu tiệm cận hoặc vượt ngưỡng ngân sách đã thiết lập.

AWS IAM được sử dụng để quản lý quyền truy cập giữa EC2 và các dịch vụ AWS. EC2 được gắn IAM Role nhằm cho phép backend truy cập S3 và CloudWatch mà không cần lưu trữ Access Key và Secret Access Key trực tiếp trên máy chủ.

### 2.3.3 Những cải tiến dự kiến

So với kiến trúc hiện tại, kiến trúc đề xuất trong tương lai mang lại các cải tiến sau:

+ **Tách biệt frontend và backend:** Các file frontend tĩnh được chuyển từ EC2 sang Amazon S3, giúp EC2 tập trung xử lý API và nghiệp vụ backend.

+ **Cải thiện hiệu suất:** Amazon CloudFront lưu vào bộ nhớ đệm và phân phối nội dung frontend thông qua hệ thống edge location, giúp giảm thời gian tải trang cho người dùng.

+ **Hỗ trợ tên miền riêng:** Amazon Route 53 cho phép người dùng truy cập Splitly thông qua tên miền thay vì sử dụng trực tiếp địa chỉ Elastic IP.

+ **Hỗ trợ HTTPS:** AWS Certificate Manager quản lý chứng chỉ SSL/TLS, giúp mã hóa dữ liệu truyền giữa người dùng và hệ thống.

+ **Tăng cường bảo mật ứng dụng web:** AWS WAF hỗ trợ lọc và kiểm soát các request trước khi chúng được chuyển đến CloudFront và các thành phần phía sau.

+ **Giảm tải cho EC2:** Việc phục vụ frontend thông qua Amazon S3 và CloudFront giúp giảm lượng request và lưu lượng mà máy chủ EC2 phải xử lý.

+ **Quản lý lưu trữ rõ ràng hơn:** Frontend Bucket được sử dụng cho các file giao diện tĩnh, trong khi Receipts Bucket được sử dụng riêng cho các file do người dùng tải lên.

+ **Cải thiện khả năng mở rộng:** Frontend và backend có thể được nâng cấp hoặc mở rộng độc lập theo nhu cầu sử dụng của hệ thống.

+ **Cải thiện khả năng giám sát:** Amazon CloudWatch, Amazon SNS và AWS Budgets giúp nhóm theo dõi trạng thái hoạt động, nhận cảnh báo sự cố và kiểm soát chi phí tốt hơn.

Kiến trúc này tạo nền tảng cho các giai đoạn mở rộng tiếp theo của Splitly, chẳng hạn như bổ sung Application Load Balancer, Auto Scaling, Amazon Cognito, AWS Lambda hoặc quy trình triển khai tự động CI/CD.

---

## 2.4 Technical Implement

Dự án tập trung triển khai toàn bộ ứng dụng (bao gồm cả frontend và backend) trên cùng một máy chủ Amazon EC2, kết hợp với các dịch vụ đám mây bổ trợ. Quá trình này được thực hiện qua 4 giai đoạn:

### Giai đoạn 1 – Nghiên cứu và thiết kế kiến trúc

Phân tích cấu trúc mã nguồn của dự án (phân tách thư mục backend và frontend/app); nghiên cứu các dịch vụ Amazon EC2, VPC, IAM, Amazon S3 (lưu trữ file), CloudWatch, SNS, AWS Budgets cùng cơ sở dữ liệu MongoDB Atlas và các công cụ như Nginx, PM2 để xây dựng kiến trúc triển khai phù hợp.

### Giai đoạn 2 – Tính toán chi phí và kiểm tra tính khả thi

Sử dụng AWS Pricing Calculator để ước tính chi phí cho máy chủ EC2, S3 (cho biên lai), lưu lượng mạng và CloudWatch; lựa chọn loại instance phù hợp với quy mô dự án sinh viên và thiết lập AWS Budgets để kiểm soát chi phí hàng tháng.

### Giai đoạn 3 – Thiết lập và cấu hình hạ tầng

Tạo hạ tầng mạng cơ bản gồm VPC, Public Subnet, Internet Gateway và Security Group; khởi tạo máy chủ EC2 và gán Elastic IP; tạo một S3 Bucket duy nhất dành cho việc lưu trữ biên lai; cấu hình IAM Role gắn cho EC2; thiết lập MongoDB Atlas, CloudWatch, SNS và AWS Budgets.

### Giai đoạn 4 – Phát triển, kiểm thử và triển khai

Clone mã nguồn từ GitHub xuống máy chủ EC2; cấu hình các biến môi trường nhạy cảm qua tệp `.env`. Cài đặt và build frontend (React/Vite), đồng thời cấu hình Nginx để phục vụ các file tĩnh và làm reverse proxy. Build backend (Node.js/Express) và duy trì tiến trình bằng PM2; kết nối thành công backend với MongoDB, S3 Receipts Bucket và các dịch vụ bên thứ 3 (VNPay, Gmail). Cuối cùng là kiểm thử API, chức năng upload biên lai, giám sát log và đưa hệ thống vào vận hành.

### Yêu cầu kỹ thuật (Bản cập nhật)

#### Kiến trúc và Hạ tầng

Hệ thống được triển khai tại AWS Region Singapore (ap-southeast-1). Toàn bộ ứng dụng (frontend và backend) được lưu trữ và vận hành tập trung trên một máy chủ Amazon EC2 duy nhất, đặt trong Public Subnet của một VPC.

#### Công nghệ

* **Frontend:** Sử dụng React, TypeScript và Vite.
* **Backend:** Sử dụng Node.js, Express và TypeScript, cung cấp REST API.
* **Web Server & Process Manager:** Cài đặt Nginx làm Web Server và Reverse Proxy; sử dụng PM2 để quản lý và tự động khởi động lại tiến trình backend.

#### Quản lý Mã nguồn và Triển khai

Mã nguồn được quản lý trên GitHub. Quá trình triển khai (cài đặt dependencies, build code) được thực hiện trực tiếp trên môi trường EC2 thông qua command line.

#### Dữ liệu và Lưu trữ

Dữ liệu nghiệp vụ cốt lõi được lưu trữ an toàn trên dịch vụ MongoDB Atlas. Các file tĩnh (hình ảnh, biên lai) do người dùng tải lên được đẩy sang Amazon S3 Receipts Bucket để tối ưu lưu trữ.

#### Mạng và Kết nối

Máy chủ EC2 giao tiếp với Internet thông qua Internet Gateway và Elastic IP. Security Group được thiết lập để mở cổng 80 (HTTP) phục vụ người dùng web và cổng 22 (SSH) cho quản trị viên. Nginx chịu trách nhiệm định tuyến traffic: trả về file frontend tĩnh hoặc proxy luồng API sang cổng 5000 của backend nội bộ.

#### Bảo mật và Phân quyền

Các thông tin nhạy cảm (Database URI, JWT Secret, Key tích hợp) được bảo vệ nội bộ thông qua file biến môi trường (`.env`). Máy chủ EC2 được gắn IAM Role để cấp quyền truy cập vào S3 Receipts Bucket và hệ thống CloudWatch tuân thủ chặt chẽ nguyên tắc đặc quyền tối thiểu (Least Privilege).

#### Giám sát và Cảnh báo

Amazon CloudWatch được cấu hình để thu thập metric máy chủ và log ứng dụng. Amazon SNS đóng vai trò làm kênh truyền tải, gửi cảnh báo đến email của quản trị viên khi hệ thống gặp sự cố hoặc tài nguyên bị tắc nghẽn.

#### Quản lý Chi phí

Dịch vụ AWS Budgets liên tục theo dõi chi phí sử dụng hệ sinh thái AWS và tự động gửi cảnh báo khi mức chi tiêu thực tế hoặc dự báo tiệm cận với giới hạn ngân sách đã thiết lập.

#### Yêu cầu Phi chức năng

Hệ thống được đặt tại khu vực Singapore nhằm tối ưu hóa, giảm thiểu độ trễ mạng khi truy cập từ Việt Nam. Kiến trúc trên EC2 hoàn toàn phù hợp với ngân sách dự án sinh viên và có khả năng mở rộng dọc (Scale-up) cấu hình RAM/CPU nếu lượng truy cập gia tăng.

---

## 2.5 Timeline

Dự án được triển khai theo 4 giai đoạn chính trong khoảng 3 tháng nhằm đảm bảo quá trình phát triển, kiểm thử và triển khai được thực hiện có hệ thống.

### Giai đoạn 1 – Phân tích yêu cầu và thiết kế hệ thống (Tuần 5 - Tuần 6)

* Phân tích yêu cầu nghiệp vụ của hệ thống quản lý chi tiêu nhóm.
* Thiết kế kiến trúc tổng thể trên AWS.
* Thiết kế cơ sở dữ liệu MongoDB Atlas.
* Xây dựng giao diện và kiến trúc Backend.

### Giai đoạn 2 – Phát triển chức năng (Tuần 7 – Tuần 8)

* Phát triển Frontend bằng React + Vite.
* Phát triển API bằng Node.js và Express.
* Tích hợp MongoDB Atlas.
* Xây dựng chức năng:

  * Authentication
  * Group Management
  * Expense Management
  * Settlement
  * Receipt Upload

### Giai đoạn 3 – Triển khai AWS (Tuần 9 – Tuần 10)

* Khởi tạo Amazon EC2.
* Cấu hình VPC, Security Group và Elastic IP.
* Triển khai Backend và Frontend lên EC2.
* Tạo Amazon S3 Bucket lưu trữ Receipt.
* Cấu hình IAM Role.
* Thiết lập CloudWatch Monitoring.

### Giai đoạn 4 – Kiểm thử và hoàn thiện (Tuần 11 – Tuần 12)

* Kiểm thử chức năng.
* Kiểm thử API.
* Kiểm thử khả năng upload Receipt.
* Kiểm tra Logging và Monitoring.
* Tối ưu chi phí AWS.
* Hoàn thiện tài liệu và báo cáo.

---

## 2.6 Budget Estimation

Hệ thống được thiết kế hướng đến quy mô nhỏ phục vụ mục đích học tập và thử nghiệm, do đó ưu tiên sử dụng tối đa các dịch vụ thuộc AWS Free Tier và các dịch vụ có chi phí thấp nhất có thể.

### Chi phí hạ tầng dự kiến

* **Amazon EC2:** 0,00 USD/tháng (Sử dụng Instance t3.micro, nằm trong AWS Free Tier, 750 giờ/tháng. Đảm nhiệm chạy cả Web Server Nginx và Backend Node.js).

* **Amazon S3 Standard:** 0,10 USD/tháng (Dự kiến 5 GB lưu trữ cho Receipts Bucket, với khoảng 2.000 requests PUT/GET).

* **Amazon CloudWatch:** 0,03 USD/tháng (Đẩy metric giám sát EC2 cơ bản và lưu trữ log của ứng dụng).

* **Amazon SNS:** 0,00 USD/tháng (Dự kiến 100 email cảnh báo mỗi tháng, hoàn toàn nằm trong Free Tier).

* **Amazon VPC:** 0,00 USD/tháng (Bao gồm 1 VPC, Public Subnet, Internet Gateway, Route Table và Security Group).

* **Elastic IP:** 3,65 USD/tháng (AWS tính phí 0.005 USD/giờ cho tất cả địa chỉ Public IPv4, bao gồm cả Elastic IP đã gắn vào EC2).

* **AWS IAM:** 0,00 USD/tháng (Quản lý IAM Role và quyền truy cập cho hệ thống).

**Tổng chi phí ước tính:** Khoảng 3,78 USD/tháng, tương đương 45,36 USD/12 tháng.

Trong giai đoạn phát triển, nhóm dự kiến tận dụng tối đa gói AWS Free Tier, kết hợp với việc lưu trữ cấu hình bảo mật trực tiếp trên máy chủ (thông qua tệp `.env`) nhằm tối ưu hóa chi phí vận hành xuống mức thấp nhất. Sau khi hệ thống được đưa vào vận hành ổn định, chi phí thực tế sẽ được theo dõi liên tục qua AWS Budgets và có thể tính toán lại bằng AWS Pricing Calculator nếu lưu lượng người dùng thực tế vượt mức dự kiến ban đầu.

---

## 2.7 Risk

### Ma trận rủi ro

* **EC2 gặp sự cố:** Ảnh hưởng cao, xác suất thấp.
* **MongoDB Atlas mất kết nối:** Ảnh hưởng cao, xác suất thấp.
* **Upload Receipt thất bại:** Ảnh hưởng trung bình, xác suất thấp.
* **Lộ thông tin Secret:** Ảnh hưởng cao, xác suất trung bình.
* **Cấu hình Security Group sai:** Ảnh hưởng cao, xác suất thấp.
* **Vượt ngân sách AWS Free Tier:** Ảnh hưởng trung bình, xác suất trung bình.

### Chiến lược giảm thiểu

* Thiết lập CloudWatch và SNS để theo dõi tình trạng EC2.
* Thiết lập AWS Budget nhằm cảnh báo khi chi phí vượt ngưỡng.
* Sử dụng IAM Role thay cho Access Key khi truy cập dịch vụ AWS.
* Thiết lập Security Group theo nguyên tắc chỉ mở các cổng cần thiết.
* Kiểm tra định kỳ kết nối giữa EC2 và MongoDB Atlas.

### Kế hoạch dự phòng

* Khởi động lại hoặc triển khai lại EC2 từ source code khi xảy ra sự cố.
* Khôi phục dữ liệu Receipt từ Amazon S3.
* Khôi phục cấu hình từ GitHub Repository.
* Chuyển sang MongoDB Atlas Backup nếu cơ sở dữ liệu gặp lỗi.
* Điều chỉnh cấu hình dịch vụ hoặc giới hạn tài nguyên khi chi phí vượt ngân sách.
