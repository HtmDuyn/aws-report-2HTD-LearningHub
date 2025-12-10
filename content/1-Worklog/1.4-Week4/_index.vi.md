---
title: "Worklog Tuần 4"
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Hiểu quy trình Migration (di chuyển hệ thống lên AWS) và Disaster Recovery (khôi phục sau thảm họa).
* Thực hành dịch vụ AWS Database Migration Service (DMS) và Elastic Disaster Recovery (EDR).
* Biết cách sao lưu, phục hồi dữ liệu và tạo kế hoạch khẩn cấp cho hạ tầng.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                        | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu các khái niệm Migration (Lift & Shift, Replatform, Refactor) <br> - Giới thiệu công cụ AWS Database Migration Service (DMS)                                       | 29/09/2025   | 29/09/2025      | [AWS Journey](https://cloudjourney.awsstudygroup.com/) |
| 3   | - Thực hành tạo Replication Instance trong DMS <br> - Cấu hình nguồn dữ liệu (on-premise) và đích (RDS) <br> - Thực hiện di chuyển dữ liệu thử nghiệm                        | 30/09/2025   | 30/09/2025      | [AWS Journey](https://cloudjourney.awsstudygroup.com/) |
| 4   | - Giới thiệu Elastic Disaster Recovery (EDR) <br> - Học cách thiết lập replication server và recovery instance                                                               | 01/10/2025   | 01/10/2025      | [AWS Journey](https://cloudjourney.awsstudygroup.com/) |
| 5   | - Thực hành mô phỏng sự cố: tắt EC2 chính và khởi động instance khôi phục từ EDR <br> - Đánh giá thời gian khôi phục (RTO/RPO)                                                | 02/10/2025   | 02/10/2025      | [AWS Journey](https://cloudjourney.awsstudygroup.com/) |
| 6   | - Tạo kế hoạch Disaster Recovery cơ bản (backup, restore, failover) <br> - Viết tài liệu tóm tắt quy trình Migration + DR <br> - Tổng kết kiến thức tuần 4                      | 03/10/2025   | 03/10/2025      | [AWS Journey](https://cloudjourney.awsstudygroup.com/) |


### Kết quả đạt được tuần 4:

* Hiểu toàn bộ quy trình Migration ứng dụng hoặc cơ sở dữ liệu lên AWS.

* Tạo và cấu hình thành công DMS Replication Instance, thực hiện migration dữ liệu mẫu.

* Thiết lập được Elastic Disaster Recovery (EDR) để bảo vệ hệ thống trước sự cố.

* Mô phỏng thành công tình huống failover và khôi phục dịch vụ.

* Hoàn thành bản kế hoạch Disaster Recovery cơ bản, làm nền cho tuần 5 (Infrastructure as Code & Systems Manager).

---
