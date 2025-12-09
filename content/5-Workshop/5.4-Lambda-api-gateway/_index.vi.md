---
title: "Lambda + API Gateway"
weight: 3
chapter: false
pre : " <b> 5.4. </b> "
---

## Tổng quan

Bài lab này hướng dẫn cách xây dựng và triển khai API HTTP dựa trên Lambda bằng Amazon API Gateway (HTTP API). Học viên sẽ tạo một hàm Lambda đơn giản, kết nối nó với route HTTP API, kiểm thử end-to-end và thêm quan sát (CloudWatch). Bài lab cũng đề cập tới quyền (IAM role), CORS và xử lý lỗi cơ bản.

## Yêu cầu trước

- Tài khoản AWS có quyền tạo Lambda, API Gateway, IAM role, CloudWatch.
- Truy cập AWS CLI hoặc Console.
- Node.js (nếu muốn chạy kiểm thử hàm cục bộ) hoặc dùng mã mẫu trong bài.

## Mục tiêu học viên

- Tạo và triển khai AWS Lambda (Node.js).
- Tạo API Gateway (HTTP API) và tích hợp Lambda.
- Cấu hình route, CORS và kiểm thử các phương thức HTTP.
- Thiết lập IAM role/quyền cho Lambda và API.
- Quan sát thực thi qua CloudWatch logs và metrics.

## Các bước lab (tóm tắt)

1. Tạo hàm Lambda
   - Dùng Console hoặc AWS CLI. Chọn runtime Node.js và handler tối giản trả về JSON.
   - Tạo hoặc gán execution role có policy `AWSLambdaBasicExecutionRole` (CloudWatch logs) và các quyền cần thiết khác.
2. Kiểm thử Lambda cục bộ (tùy chọn)
   - Dùng `sam local invoke` hoặc `node` để kiểm tra handler.
3. Tạo API Gateway (HTTP API)
   - Tạo HTTP API mới và thêm integration tới hàm Lambda.
   - Thêm route (ví dụ: `GET /status`, `POST /items`).
4. Cấu hình CORS
   - Bật CORS cho các route gọi từ trình duyệt và đặt origin/headers/methods cho phép.
5. Triển khai và kiểm thử
   - Triển khai API và gọi endpoint bằng `curl`, trình duyệt (GET) hoặc Postman/fetch (POST).
   - Xác nhận responses và mã trạng thái HTTP.
6. Quan sát
   - Kiểm tra CloudWatch Logs cho hàm Lambda để xem logs và lỗi.
   - Xem CloudWatch Metrics (invocations, errors, duration) để hiểu hiệu năng.
7. Xử lý lỗi và retry
   - Trả về mã HTTP thích hợp từ Lambda và phân biệt lỗi phía client và server.
8. Dọn dẹp
   - Xoá API, hàm Lambda và IAM role (nếu tạo riêng cho lab).

## Ví dụ Lambda tối giản (Node.js)
```js
exports.handler = async (event) => {
  return {
    statusCode: 200,
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ message: 'OK', input: event })
  };
};
```

Ví dụ kiểm thử

- curl GET:

  curl -i https://<api-id>.execute-api.<region>.amazonaws.com/status

- curl POST:

  curl -i -X POST https://<api-id>.execute-api.<region>.amazonaws.com/items -H "Content-Type: application/json" -d '{"name":"test"}'

Xử lý sự cố

- Nếu thấy 502/500, kiểm tra CloudWatch Logs của Lambda để tìm stack trace.
- Đảm bảo execution role của Lambda có đủ quyền ghi CloudWatch Logs và các dịch vụ AWS khác nếu sử dụng.
- Kiểm tra cấu hình CORS của API cho các gọi từ trình duyệt.

Tham khảo

- AWS Lambda Developer Guide
- Amazon API Gateway (HTTP API) Developer Guide
