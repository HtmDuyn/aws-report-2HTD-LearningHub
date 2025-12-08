---
title: "Workshop"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# Workshop — LearningHub (2HTD)

#### Tổng quan

Workshop này đi kèm với proposal 2HTD-LearningHub và hướng dẫn học viên triển khai, kiểm thử các thành phần chính: frontend host trên Vercel, backend serverless (AWS Lambda + API Gateway), database quan hệ chạy SQL Server trên EC2 (private subnet) và media lưu trữ trên S3. Các bài lab tập trung vào truy cập hybrid an toàn, thiết kế mạng (VPC/subnets), quản trị EC2 bằng SSM, upload lên S3 bằng presigned URL, API Lambda và cấu hình Cognito.

Workshop được tổ chức thành các lab ngắn, có thể chạy trong môi trường test nhỏ (single VPC) hoặc môi trường mở rộng (cloud VPC + mô phỏng on-prem) để minh hoạ luồng mạng khác nhau và các cân nhắc về NAT/egress.

### Mục tiêu học viên

- Hiểu kiến trúc hybrid của LearningHub (Vercel + Lambda + EC2 + S3).
- Provision VPC, subnets và thiết kế NAT / VPC endpoints để kiểm soát chi phí egress.
- Triển khai EC2 Windows + SQL Server Express và quản trị qua AWS Systems Manager (SSM).
- Thiết lập S3 bucket và upload bằng presigned URL từ frontend.
- Triển khai Lambda (Node.js) và expose qua API Gateway (HTTP API).
- Cấu hình Amazon Cognito cho authentication và kiểm thử API có bảo vệ.

### Mục lục lab

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị & thiết lập môi trường](5.2-Prerequiste/)
3. [Truy cập S3 từ VPC (với VPC endpoints)](5.3-S3-vpc/)
4. [Truy cập S3 từ mô phỏng On-prem](5.4-S3-onprem/)
5. [VPC Endpoint Policies (tùy chọn)](5.5-Policy/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)

---

