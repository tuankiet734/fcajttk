---
title: "Tuần 5"
date: 2026-05-18
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

# Nhật Ký Làm Việc: Thiết Lập Cơ Sở Dữ Liệu – Business RDS, Training RDS, Bảo Mật & Kiểm Tra Kết Nối

> **Tuần 5 - Thứ Hai, ngày 18/05/2026:** Cấp phát và cấu hình hai instance Amazon RDS PostgreSQL phục vụ mục đích riêng biệt: cơ sở dữ liệu giao dịch production (`fashion-rds`) cho ứng dụng web trực tiếp và cơ sở dữ liệu huấn luyện chuyên dụng (`training-db`) cho ML pipeline. Tuần này cũng bao gồm quy tắc cô lập Security Group VPC và xác thực kết nối từ EC2 instance.

---

### Mục Tiêu Học Tập Trong Tuần

- Triển khai **Business Database** (`fashion-rds`): instance PostgreSQL RDS trung tâm lưu dữ liệu giao dịch trực tiếp (đơn hàng, khách hàng, sản phẩm, tồn kho).
- Triển khai **Training Database** (`training-db`): instance PostgreSQL RDS riêng biệt lưu các bảng đặc trưng ML đã xử lý từ AWS Glue ETL pipeline.
- Cấu hình **quy tắc Security Group** để thực thi kiểm soát truy cập mức mạng nghiêm ngặt giữa database và các tầng ứng dụng tương ứng.
- Xác thực **kết nối database** trực tiếp từ EC2 instance dùng `psql` CLI client.
- Hiểu sự khác biệt giữa cấu hình Multi-AZ standby và Read Replica về mục đích, hành vi failover và chi phí.

---

### Nhật Ký Hoạt Động Theo Ngày

| Ngày | Hoạt động | Ngày thực hiện |
|------|-----------|----------------|
| Thứ Hai | Triển khai **`fashion-rds`** (Business DB): chọn engine PostgreSQL 15.x, instance class `db.t3.medium`, cấu hình tên database `fashiondb`, master username `dbadmin`, và đặt instance trong private subnet group. | 18/05 |
| Thứ Ba | Triển khai **`training-db`** (Training DB): dùng phiên bản PostgreSQL và instance class tương tự nhưng tạo DB subnet group riêng trong private data tier subnet, đặt tên database `trainingdb`. | 19/05 |
| Thứ Tư | Cấu hình **quy tắc Security Group**: tạo `rds-fashion-sg` cho phép inbound cổng 5432 chỉ từ Security Group ID của ứng dụng EC2, và `rds-training-sg` chỉ cho phép từ Security Group của Glue job và ML-Forecast-Server. | 20/05 |
| Thứ Năm | Xác thực **kết nối từ EC2 đến `fashion-rds`**: SSH vào Web Application Server và chạy `psql -h <fashion-rds-endpoint> -U dbadmin -d fashiondb` để xác nhận kết nối và chạy query `SELECT version();`. | 21/05 |
| Thứ Sáu | Xác thực **kết nối từ EC2 đến `training-db`**: SSH vào ML-Forecast-Server và kiểm tra kết nối. Nạp dữ liệu seed vào `trainingdb` cho việc xác thực Glue ETL tiếp theo. | 22/05 |

---

### Kết Quả Học Tập Chính

- **Mẫu Dual-Database:** Tách database production khỏi database training ngăn các query spike từ ML workload làm giảm hiệu suất ứng dụng người dùng—nguyên tắc thiết kế then chốt cho hệ thống ML production.
- **DB Subnet Group:** RDS instance cần **DB Subnet Group** trải rộng ít nhất hai Availability Zone để hỗ trợ Multi-AZ failover; không cấu hình điều này dẫn đến lỗi cấp phát.
- **Security Group Chaining:** Thay vì cho phép cổng 5432 từ CIDR range, tham chiếu **Security Group ID** nguồn giới hạn truy cập database chỉ cho các instance được gán vào nhóm đó—cách tiếp cận bảo mật và có khả năng mở rộng cao hơn.
- **Kiểm tra kết nối `psql` CLI:** Xác nhận kết nối với `psql` trước khi triển khai code ứng dụng loại bỏ sự mơ hồ về nguồn gốc vấn đề kết nối.
- **Parameter Group:** Tùy chỉnh cài đặt PostgreSQL (ví dụ: `max_connections`, `shared_buffers`) qua RDS Parameter Group cho phép tinh chỉnh hiệu suất mà không cần truy cập trực tiếp OS server.

---

*Nguồn tài liệu chính: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
