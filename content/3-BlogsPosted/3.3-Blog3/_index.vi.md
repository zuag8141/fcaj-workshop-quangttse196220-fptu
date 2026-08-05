---
title: "Blog 3"
date: 2026-07-17
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
# Kiến trúc cho phát triển Agentic AI trên AWS

> **Bài viết gốc:** [Architecting for agentic AI development on AWS](https://aws.amazon.com/blogs/architecture/architecting-for-agentic-ai-development-on-aws/)

**Tác giả:** Alan Oberto Jimenez  
**Ngày xuất bản:** 26 tháng 3 năm 2026  
**Danh mục:** Architecture, Intermediate (200), Kiro

## Giới thiệu

Những kiến trúc đám mây được thiết kế cho quy trình phát triển phần mềm truyền thống thường tạo ra nhiều trở ngại khi được sử dụng cùng AI agent.

Nhiều nhóm phát triển đang thử nghiệm AI coding assistant và các agent tự chủ. Tuy nhiên, họ nhanh chóng nhận ra khoảng cách giữa khả năng mà các công cụ này hứa hẹn với những gì kiến trúc hiện tại thực sự cho phép.

AI agent có thể tạo hoặc chỉnh sửa mã nguồn chỉ trong vài giây, nhưng việc xác thực thay đổi đó vẫn có thể mất nhiều phút hoặc thậm chí nhiều giờ. Chu kỳ triển khai chậm, các dịch vụ liên kết quá chặt chẽ, môi trường phát triển dùng chung và codebase thiếu cấu trúc rõ ràng khiến mỗi vòng lặp trở nên khó khăn hơn.

Khi phản hồi đến quá chậm, AI agent không thể hoạt động hiệu quả một cách tự chủ. Lập trình viên phải liên tục quay lại quy trình để triển khai thay đổi, kiểm tra lỗi và xác thực kết quả theo cách thủ công.

Phát triển agentic là một mô hình rộng hơn so với việc chỉ sử dụng AI để đề xuất đoạn mã. Trong quy trình này, AI agent có thể tham gia vào toàn bộ vòng đời phát triển:

- Phân tích yêu cầu
- Thiết kế giải pháp
- Viết mã nguồn
- Chạy kiểm thử
- Triển khai tài nguyên
- Quan sát hành vi của ứng dụng
- Phát hiện lỗi
- Tinh chỉnh phần triển khai

Để hỗ trợ quy trình này, cả kiến trúc hệ thống lẫn kiến trúc codebase cần được xây dựng dựa trên ba nguyên tắc:

1. Xác thực nhanh
2. Thử nghiệm an toàn và cô lập
3. Ý định kiến trúc được thể hiện rõ ràng

Bài viết này trình bày các mẫu kiến trúc giúp xây dựng môi trường AWS phù hợp với phát triển agentic. Nội dung trước tiên giải thích vì sao kiến trúc truyền thống hạn chế AI agent, sau đó giới thiệu các mẫu ở cấp hệ thống để tăng tốc phản hồi và các mẫu ở cấp codebase để giúp agent hiểu, chỉnh sửa và xác thực ứng dụng một cách đáng tin cậy.

## Vì sao kiến trúc truyền thống cản trở Agentic AI

Phần lớn kiến trúc đám mây ban đầu được thiết kế cho quy trình phát triển do con người điều khiển.

Những kiến trúc này thường giả định rằng:

- Môi trường phát triển và kiểm thử tồn tại lâu dài
- Việc xác thực được thực hiện thủ công
- Triển khai diễn ra không thường xuyên
- Các nhóm sử dụng chung môi trường tích hợp tập trung
- Con người có thể tự hiểu những quy ước chưa được tài liệu hóa
- Mã ứng dụng phụ thuộc trực tiếp vào dịch vụ đám mây

Những giả định này không phù hợp với quy trình agentic.

AI agent cần liên tục xác thực các thay đổi. Nếu mỗi lần kiểm thử đều phải cấp phát tài nguyên AWS, chờ pipeline triển khai hoặc xử lý lỗi chỉ xuất hiện sau khi deployment, vòng phản hồi sẽ trở nên quá chậm.

Việc liên kết trực tiếp logic nghiệp vụ với dịch vụ managed cũng gây khó khăn cho kiểm thử cục bộ. Ví dụ, nếu quy tắc ứng dụng gọi trực tiếp Amazon DynamoDB hoặc Amazon SNS, agent sẽ khó xác thực logic đó nếu không có môi trường AWS đã triển khai.

Cấu trúc repository thiếu nhất quán cũng tạo thêm sự không chắc chắn. Agent có thể không xác định được:

- Tính năng mới nên được triển khai ở đâu
- Module nào có thể chỉnh sửa an toàn
- Quy tắc kiến trúc nào cần tuân thủ
- Dịch vụ nào có thể bị ảnh hưởng
- Bài kiểm thử nào có liên quan
- Ứng dụng cần được triển khai như thế nào

Nếu không có sự hỗ trợ từ kiến trúc, phát triển tự chủ có thể tạo ra nhiều rủi ro hơn giá trị.

Giải pháp không chỉ là viết prompt tốt hơn. Hệ thống phải được thiết kế để phản hồi nhanh, cấu trúc có thể dự đoán và ranh giới rõ ràng trở thành những yêu cầu kiến trúc cốt lõi.

## Kiến trúc hệ thống cho vòng phản hồi Agentic nhanh

Hiệu quả của phát triển agentic phụ thuộc rất lớn vào tốc độ agent nhận được kết quả từ công việc của mình.

Một vòng phản hồi nhanh cho phép agent:

1. Thực hiện thay đổi.
2. Chạy bước xác thực phù hợp.
3. Kiểm tra kết quả.
4. Sửa phần triển khai.
5. Lặp lại quy trình.

Chu kỳ này càng ngắn, agent càng có thể tinh chỉnh kết quả hiệu quả hơn.

Một kiến trúc phát triển agentic hoàn chỉnh có thể kết hợp kiểm thử cục bộ, môi trường đám mây ngắn hạn, pipeline triển khai tự động và phản hồi từ production.

![Kiến trúc tổng quan hỗ trợ phát triển Agentic AI trên AWS](/aws_internship/images/blog3/agentic-ai-development-architecture.png)

*Hình 1: Kiến trúc tổng quan cho phát triển agentic, gồm vòng kiểm thử cục bộ, test stack tạm thời và pipeline CI/CD được kích hoạt bởi AI.*

## Sử dụng mô phỏng cục bộ làm vòng phản hồi mặc định

Khi có thể, những thay đổi do AI tạo ra nên được kiểm thử trên máy cục bộ trước khi agent cấp phát hoặc thay đổi tài nguyên đám mây.

Mô phỏng cục bộ mang lại nhiều lợi ích:

- Thực thi nhanh hơn
- Chi phí thấp hơn
- Giảm rủi ro
- Dễ debug hơn
- Có thể kiểm thử thường xuyên hơn
- Ít phụ thuộc vào môi trường dùng chung

### Ứng dụng serverless

Các ứng dụng được xây dựng bằng [AWS Lambda](https://aws.amazon.com/lambda/) và [Amazon API Gateway](https://aws.amazon.com/api-gateway/) có thể được kiểm thử cục bộ bằng [AWS Serverless Application Model](https://aws.amazon.com/serverless/sam/).

Ví dụ, lệnh sau khởi động một API Gateway endpoint được mô phỏng trên máy cục bộ:

```bash
sam local start-api
```

Sau đó, AI agent có thể thực hiện vòng lặp sau:

```text
Chỉnh sửa Lambda function
        |
        v
Khởi động API cục bộ
        |
        v
Gửi test request
        |
        v
Kiểm tra response và log
        |
        v
Cập nhật phần triển khai
```

Vì ứng dụng không cần được triển khai lên đám mây sau mỗi thay đổi, agent có thể xác thực kết quả trong vài giây thay vì phải chờ một deployment hoàn chỉnh.

### Ứng dụng dựa trên container

Các ứng dụng được thiết kế để chạy trên [Amazon Elastic Container Service](https://aws.amazon.com/ecs/) hoặc [AWS Fargate](https://aws.amazon.com/fargate/) có thể sử dụng cùng một container image trong quá trình phát triển cục bộ.

Agent có thể build và chạy container để kiểm tra:

- Quá trình khởi động ứng dụng
- Hành vi API
- Cấu hình môi trường
- Quá trình cài đặt dependency
- Cơ chế xử lý lỗi
- Container health check
- Khả năng tương thích runtime

Việc kiểm thử cùng một image trên máy cục bộ giúp giảm sự khác biệt giữa môi trường phát triển và production.

### Lưu trữ dữ liệu cục bộ

Ứng dụng phụ thuộc vào [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) có thể sử dụng DynamoDB Local.

DynamoDB Local triển khai DynamoDB API trong môi trường cục bộ, cho phép agent kiểm thử:

- Thao tác tạo dữ liệu
- Thao tác đọc dữ liệu
- Thao tác cập nhật
- Thao tác xóa
- Hành vi truy vấn
- Quy tắc xác thực dữ liệu

Nhờ đó, agent có thể xác minh logic truy cập dữ liệu mà không cần kết nối tới bảng AWS dùng chung.

> **Điểm chính:** Mô phỏng cục bộ cho phép mã nguồn do AI tạo ra được xác thực trong vài giây, đồng thời có thể giảm chi phí và rủi ro của quá trình thử nghiệm.

## Phát triển ngoại tuyến cho workload dữ liệu và phân tích

Không phải ứng dụng nào cũng tuân theo mô hình request-response đơn giản.

Các hệ thống xử lý dữ liệu thường liên quan đến:

- Dataset lớn
- Thực thi phân tán
- Batch processing
- Pipeline chuyển đổi
- Quy tắc chất lượng dữ liệu
- Dịch vụ phân tích được quản lý

Ngay cả với những workload này, quá trình phát triển vẫn nên bắt đầu từ vòng phản hồi nhỏ nhất có thể.

[AWS Glue](https://aws.amazon.com/glue/) cung cấp Docker image chứa các thư viện AWS Glue ETL. Những image này cho phép AWS Glue job được thực thi trong container cục bộ.

AI agent có thể sử dụng môi trường này để:

1. Nạp một dataset mẫu có tính đại diện.
2. Chạy logic chuyển đổi cục bộ.
3. Kiểm tra dữ liệu trung gian.
4. Phát hiện lỗi schema và chất lượng dữ liệu.
5. Chỉnh sửa phép chuyển đổi.
6. Xác thực lại kết quả.
7. Chỉ chuyển workload lên AWS khi cần kiểm thử ở quy mô lớn.

Một quy trình chung cho workload dữ liệu và machine learning là:

```text
Tách riêng logic xử lý cốt lõi
        |
        v
Sử dụng dataset cục bộ có kích thước nhỏ
        |
        v
Xác thực phép chuyển đổi và kết quả
        |
        v
Sửa lỗi dữ liệu hoặc schema
        |
        v
Triển khai workload đã được xác thực lên AWS
        |
        v
Kiểm thử ở quy mô production
```

Cách tiếp cận này ngăn agent liên tục khởi chạy các cloud job tốn kém trong giai đoạn thử nghiệm ban đầu.

> **Điểm chính:** Phát triển ngoại tuyến rút ngắn vòng phản hồi cho workload dữ liệu và giảm những lần thực thi đám mây không cần thiết trong giai đoạn đầu.

## Kiểm thử hybrid với tài nguyên đám mây gọn nhẹ

Một số dịch vụ AWS không thể được mô phỏng hoàn toàn trong môi trường cục bộ.

Đối với những dịch vụ này, mục tiêu không phải là loại bỏ kiểm thử trên đám mây. Thay vào đó, kiến trúc cần làm cho việc xác thực trên đám mây trở nên nhỏ gọn, cô lập, tự động và có thể dự đoán.

Đối với hệ thống event-driven sử dụng [Amazon Simple Notification Service](https://aws.amazon.com/sns/) hoặc [Amazon Simple Queue Service](https://aws.amazon.com/sqs/), nhóm phát triển có thể định nghĩa các test stack tối thiểu bằng infrastructure as code.

Các công cụ phù hợp gồm:

- [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
- [AWS Cloud Development Kit](https://aws.amazon.com/cdk/)

AI agent chỉ cần triển khai những tài nguyên cần thiết cho một bài kiểm thử.

Ví dụ:

```text
AI agent
   |
   v
Triển khai một SQS queue tạm thời
   |
   v
Triển khai hoặc gọi thành phần cần kiểm thử
   |
   v
Gửi test message
   |
   v
Kiểm tra kết quả
   |
   v
Xóa các tài nguyên tạm thời
```

Agent có thể sử dụng AWS SDK để tương tác với dịch vụ thật và xác nhận ứng dụng hoạt động chính xác.

Các test stack tạm thời nên có:

- Quy ước đặt tên có thể dự đoán
- Phạm vi tài nguyên tối thiểu
- Quy trình cấp phát tự động
- Quy trình dọn dẹp tự động
- Quyền IAM bị giới hạn
- Giới hạn thời gian tồn tại
- Cơ chế kiểm soát chi phí

Mô hình này xem đám mây là một dependency phục vụ kiểm thử có kiểm soát, thay vì yêu cầu môi trường đầy đủ cho mọi vòng xác thực.

> **Điểm chính:** Kiểm thử hybrid giúp xác nhận sớm hành vi thực tế của dịch vụ AWS trong khi vẫn giữ việc sử dụng đám mây trong phạm vi tập trung và có kiểm soát.

## Preview environment và thiết kế contract-first

Kiểm thử cục bộ và tài nguyên đám mây gọn nhẹ rất hữu ích, nhưng kiểm thử end-to-end vẫn cần thiết khi nhiều dịch vụ tương tác với nhau.

Preview environment là một application stack ngắn hạn được tạo cho một tính năng, branch hoặc pull request cụ thể.

Vì môi trường được định nghĩa bằng infrastructure as code, AI agent có thể tạo và xóa môi trường một cách nhất quán.

Preview environment cho phép agent:

- Triển khai toàn bộ ứng dụng
- Cấu hình các dịch vụ phụ thuộc
- Xác thực giao tiếp giữa các dịch vụ
- Chạy integration test
- Chạy smoke test
- Kiểm tra log
- Thu thập kết quả kiểm thử
- Xóa môi trường sau khi xác thực

Một quy trình điển hình như sau:

```text
Thay đổi do AI tạo ra
        |
        v
Tạo feature branch
        |
        v
Triển khai preview environment
        |
        v
Chạy contract test, integration test và smoke test
        |
        +-- Kiểm thử thành công --> Tiếp tục review hoặc merge
        |
        +-- Kiểm thử thất bại --> Trả kết quả về cho agent
```

Preview environment đặc biệt hiệu quả khi kết hợp với phát triển contract-first.

Trong thiết kế contract-first, interface của dịch vụ được định nghĩa trước phần triển khai. Đối với API, OpenAPI specification có thể mô tả:

- Endpoint
- HTTP method
- Request parameter
- Request schema
- Response schema
- Yêu cầu xác thực
- Error response

AI agent có thể sử dụng contract để tạo hoặc xác thực client và service ngay cả khi toàn bộ thành phần chưa được triển khai hoàn chỉnh.

> **Điểm chính:** Preview environment giảm rủi ro tích hợp và cung cấp môi trường an toàn để xác thực thay đổi do AI tạo ra trước khi đưa lên production.

## Kiến trúc codebase thân thiện với AI

Kiến trúc hệ thống quyết định agent có thể kiểm thử thay đổi nhanh đến mức nào. Kiến trúc codebase quyết định agent có hiểu được những gì nó đang chỉnh sửa hay không.

Một repository thân thiện với AI nên cung cấp:

- Cách tổ chức có thể dự đoán
- Ranh giới kiến trúc rõ ràng
- Quy tắc dependency cụ thể
- Các bài kiểm thử nhanh và phù hợp
- Tài liệu máy có thể đọc
- Lệnh phát triển nhất quán

## Cấu trúc theo domain với ranh giới rõ ràng

Phát triển agentic hiệu quả hơn khi repository thể hiện rõ ý định kiến trúc.

Cấu trúc lấy cảm hứng từ Domain-Driven Design có thể tách logic nghiệp vụ cốt lõi khỏi phần điều phối ứng dụng và tích hợp hạ tầng.

Một dự án có thể sử dụng cấu trúc:

```text
src/
├── domain/
│   ├── entities/
│   ├── value-objects/
│   ├── services/
│   └── rules/
├── application/
│   ├── commands/
│   ├── queries/
│   ├── use-cases/
│   └── interfaces/
├── infrastructure/
│   ├── repositories/
│   ├── persistence/
│   ├── messaging/
│   └── aws/
└── interfaces/
    ├── api/
    ├── events/
    └── cli/
```

Layer `/domain` chứa quy tắc nghiệp vụ và không nên phụ thuộc trực tiếp vào dịch vụ AWS.

Layer `/application` điều phối các use case và định nghĩa interface mà domain cần.

Layer `/infrastructure` triển khai tích hợp với những hệ thống như:

- Amazon DynamoDB
- Amazon SNS
- Amazon SQS
- AWS Lambda
- API bên ngoài

Việc phân tách này cho phép AI agent chỉnh sửa và kiểm thử logic nghiệp vụ mà không cần cấp phát hạ tầng đám mây.

[Hexagonal architecture](https://docs.aws.amazon.com/prescriptive-guidance/latest/hexagonal-architectures/welcome.html) củng cố nguyên tắc này bằng cách xem database, queue, API và dịch vụ đám mây là các adapter bên ngoài.

Ví dụ, application logic có thể phụ thuộc vào một repository interface:

```typescript
export interface CustomerRepository {
  findById(customerId: string): Promise<Customer | null>;
  save(customer: Customer): Promise<void>;
}
```

Layer infrastructure có thể cung cấp phần triển khai bằng Amazon DynamoDB:

```typescript
export class DynamoDbCustomerRepository implements CustomerRepository {
  async findById(customerId: string): Promise<Customer | null> {
    // Triển khai DynamoDB
    return null;
  }

  async save(customer: Customer): Promise<void> {
    // Triển khai DynamoDB
  }
}
```

Trong unit test, agent có thể thay adapter này bằng một implementation lưu trữ trong bộ nhớ.

> **Điểm chính:** Ranh giới rõ ràng giúp giảm tác động ngoài ý muốn và làm cho những thay đổi do AI tạo ra dễ hiểu, dễ kiểm thử hơn.

## Mã hóa ý định kiến trúc bằng project rule

Chỉ riêng cấu trúc repository có thể chưa thể hiện đầy đủ mọi quyết định kiến trúc.

Một dự án có thể có những quy tắc như:

- Domain code không được import AWS SDK package.
- Việc truy cập database phải đi qua repository class.
- Lambda handler phải được giữ đơn giản.
- Public API phải xác thực input.
- Hạ tầng phải được định nghĩa bằng AWS CDK.
- Tính năng mới phải có automated test.
- IAM policy phải tuân theo nguyên tắc least privilege.

Những quy tắc này nên được viết dưới định dạng mà AI agent có thể tự động truy cập.

[Kiro](https://kiro.dev/) hỗ trợ [steering file](https://kiro.dev/docs/cli/steering/), là các tài liệu Markdown được lưu trong:

```text
.kiro/steering/
```

Một dự án có thể tổ chức steering file như sau:

```text
.kiro/
└── steering/
    ├── architecture.md
    ├── coding-standards.md
    ├── testing.md
    ├── security.md
    └── deployment.md
```

Ví dụ:

```markdown
# Quy tắc truy cập dữ liệu

- Domain code và application code không được gọi DynamoDB trực tiếp.
- Mọi thao tác database phải sử dụng repository interface.
- Repository implementation phải nằm trong `src/infrastructure/repositories/`.
- Unit test phải sử dụng in-memory repository implementation.
```

Agent có thể tham chiếu các quy tắc này khi thực hiện một tác vụ.

Cách tiếp cận này giúp giảm nhu cầu lặp lại cùng một ràng buộc trong mọi prompt và ngăn mã nguồn được tạo ra đi lệch khỏi kiến trúc đã định.

> **Điểm chính:** Project rule giúp giảm sự sai lệch kiến trúc và duy trì tính nhất quán khi AI agent được trao quyền tự chủ cao hơn.

## Kiểm thử như một đặc tả có thể thực thi

Trong phát triển agentic, kiểm thử không chỉ dùng để phát hiện regression. Chúng còn mô tả hành vi mà hệ thống phải cung cấp.

Một chiến lược kiểm thử nhiều lớp đặc biệt hiệu quả.

### Unit test

Unit test xác thực domain logic và application logic trong môi trường độc lập.

Unit test nên:

- Chạy nhanh
- Không truy cập mạng
- Không sử dụng tài nguyên AWS thật
- Tạo kết quả có thể dự đoán
- Chỉ rõ hành vi bị lỗi

Unit test nhanh cung cấp vòng phản hồi chính cho các thay đổi thường xuyên do AI tạo ra.

### Contract test

Contract test xác minh rằng các dịch vụ tuân theo interface đã thống nhất.

Chúng có thể kiểm tra:

- Định dạng API request
- Định dạng API response
- Event schema
- Trường bắt buộc
- Error code
- Khả năng tương thích giữa các dịch vụ

Contract test phát hiện breaking change trước khi nhiều dịch vụ được triển khai cùng nhau.

### Smoke test

Smoke test chạy trên môi trường đã triển khai và xác nhận các chức năng quan trọng hoạt động.

Chúng có thể kiểm tra ứng dụng có thể:

- Khởi động thành công
- Truy cập tài nguyên cần thiết
- Xử lý một request cơ bản
- Publish event
- Consume event
- Đọc dữ liệu
- Ghi dữ liệu

Smoke test có thể phát hiện những vấn đề chỉ xuất hiện tại runtime, bao gồm thiếu quyền [AWS Identity and Access Management](https://aws.amazon.com/iam/).

Một luồng xác thực hoàn chỉnh có thể là:

```text
Thay đổi mã nguồn
    |
    v
Unit test
    |
    v
Contract test
    |
    v
Triển khai preview environment
    |
    v
Smoke test và integration test
    |
    v
Phê duyệt production
```

Các bài kiểm thử cũng đóng vai trò là tài liệu mà máy có thể đọc.

Khi bài kiểm thử thất bại, agent có thể kiểm tra:

- Mô tả bài kiểm thử
- Kết quả mong đợi
- Kết quả thực tế
- Thông báo lỗi
- Stack trace

Agent có thể sử dụng thông tin đó để tinh chỉnh phần triển khai.

> **Điểm chính:** Kiểm thử cung cấp khả năng xác thực nhanh và khách quan cho mã nguồn do AI tạo ra, đồng thời giảm rủi ro của những lỗi tích hợp khó phát hiện.

## Monorepository và tài liệu máy có thể đọc

AI agent hoạt động hiệu quả hơn khi có thể truy cập ngữ cảnh rộng và nhất quán.

Monorepository đặt nhiều dịch vụ, thư viện dùng chung, contract và định nghĩa hạ tầng trong cùng một repository.

Ví dụ:

```text
repository/
├── services/
│   ├── customer-service/
│   ├── order-service/
│   └── notification-service/
├── packages/
│   ├── shared-domain/
│   ├── api-contracts/
│   └── testing-tools/
├── infrastructure/
├── docs/
└── .kiro/
```

Cấu trúc này cho phép agent:

- Điều hướng giữa các dịch vụ liên quan
- Phát hiện các mẫu dùng chung
- Cập nhật interface dùng chung
- Xác định dependency giữa nhiều dịch vụ
- Đánh giá tác động trên toàn hệ thống
- Chạy kiểm thử cho tất cả thành phần bị ảnh hưởng

Tài liệu nên ngắn gọn, có cấu trúc và dễ được cả con người lẫn agent diễn giải.

Các file hữu ích có thể bao gồm:

```text
AGENT.md
ARCHITECTURE.md
RUNBOOK.md
CONTRIBUTING.md
SECURITY.md
```

`AGENT.md` có thể mô tả:

- Kiến trúc hệ thống
- Các ranh giới quan trọng
- Những lệnh thường dùng
- Những thay đổi bị cấm
- Yêu cầu kiểm thử
- Yêu cầu triển khai

`RUNBOOK.md` có thể ghi lại:

- Các lỗi vận hành phổ biến
- Vị trí xem log
- Các bước khôi phục
- Quy trình rollback
- Quy trình escalation

`CONTRIBUTING.md` có thể định nghĩa:

- Quy ước branch
- Yêu cầu pull request
- Tiêu chuẩn chất lượng mã nguồn
- Các bài kiểm thử bắt buộc
- Quy trình review

Định dạng máy có thể đọc thường dễ được agent xử lý hơn tài liệu văn xuôi dài và thiếu cấu trúc.

Những định dạng hữu ích gồm:

- YAML
- JSON
- TOML
- OpenAPI
- JSON Schema
- Structured Markdown

Kiro cũng có thể sử dụng foundational steering document để tóm tắt cấu trúc dự án, lựa chọn công nghệ và yêu cầu sản phẩm.

> **Điểm chính:** Ngữ cảnh dùng chung có cấu trúc giúp cải thiện chất lượng thay đổi do AI tạo ra và giảm nhu cầu chỉnh sửa thủ công.

## Tích hợp agent an toàn vào pipeline triển khai

Khi AI agent có thêm nhiều khả năng, cơ chế quản trị vẫn rất cần thiết.

Những thay đổi do AI tạo ra phải vượt qua các biện pháp kiểm soát CI/CD trước khi được đưa lên production.

Các biện pháp bảo vệ được khuyến nghị gồm:

- Unit test bắt buộc
- Contract test
- Static analysis
- Dependency scanning
- Xác thực hạ tầng
- Automated review
- Branch protection
- Cơ chế phê duyệt bắt buộc
- Deployment policy
- Cơ chế rollback

Một quy trình triển khai có kiểm soát có thể như sau:

```text
AI agent tạo thay đổi
        |
        v
Tạo feature branch
        |
        v
Chạy xác thực cục bộ
        |
        v
Mở pull request
        |
        v
Thực hiện các bước kiểm tra CI tự động
        |
        v
Triển khai preview environment
        |
        v
Chạy smoke test và integration test
        |
        v
Con người phê duyệt thay đổi có tác động lớn
        |
        v
Triển khai production
```

Mức độ tự chủ của agent có thể được tăng dần.

Ban đầu, agent có thể chỉ được phép:

- Đề xuất thay đổi
- Chỉnh sửa feature branch
- Chạy kiểm thử cục bộ
- Mở pull request

Sau khi tổ chức có đủ độ tin cậy, agent có thể được phép:

- Triển khai preview environment
- Sửa các bài kiểm thử thất bại
- Cập nhật dependency có rủi ro thấp
- Merge thay đổi có rủi ro thấp sau khi xác thực
- Khởi tạo deployment có kiểm soát

Con người vẫn nên tham gia vào những quyết định có tác động lớn, đặc biệt là thay đổi liên quan đến:

- Quyền IAM
- Security policy
- Dữ liệu production
- Cấu hình mạng
- Compliance control
- Thao tác hạ tầng có tính phá hủy

Sự cân bằng này cho phép AI agent tăng tốc công việc phát triển thường xuyên mà không làm tăng rủi ro vận hành một cách không cần thiết.

## Kết luận

Phát triển Agentic AI thành công đòi hỏi nhiều hơn việc thêm một AI coding assistant vào quy trình phần mềm hiện tại.

Kiến trúc cần ưu tiên:

- Phản hồi nhanh
- Ranh giới rõ ràng
- Project rule cụ thể
- Môi trường có thể tái tạo
- Xác thực tự động
- Cơ chế triển khai an toàn

Mô phỏng cục bộ cung cấp cho AI agent vòng phản hồi nhanh nhất đối với logic ứng dụng.

Kiểm thử đám mây gọn nhẹ cho phép agent xác thực hành vi thật của dịch vụ AWS mà không phải liên tục triển khai môi trường hoàn chỉnh.

Preview environment cung cấp khả năng xác thực end-to-end trong môi trường cô lập trước khi thay đổi đến production.

Trong codebase, tổ chức theo domain, hexagonal architecture, kiểm thử nhiều lớp, monorepository và tài liệu máy có thể đọc cung cấp ngữ cảnh cần thiết để agent thực hiện những thay đổi đáng tin cậy.

Các công cụ như Kiro giúp kết nối quyết định kiến trúc của con người với quá trình thực thi tự chủ bằng cách cung cấp trực tiếp project rule và ràng buộc cho agent.

Khi kiến trúc hệ thống và kiến trúc codebase được thiết kế phù hợp với agentic workflow, AI agent có thể trở thành công cụ tăng cường năng suất thực sự. Agent có thể xử lý các vòng triển khai và tinh chỉnh nhanh chóng, trong khi nhóm phát triển tập trung vào kiến trúc, chiến lược sản phẩm, bảo mật và đổi mới.

Để tìm hiểu thêm về việc triển khai giải pháp agentic trên AWS, hãy truy cập [AWS Agentic AI](https://aws.amazon.com/ai/agentic-ai/).

## Về tác giả

### Alan Oberto Jimenez

Alan Oberto Jimenez là một Application Architect tập trung vào việc thiết kế ứng dụng cloud-native hiện đại và hệ thống phát triển được hỗ trợ bởi AI.

Công việc của ông bao gồm kiến trúc đám mây có khả năng mở rộng, workflow phát triển tự chủ, công nghệ agentic và những công cụ sử dụng AI để đơn giản hóa cũng như tăng tốc vòng đời phát triển phần mềm.