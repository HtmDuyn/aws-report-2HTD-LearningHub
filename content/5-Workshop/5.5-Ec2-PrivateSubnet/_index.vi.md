---
title: "EC2 (Private Subnet) — Windows + SQL Server"
weight: 3
chapter: false
pre : " <b> 5.5. </b> "
---

## Tổng quan

Bài lab này hướng dẫn việc triển khai và quản trị một instance EC2 Windows chạy SQL Server Express trong private subnet. Trọng tâm là quản trị an toàn bằng AWS Systems Manager (SSM), các cân nhắc mạng (private subnet, route tables, NAT, VPC endpoints) và backup (EBS snapshots). Lab minh hoạ cách Lambda hoặc các dịch vụ nội bộ truy cập database qua mạng riêng mà không phơi bày database ra Internet.

## Yêu cầu trước

- Tài khoản AWS có quyền tạo VPC, EC2, IAM role, Systems Manager và EBS snapshots.
- Am hiểu cơ bản AWS Console hoặc AWS CLI.
- Có VPC sẵn cho lab hoặc quyền tạo VPC mới.

## Mục tiêu học viên

- Triển khai EC2 Windows vào private subnet kèm instance profile IAM cho SSM.
- Cài đặt và cấu hình SQL Server Express trên instance.
- Quản trị an toàn qua AWS Systems Manager (Session Manager / Run Command) mà không mở RDP ra Internet.
- Cấu hình security groups và routing để backend Lambda hoặc host nội bộ có thể truy cập database trong khi database vẫn giữ private.
- Thực hiện EBS snapshot để backup và kiểm thử phục hồi.

## Các bước lab (tóm tắt)

1. Chuẩn bị mạng
   - Kiểm tra hoặc tạo VPC có public và private subnets, route tables phù hợp và chiến lược NAT/egress cho instance private.
   - (Tuỳ chọn) Thêm VPC endpoints cho S3/SSM để giảm egress.
2. Tạo IAM role cho EC2
   - Tạo instance profile role cấp `AmazonSSMManagedInstanceCore` và các quyền tối thiểu cần thiết (EBS snapshot nếu cần).
3. Khởi tạo EC2 (Windows) trong private subnet
   - Chọn Windows AMI và gán instance profile.
   - Dùng security group chỉ cho phép traffic cần thiết từ CIDR nội bộ (ví dụ port SQL 1433 từ subnet ứng dụng).
4. Cài SQL Server Express
   - Dùng SSM `Run Command` để tải và cài đặt SQL Server Express silent, hoặc dùng SSM Session Manager để truy cập và cài đặt tương tác.
   - Cấu hình SQL Server lắng nghe trên IP riêng và bật authentication phù hợp.
5. Kiểm thử kết nối từ client nội bộ
   - Từ Lambda hoặc bastion trong cùng VPC, kiểm tra kết nối TCP tới port SQL và chạy truy vấn kiểm thử.
6. Quản trị qua SSM
   - Dùng Session Manager cho quản trị tương tác hoặc port-forwarding để tránh mở RDP.
   - Dùng SSM Run Command để tự động hóa (patch, cài đặt, cấu hình).
7. Backup & snapshot
   - Tạo EBS snapshot sau khi cấu hình xong; minh hoạ phục hồi sang volume/instance mới.
8. Dọn dẹp
   - Terminate instance, xóa snapshots, xóa IAM role và các tài nguyên tạm khác.

## Kiểm thử & xử lý sự cố

- Nếu SSM không kết nối, kiểm tra agent SSM đã cài và role `AmazonSSMManagedInstanceCore`.
- Kiểm tra Security Group: đảm bảo port SQL chỉ mở cho subnet nội bộ, không mở ra public.
- Nếu instance cần cập nhật OS nhưng không có outbound Internet, đảm bảo NAT hoặc SSM + VPC endpoints cho patching.
- Kiểm tra log CloudWatch (SSM / EC2) để tìm lỗi cài đặt hoặc chạy.

## Ghi chú & best practices

- Ưu tiên dùng SSM Session Manager thay vì mở RDP ra Internet; dùng SSM Port Forwarding khi cần GUI.
- Áp dụng nguyên tắc least-privilege cho IAM roles của EC2 và Lambda.
- Sử dụng VPC endpoints cho S3/SSM để giảm egress và tăng bảo mật.
- Tự động hoá backup với EBS snapshot theo lịch và cân nhắc lưu trữ bản sao DB vào S3.

## Tham khảo

- AWS Systems Manager Session Manager
- Amazon EC2 Windows AMIs
- Hướng dẫn cài SQL Server Express
