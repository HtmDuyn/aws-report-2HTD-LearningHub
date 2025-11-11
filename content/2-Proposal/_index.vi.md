---
title: "Đề xuất nền tảng học trực tuyến — Ôn luyện & Thi thử"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## 1. Tóm tắt dự án

Mục tiêu của dự án là xây dựng một nền tảng web cho học tập, ôn luyện, tạo đề thi và tổ chức lớp học trực tiếp (live class). Ứng dụng phục vụ học sinh/sinh viên và giáo viên bằng các tính năng: ngân hàng đề (create/modify), bài tập thực hành, mock exams, lớp học trực tiếp với Chime SDK, và báo cáo tiến độ học tập.

Nền tảng được thiết kế theo mô hình serverless/hybrid (dựa trên sơ đồ kiến trúc đính kèm) nhằm tối ưu chi phí, khả năng mở rộng và giảm công tác vận hành. Tài nguyên tĩnh được phân phối qua CloudFront/S3, API và business logic xử lý bởi API Gateway / Lambda / AppSync, dữ liệu lưu trữ trong DynamoDB, và Cognito quản lý xác thực/ủy quyền.

Hệ thống hỗ trợ luồng sự kiện (EventBridge + SNS) cho thông báo (ví dụ thông báo lịch thi, kết quả), cùng CloudWatch để giám sát logs và metrics. Chime SDK tích hợp cho chức năng live class/recording, AppSync hoặc API Gateway cho realtime/GraphQL nếu cần.

Thiết kế chú trọng bảo mật: phân tách public/private, VPC endpoints cho DynamoDB, IAM least-privilege, và lưu trữ bí mật an toàn (Secrets Manager / Parameter Store).

## 2. Bài toán cần giải quyết

- Người dùng cần một nền tảng tích hợp để ôn luyện, làm đề và tham gia lớp học trực tiếp thay vì sử dụng nhiều công cụ rời rạc.
- Quản trị viên/giáo viên gặp khó trong việc tạo/duyệt ngân hàng đề và phân phối bài kiểm tra có kiểm soát.
- Khó đảm bảo hiệu suất khi số lượng user tăng đột biến trong giờ thi thử hoặc live class.
- Thiếu hệ thống thông báo & báo cáo thời gian thực cho học sinh và giáo viên.
- Yêu cầu bảo mật dữ liệu người dùng và bài làm (che giấu đáp án, bảo vệ tài khoản).

## 3. Giải pháp & Kiến trúc

Giải pháp đề xuất sử dụng các thành phần chính theo sơ đồ kiến trúc trong thư mục `images`:

- Edge: Route 53 + CloudFront với origin là S3 để phục vụ static assets (landing page, tài liệu, media cached).
- Authentication: Amazon Cognito cho đăng nhập/đăng ký, quản lý session và MFA khi cần.
- API layer: Amazon API Gateway (REST) và/hoặc AWS AppSync (GraphQL) làm gateway cho frontend; AWS Lambda xử lý business logic.
- Data layer: Amazon DynamoDB cho lưu trữ các thực thể (users, exams, questions, results) với VPC Endpoints nếu cần bảo mật.
- Real-time / Live class: Amazon Chime SDK để tổ chức lớp học trực tiếp, ghi hình và interaction.
- Events & Notifications: Amazon EventBridge + Amazon SNS để xử lý workflow (lên lịch exam, thông báo kết quả, reminders).
- Observability: Amazon CloudWatch (logs, metrics, dashboards) và EventBridge để chuyển log/metrics tới SNS hoặc các hệ thống external.

### Sơ đồ kiến trúc

![Architecture Diagram](/images/architecture_2HTD_LearningHub.png)

### AWS Services Used

- Amazon Route 53
- Amazon CloudFront
- Amazon S3
- Amazon Cognito
- Amazon API Gateway
- AWS AppSync (optional)
- AWS Lambda
- Amazon DynamoDB (VPC Endpoints)
- Amazon Chime SDK
- Amazon EventBridge
- Amazon SNS
- Amazon CloudWatch
- AWS IAM, AWS Secrets Manager / SSM Parameter Store

### Component Design

- Edge Layer: CloudFront + S3 cho static content và CDN caching.
- Auth & Identity: Cognito User Pools + Identity Pools cho access control và federated login.
- API & Business Logic: API Gateway / AppSync -> Lambda. Lambdas được triển khai trong private subnets nếu cần gọi tài nguyên nội bộ, hoặc sử dụng VPC endpoints cho DynamoDB.
- Data Model: DynamoDB với thiết kế partition key / sort key cho exams, questions, user-results; Global Secondary Indexes cho truy vấn nhanh.
- Live Class: Sử dụng Chime SDK serverless components cho phiên họp, recording và streaming tới S3.
- Events: EventBridge rules kích hoạt SNS notifications và Lambda workflows (grading, reminders).
- Monitoring & Logging: CloudWatch Logs + Dashboards, cấu hình alarms cho error rates, latency, concurrent connections.

