---
title: "Blog 2"
date: 2026-07-17
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Cách Scale to Win tăng cường khả năng chống DDoS bằng AWS WAF

> **Nguồn gốc:** [AWS Architecture Blog — How Scale to Win uses AWS WAF to block DDoS events](https://aws.amazon.com/blogs/architecture/how-scale-to-win-uses-aws-waf-to-block-ddos-events/)

**Tác giả:** Ben “Fuzzy” Shonaldmann, Fenil Patel và Patrick Duffy  
**Ngày xuất bản:** 14 tháng 7 năm 2025  
**Danh mục:** AWS Architecture, AWS WAF, Customer Solutions

## Bối cảnh

Scale to Win là một công ty công nghệ phục vụ chiến dịch, được thành lập bởi những người tổ chức có kinh nghiệm làm việc trong các chiến dịch chính trị lớn tại Hoa Kỳ. Nền tảng của công ty ban đầu tập trung vào nhắn tin ngang hàng, sau đó mở rộng để hỗ trợ gọi điện, gây quỹ và các hoạt động truyền thông chiến dịch rộng hơn.

Trong chu kỳ bầu cử tổng thống Hoa Kỳ năm 2024, công ty trở thành mục tiêu của các cuộc tấn công từ chối dịch vụ phân tán.

Tại thời điểm cao nhất, hệ thống của Scale to Win nhận hơn hai triệu yêu cầu mỗi giây từ gần mười nghìn địa chỉ IP khác nhau.

Để cải thiện tính bảo mật và khả năng sẵn sàng của dịch vụ, Scale to Win đã hợp tác với [Amazon Web Services](https://aws.amazon.com/) và triển khai một kiến trúc bảo vệ nhiều lớp gồm:

- [AWS WAF](https://aws.amazon.com/waf/)
- [AWS Shield Advanced](https://aws.amazon.com/shield/)
- [Amazon CloudFront](https://aws.amazon.com/cloudfront/)

Một phần quan trọng của giải pháp là sử dụng [AWS WAF CAPTCHA and Challenge](https://docs.aws.amazon.com/waf/latest/developerguide/waf-captcha-and-challenge.html).

Các client đáng ngờ phải hoàn thành CAPTCHA sau khi vượt quá ngưỡng tốc độ yêu cầu thấp hơn. Một ngưỡng tốc độ cao hơn cũng được thiết lập để chặn lượng truy cập cực lớn đến từ cùng một địa chỉ IP dùng chung.

Công ty cũng cần xử lý một kỹ thuật tấn công nâng cao hơn. Kẻ tấn công có thể giải một CAPTCHA, sao chép token nhận được, sau đó phân phối token đó cho nhiều máy khác nhau.

Vì vậy, kiến trúc cuối cùng kết hợp bảo vệ tại edge, phân loại lưu lượng, quy tắc giới hạn tốc độ, CAPTCHA challenge và cơ chế kiểm soát bot.

## 1. Đưa lớp bảo vệ ra AWS Edge

### Kiến trúc ban đầu

Trong kiến trúc ban đầu, các bản ghi DNS công khai trỏ trực tiếp đến Application Load Balancer.

Load balancer đóng vai trò là điểm truy cập công khai và chuyển tiếp các yêu cầu đến một Auto Scaling group gồm các máy chủ [Amazon EC2](https://aws.amazon.com/ec2/).

![Kiến trúc ban đầu của Scale to Win với Application Load Balancer kết nối trực tiếp tới các máy chủ Amazon EC2 trong Auto Scaling group](/aws_internship/images/blog2/original-architecture.png)

Mặc dù kiến trúc này đáp ứng được lưu lượng ứng dụng thông thường, Application Load Balancer vẫn bị phơi trực tiếp trước lượng lớn yêu cầu độc hại.

### Kiến trúc được cập nhật

Trong kiến trúc mới, Amazon CloudFront và AWS WAF được đặt phía trước load balancer.

![Kiến trúc được cập nhật với Amazon CloudFront và AWS WAF được đặt phía trước Application Load Balancer](/aws_internship/images/blog2/cloudfront-waf-architecture.png)

Kiến trúc này mang lại một số lợi ích quan trọng.

Thứ nhất, CloudFront có thể hấp thụ và xử lý lượng truy cập rất lớn tại AWS Edge trước khi các yêu cầu đi đến hạ tầng trong khu vực.

Thứ hai, AWS WAF được triển khai cùng CloudFront có khả năng mở rộng hiệu quả hơn so với web ACL chỉ được gắn trực tiếp vào Application Load Balancer trong một region.

Thứ ba, CloudFront có thể từ chối lưu lượng đến từ những vị trí địa lý cụ thể thông qua [geographic restrictions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/georestrictions.html).

Một tỷ lệ đáng kể lưu lượng độc hại mà Scale to Win nhận được đến từ những khu vực mà công ty không cần cung cấp dịch vụ.

Việc chặn những vị trí này tại lớp CloudFront giúp giảm cả lượng lưu lượng đi vào AWS WAF lẫn chi phí xử lý yêu cầu liên quan.

## 2. Ngăn truy cập trực tiếp vào load balancer

Việc đặt CloudFront phía trước ứng dụng chưa thể bảo vệ hoàn toàn origin nếu kẻ tấn công vẫn có thể kết nối trực tiếp với Application Load Balancer.

Scale to Win bổ sung hai biện pháp kiểm soát để ngăn người dùng bỏ qua CloudFront.

### Chỉ cho phép lưu lượng từ CloudFront

Security group gắn với Application Load Balancer được cấu hình để chỉ chấp nhận kết nối từ các CloudFront edge location.

Cấu hình này sử dụng AWS-managed CloudFront prefix list.

Thông tin chi tiết có thể xem tại [CloudFront edge locations and IP address ranges](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/LocationsOfEdgeServers.html).

Sau khi quy tắc này được áp dụng, lưu lượng trực tiếp từ Internet công cộng không thể kết nối đến load balancer nếu không xuất phát từ một địa chỉ CloudFront hợp lệ.

### Thêm shared secret header

CloudFront cũng được cấu hình để thêm một custom header bí mật vào mọi yêu cầu gửi đến origin.

Ví dụ:

```text
x-stw-example-secret: <private-value>
```
![Cấu hình custom header trong Amazon CloudFront origin settings](/aws_internship/images/blog2/cloudfront-custom-header.png)

Listener của Application Load Balancer kiểm tra header này trước khi chuyển tiếp yêu cầu đến ứng dụng.

```text
NẾU x-stw-example-secret chứa giá trị chính xác
    Chuyển tiếp yêu cầu đến application target group
NGƯỢC LẠI
    Trả về HTTP 403
```
![Listener rule của Application Load Balancer kiểm tra shared secret header trước khi chuyển tiếp yêu cầu](/aws_internship/images/blog2/alb-listener-secret-rule.png)

Biện pháp này ngăn kẻ tấn công tạo một CloudFront distribution riêng và cấu hình distribution đó sử dụng cùng một Application Load Balancer làm origin.

Ngay cả khi lưu lượng đến từ một địa chỉ IP CloudFront được cho phép, yêu cầu vẫn bị từ chối nếu không chứa đúng giá trị header bí mật.

Giá trị bí mật cũng có thể được tự động xoay vòng bằng cách sử dụng:

* [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/)
* [AWS Lambda](https://aws.amazon.com/lambda/)
* [AWS CloudTrail](https://aws.amazon.com/cloudtrail/)

CloudTrail có thể ghi lại các thay đổi cấu hình, trong khi Secrets Manager và Lambda có thể tạo giá trị bí mật mới và cập nhật đồng thời cấu hình origin của CloudFront và listener rule của Application Load Balancer.

## 3. Kết hợp quy tắc heuristic với giới hạn tốc độ cứng

Scale to Win sử dụng hai phương pháp bổ trợ để phát hiện và ngăn chặn lưu lượng độc hại:

1. Phát hiện heuristic dựa trên các đặc điểm đáng ngờ của yêu cầu.
2. Giới hạn tốc độ cứng dựa trên những thuộc tính khó bị kẻ tấn công thao túng hơn.

### Phát hiện heuristic

Nhóm kỹ thuật xem xét:

* [AWS WAF sampled web requests](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing-view-sample.html)
* [AWS WAF traffic logs](https://docs.aws.amazon.com/waf/latest/developerguide/logging.html)

Mục tiêu là tìm ra các thuộc tính xuất hiện thường xuyên trong lưu lượng tấn công nhưng hiếm gặp trong các yêu cầu hợp lệ.

Các dấu hiệu có thể bao gồm:

* HTTP header bất thường
* Giá trị query string đáng ngờ
* Đường dẫn yêu cầu không mong đợi
* Mẫu request body lặp lại
* Sự kết hợp bất thường giữa các thuộc tính của yêu cầu

Quy trình thực tế để kiểm thử các quy tắc heuristic gồm:

1. Tạo một [AWS WAF rule](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rules.html) khớp với mẫu đáng ngờ.
2. Ban đầu cấu hình quy tắc với action `Count` thay vì chặn lưu lượng.
3. So sánh các kết quả khớp với những địa chỉ IP có lưu lượng cao.
4. Sử dụng [Amazon Athena](https://aws.amazon.com/athena/) để [truy vấn AWS WAF logs](https://docs.aws.amazon.com/athena/latest/ug/waf-logs.html).
5. Kiểm tra các trường hợp false positive và false negative.
6. Chuyển action của quy tắc sang `Block` sau khi xác nhận độ chính xác đạt yêu cầu.

Việc bắt đầu bằng action `Count` cho phép nhóm kiểm thử quy tắc với lưu lượng production mà không làm gián đoạn người dùng hợp lệ.

Tuy nhiên, phương pháp heuristic có một hạn chế quan trọng. Kẻ tấn công có thể thay đổi các đặc điểm của yêu cầu sau khi phát hiện những giá trị nào đang bị chặn.

Ví dụ, kẻ tấn công có thể:

* Ngẫu nhiên hóa HTTP header
* Thay đổi request path
* Phát lại dữ liệu session thực tế
* Thay đổi query parameter
* Mô phỏng hành vi trình duyệt thông thường

Vì vậy, các quy tắc heuristic cần được theo dõi và điều chỉnh liên tục.

### JA4 và JA3 Fingerprints

Các TLS fingerprint như JA4 và JA3 có thể hỗ trợ phân nhóm client dựa trên đặc điểm của quá trình TLS handshake.

Một lượng lớn client tự động sử dụng cùng một phần mềm có thể tạo ra fingerprint giống hoặc gần giống nhau.

Tuy nhiên, việc chặn dựa trên fingerprint có hai rủi ro.

Thứ nhất, một client độc hại có thể dùng chung fingerprint với một trình duyệt, runtime hoặc thư viện API hợp lệ. Việc chặn fingerprint đó có thể ảnh hưởng đến người dùng thật.

Thứ hai, những kẻ tấn công nâng cao có thể chủ động thay đổi một phần TLS handshake để tạo ra nhiều fingerprint khác nhau.

Vì những lý do này, TLS fingerprint là một tín hiệu bảo mật hữu ích nhưng không nên được dùng làm cơ chế chặn duy nhất.

### Giới hạn tốc độ cứng

Phương pháp thứ hai dựa trên những thuộc tính khó giả mạo hơn, đặc biệt là địa chỉ IP nguồn.

Mặc dù packet mạng có thể chứa địa chỉ bị giả mạo, một client giả mạo thường không thể hoàn thành TCP và TLS handshake cần thiết để gửi một HTTP request hợp lệ và nhận phản hồi.

AWS WAF hỗ trợ giới hạn yêu cầu theo nguồn thông qua [rate-based rule statements](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html).

Tuy nhiên, Scale to Win không thể sử dụng một giới hạn duy nhất cho toàn bộ lưu lượng.

Các thành phần khác nhau của nền tảng có mẫu lưu lượng rất khác nhau.

Ví dụ:

* Một số API nhận lượng lớn lưu lượng do máy tạo ra.
* Các nhà cung cấp như Twilio có thể gửi hàng nghìn callback từ một số lượng nhỏ địa chỉ IP.
* Văn phòng chiến dịch có thể có hàng trăm người dùng hợp lệ cùng sử dụng một địa chỉ IP công khai.
* Trường đại học hoặc phone bank cũng có thể tạo ra lượng lớn lưu lượng hợp lệ từ mạng dùng chung.

Một giới hạn đủ thấp để chặn kẻ tấn công cũng có thể vô tình chặn toàn bộ một văn phòng chiến dịch.

Vì vậy, công ty phân loại lưu lượng thành những nhóm khác nhau trước khi áp dụng giới hạn tốc độ.

## 4. Phân tách lưu lượng máy với lưu lượng trình duyệt

AWS WAF rule có thể kiểm tra và so khớp URI path của yêu cầu.

Scale to Win sử dụng các mẫu URI để phân biệt endpoint machine-to-machine với các route được người dùng truy cập thông qua trình duyệt web.

```text
Webhook và API path đã biết -> Lưu lượng máy
Các application path còn lại -> Lưu lượng trình duyệt
```

Mỗi loại lưu lượng sau đó được đánh giá bằng một chiến lược bảo vệ khác nhau.

### Lưu lượng machine-to-machine

CAPTCHA không phù hợp với API client, webhook service hoặc các hệ thống tự động khác vì những client này không thể tương tác với thử thách trực quan.

Đối với API client, Scale to Win thiết lập giới hạn theo IP phù hợp với lưu lượng dự kiến.

Khi một client vượt quá tốc độ cho phép, ứng dụng hoặc lớp bảo vệ trả về:

```text
HTTP 429 Too Many Requests
```

API client được thiết kế để nhận biết lỗi tạm thời và thử lại yêu cầu sau đó.

Đối với webhook provider, nhóm tạo một [AWS WAF IP set](https://docs.aws.amazon.com/waf/latest/developerguide/waf-ip-set-managing.html) chứa các địa chỉ IP đáng tin cậy của nhà cung cấp.

Một yêu cầu gửi đến webhook route sẽ bị từ chối nếu địa chỉ IP nguồn không nằm trong IP set được phê duyệt.

![AWS WAF rule cho phép machine traffic khi URI path và địa chỉ IP nguồn khớp với điều kiện đã cấu hình](/aws_internship/images/blog2/waf-machine-traffic-rule-1.png)

![AWS WAF rule cho phép machine traffic khi URI path và địa chỉ IP nguồn khớp với điều kiện đã cấu hình](/aws_internship/images/blog2/waf-machine-traffic-rule-2.png)

Khi có thể, lưu lượng machine-to-machine cũng nên sử dụng những cơ chế xác thực mạnh hơn như:

* API key
* Yêu cầu được ký bằng mật mã
* Xác thực dựa trên certificate
* Địa chỉ IP nguồn ổn định
* Webhook signature của nhà cung cấp
* Request timestamp và cơ chế chống replay

Lưu lượng máy hợp lệ được cho phép trước khi các quy tắc CAPTCHA và blocking dành cho trình duyệt được đánh giá.

### Lưu lượng trình duyệt

Lưu lượng trình duyệt được kiểm soát bằng mô hình giới hạn tốc độ hai tầng.

#### Ngưỡng tốc độ thấp hơn

Ngưỡng thấp hơn đại diện cho số lượng yêu cầu tối đa dự kiến từ một nhóm nhỏ người dùng cùng chia sẻ một địa chỉ IP.

Khi một địa chỉ IP vượt quá ngưỡng này, AWS WAF trả về CAPTCHA challenge.

CAPTCHA giúp phân biệt người dùng thật với các client tự động cơ bản.

#### Ngưỡng tốc độ cao hơn

Ngưỡng cao hơn đại diện cho lưu lượng hợp lệ tối đa dự kiến từ một văn phòng lớn, trường đại học, phone bank hoặc mạng dùng chung khác.

Khi một địa chỉ IP vượt quá ngưỡng cao hơn, AWS WAF chặn hoàn toàn lưu lượng đó.

Quy tắc blocking ở ngưỡng cao hơn cần được đánh giá trước quy tắc CAPTCHA ở ngưỡng thấp hơn.

Thứ tự này rất quan trọng vì việc giải thành công CAPTCHA không nên cho phép một client gửi số lượng yêu cầu không giới hạn.

Hành vi mong đợi như sau:

```text
Địa chỉ IP dùng chung bình thường
    -> Không bị gián đoạn

Địa chỉ IP dùng chung có lưu lượng cao nhưng hợp lệ
    -> Yêu cầu CAPTCHA

Địa chỉ IP có lưu lượng cực lớn
    -> Yêu cầu bị chặn
```

![Thứ tự đánh giá các quy tắc trong AWS WAF Web ACL](/aws_internship/images/blog2/waf-rule-priority.png)

Cách tiếp cận này cho phép Scale to Win bảo vệ ứng dụng mà không vô tình chặn những người dùng hợp lệ đang làm việc từ các mạng dùng chung.

## 5. Tích hợp CAPTCHA vào frontend

Đối với các website server-rendered truyền thống, AWS WAF có thể tự động xử lý phần lớn quy trình CAPTCHA.

Quy trình diễn ra như sau:

1. Một yêu cầu đi đến quy tắc được bảo vệ bằng CAPTCHA nhưng không có token hợp lệ.
2. AWS WAF hiển thị trang CAPTCHA được quản lý.
3. Người dùng hoàn thành CAPTCHA.
4. AWS WAF lưu token vào cookie của trình duyệt.
5. Những yêu cầu tiếp theo được cho phép trong thời gian miễn kiểm tra đã cấu hình.

Single-page application cần thêm logic ở frontend vì phần lớn lưu lượng được gửi thông qua background API request thay vì tải lại toàn bộ trang.

Một ứng dụng React có thể sử dụng quy trình sau:

```text
Gửi API request bằng AwsWafIntegration.fetch()
    |
    +-- HTTP 200
    |      -> Phân tích response
    |      -> Hiển thị nội dung được yêu cầu
    |
    +-- HTTP 405
           -> Hiển thị AWS WAF CAPTCHA
                  |
                  +-- CAPTCHA thành công
                  |      -> Gửi lại API request ban đầu
                  |
                  +-- CAPTCHA thất bại
                         -> Hiển thị lỗi ứng dụng
```

Ứng dụng frontend sử dụng AWS WAF JavaScript integration và CAPTCHA API để phát hiện khi nào người dùng cần hoàn thành challenge.

Sau khi người dùng hoàn thành challenge thành công, yêu cầu ban đầu được gửi lại cùng AWS WAF token hợp lệ.

Phần triển khai React đầy đủ có trong [bài viết AWS Architecture Blog gốc](https://aws.amazon.com/blogs/architecture/how-scale-to-win-uses-aws-waf-to-block-ddos-events/).

## 6. Ngăn việc tái sử dụng CAPTCHA token

Một CAPTCHA token hợp lệ vẫn có thể trở thành rủi ro bảo mật nếu kẻ tấn công chỉ giải một challenge rồi phân phối token đó cho nhiều máy.

Điều này cho phép nhiều client tự động tạo cảm giác như chúng đã hoàn thành CAPTCHA thành công.

[AWS WAF Bot Control](https://aws.amazon.com/waf/features/bot-control/) có thể phát hiện các mẫu tái sử dụng token đáng ngờ giữa:

* Nhiều địa chỉ IP khác nhau
* Nhiều autonomous system number khác nhau
* Nhiều quốc gia khác nhau
* Nhiều vị trí mạng khác nhau

Scale to Win sử dụng phương pháp triển khai sau:

1. Thêm [AWSManagedRulesBotControlRuleSet](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups-bot.html) sau các quy tắc giới hạn tốc độ tùy chỉnh.
2. Sử dụng mức kiểm tra và bảo vệ targeted.
3. Ghi đè từng managed rule bằng `Count` hoặc `Allow` khi quy tắc đó không nên chặn một nhóm lưu lượng hợp lệ cụ thể.
4. Theo dõi các yêu cầu trong môi trường production.
5. Điều chỉnh action của các quy tắc dựa trên mẫu lưu lượng thực tế.

Việc đặt Bot Control sau các rate-based rule tùy chỉnh cho phép nền tảng áp dụng chính sách lưu lượng riêng trước.

Sau đó, các managed bot rule có thể phát hiện việc chia sẻ CAPTCHA token, trình duyệt tự động và những hành vi đáng ngờ khác mà các giới hạn tùy chỉnh chưa phát hiện được.

## Kiến trúc cuối cùng

Quy trình bảo vệ hoàn chỉnh có thể được tóm tắt như sau:

```text
Người dùng Internet
      |
      v
Amazon CloudFront
      |
      +-- Geographic restrictions
      |
      v
AWS WAF
      |
      +-- Heuristic rules
      +-- Quy tắc dành cho lưu lượng máy đáng tin cậy
      +-- Quy tắc API và webhook
      +-- Giới hạn tốc độ chặn ở ngưỡng cao
      +-- Giới hạn tốc độ CAPTCHA ở ngưỡng thấp
      +-- AWS WAF Bot Control
      |
      v
Application Load Balancer
      |
      +-- Security group chỉ cho phép CloudFront
      +-- Kiểm tra shared-secret header
      |
      v
Máy chủ ứng dụng trên Amazon EC2
```

## Những điểm chính

Chiến lược chống DDoS của Scale to Win không phụ thuộc vào một dịch vụ AWS duy nhất hoặc một quy tắc bảo mật duy nhất.

Thay vào đó, công ty sử dụng nhiều lớp phòng vệ.

* Amazon CloudFront hấp thụ và lọc lưu lượng tại AWS Edge.
* AWS WAF kiểm tra yêu cầu và áp dụng các quy tắc bảo mật tùy chỉnh.
* AWS Shield Advanced cung cấp thêm khả năng bảo vệ DDoS.
* Geographic restrictions chặn lưu lượng từ những khu vực không cần thiết.
* Application Load Balancer chỉ chấp nhận lưu lượng từ CloudFront.
* Custom header bí mật ngăn các CloudFront distribution không được phép truy cập origin.
* Lưu lượng máy và lưu lượng trình duyệt sử dụng các luồng xác thực khác nhau.
* Webhook provider đáng tin cậy được xác minh bằng IP set.
* API client được kiểm soát bằng các giới hạn tốc độ phù hợp.
* Ngưỡng yêu cầu thấp hơn kích hoạt CAPTCHA challenge.
* Ngưỡng cao hơn chặn lưu lượng cực lớn.
* AWS WAF Bot Control phát hiện việc tái sử dụng CAPTCHA token và các hành vi tự động khác.

Cách tiếp cận nhiều lớp này giúp Scale to Win bảo vệ ứng dụng trước các cuộc tấn công HTTP phân tán quy mô lớn, đồng thời vẫn duy trì khả năng truy cập cho người dùng hợp lệ, bao gồm những người đang làm việc tại văn phòng hoặc sử dụng mạng dùng chung.

## Tài liệu tham khảo thêm

* [AWS WAF](https://aws.amazon.com/waf/)
* [AWS Shield](https://aws.amazon.com/shield/)
* [Amazon CloudFront](https://aws.amazon.com/cloudfront/)
* [AWS WAF CAPTCHA and Challenge](https://docs.aws.amazon.com/waf/latest/developerguide/waf-captcha-and-challenge.html)
* [AWS WAF rate-based rules](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html)
* [AWS WAF Bot Control](https://aws.amazon.com/waf/features/bot-control/)
* [AWS WAF sampled web requests](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing-view-sample.html)
* [AWS WAF logging](https://docs.aws.amazon.com/waf/latest/developerguide/logging.html)
* [Querying AWS WAF logs with Amazon Athena](https://docs.aws.amazon.com/athena/latest/ug/waf-logs.html)
* [AWS WAF IP sets](https://docs.aws.amazon.com/waf/latest/developerguide/waf-ip-set-managing.html)
* [Fine-tune and optimize AWS WAF Bot Control mitigation capability](https://aws.amazon.com/blogs/security/fine-tune-and-optimize-aws-waf-bot-control-mitigation-capability/)

## Về các tác giả

### Ben “Fuzzy” Shonaldmann

Ben là một kỹ sư phần mềm có kinh nghiệm xây dựng, mở rộng và bảo mật các sản phẩm công nghệ. Kinh nghiệm nghề nghiệp của ông cũng bao gồm các chiến dịch chính trị, tổ chức vận động cử tri và các công ty công nghệ chính trị.

### Fenil Patel

Fenil là AWS Solutions Architect làm việc tại New Jersey. Công việc của ông tập trung vào việc hỗ trợ khách hàng cải thiện và bảo mật quá trình phân phối nội dung thông qua các dịch vụ AWS Edge.

### Patrick Duffy

Patrick là AWS Solutions Architect làm việc với các khách hàng thương mại. Ông tập trung vào việc nâng cao nhận thức về bảo mật đám mây và giúp các tổ chức tăng cường bảo mật cho workload trên AWS.