## 4. Technical Implementation

1. IaC & Provisioning
    - Viết Terraform / AWS CloudFormation / AWS CDK để provision VPC, CloudFront, S3, Cognito, API Gateway/AppSync, Lambda, DynamoDB, EventBridge, SNS, CloudWatch, và Chime resources.
2. CI/CD
    - Thiết lập pipeline (GitHub Actions / CodePipeline) để build frontend (static) -> deploy S3/CloudFront; build & deploy Lambda functions (zip or container) và IaC changes.
3. API & Data
    - Thiết kế schema GraphQL (nếu dùng AppSync) hoặc REST endpoints; implement Lambda handlers cho create/edit exams, submit answers, grading.
    - DynamoDB table design: exams, questions, user_submissions, results; TTL cho dữ liệu tạm thời.
4. Live class
    - Tích hợp Chime SDK cho phiên họp, cấp quyền cho hosts (giáo viên) & attendees (student); lưu recordings vào S3.
5. Security
    - IAM roles with least privilege, Cognito user pool policies, encrypt-at-rest (DynamoDB encryption) và in-transit (HTTPS), secrets in Secrets Manager.
6. Observability & Testing
    - CloudWatch dashboards, structured logs, X-Ray tracing (optional). Load testing for peak exam times, automated integration tests.

## 5. Timeline & Milestones

- Week 1-2: Detailed design, data model, IaC scaffold
- Week 3-4: Implement authentication, static site (S3 + CloudFront), basic API endpoints
- Week 5-6: Implement exam creation, question bank, submission & grading workflows
- Week 7: Integrate Chime SDK and test live class flows
- Week 8: Observability, load testing, security review, documentation and handover

## 6. Budget Estimation

Lưu ý: con số ước tính mang tính tham khảo, tùy vào usage và scale; khuyến nghị dùng AWS Pricing Calculator.

- S3 + CloudFront: $20–$150 / tháng (phụ thuộc traffic and egress)
- Cognito: $0–$50 / tháng (tùy số người dùng hoạt động)
- API Gateway / AppSync + Lambda: $50–$300 / tháng
- DynamoDB: $50–$500 / tháng (phụ thuộc read/write capacity / on-demand)
- Chime SDK: $50–$500+ (tùy số phút meeting & recording storage)
- CloudWatch + Logs: $10–$200 / tháng
- SNS / EventBridge: $5–$50

Tổng ước tính sơ bộ: $200–$1,500 / tháng (phạm vi rộng để phản ánh các phương án cấu hình khác nhau).

## 7. Risk Assessment

- Chi phí tăng cao khi lưu lượng live-class hoặc lưu lượng download tăng đột biến — mitigation: autoscaling, on-demand DynamoDB, cost alarms.
- Vấn đề bảo mật (lộ đáp án hoặc tài khoản): mitigation: thiết kế RBAC, session control, encrypt data, security review & pen-testing.
- Performance bottlenecks during peak exam times: mitigation: caching (CloudFront), DynamoDB on-demand/warm-up, provisioned concurrency for critical Lambda.
- Chime/third-party dependency risk: mitigation: fallback workflows (recording available later), test alternate streaming options.

## 8. Expected Outcomes

- Trải nghiệm người dùng tích hợp: học, luyện, thi thử và tham gia lớp trực tiếp trên cùng một nền tảng.
- Giảm thời gian soạn đề và quản lý đề thi cho giáo viên (tăng năng suất).
- Khả năng phục vụ giờ thi thử đồng thời với availability cao và latency thấp.
- Hệ thống cảnh báo & báo cáo giúp cải thiện kết quả học tập qua phân tích dữ liệu.
- Kiểm soát chi phí và khả năng mở rộng linh hoạt theo nhu cầu.

---

Ghi chú: Tôi đã cập nhật nội dung trực tiếp trong `content/2-Proposal/_index.vi.md`. Nếu bạn muốn tôi tinh chỉnh độ chi tiết (ví dụ thêm sơ đồ bảng dữ liệu DynamoDB, policies IAM mẫu, hoặc ước tính chi phí chi tiết theo hiện trạng), tôi sẽ tiếp tục.