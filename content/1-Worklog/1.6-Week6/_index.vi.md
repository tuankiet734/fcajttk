---
title: "Tuần 6"
date: 2026-05-25
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

# Nhật Ký Làm Việc: Trích Xuất Đặc Trưng – AWS Glue ETL, Thu Nạp Dữ Liệu Thô & Feature Engineering

> **Tuần 6 - Thứ Hai, ngày 25/05/2026:** Triển khai pipeline chuyển đổi dữ liệu tự động bằng AWS Glue. Tuần này bao gồm cấu hình hai ETL job tuần tự: job đầu tiên thu nạp bản ghi giao dịch thô từ production RDS, và job thứ hai tính toán các đặc trưng chuỗi thời gian thiết yếu cho độ chính xác mô hình dự báo nhu cầu.

---

### Mục Tiêu Học Tập Trong Tuần

- Hiểu vai trò của **AWS Glue** là dịch vụ ETL serverless được quản lý hoàn toàn trong ML pipeline.
- Cấu hình Glue job đầu tiên (**Thu nạp thô**): trích xuất giao dịch bán hàng thô từ `fashion-rds` và đưa vào vùng staging trong `training-db`.
- Cấu hình Glue job thứ hai (**Feature Engineering**): áp dụng chuyển đổi đặc trưng chuỗi thời gian gồm lag features, thống kê rolling window và số liệu tốc độ bán hàng.
- Xác thực đầu ra chuyển đổi trong `training-db` bằng cách query các bảng đặc trưng kết quả.
- Kiểm tra và giám sát các lần chạy Glue job trong AWS Glue Console (run log, thời gian, DPU metrics).

---

### Nhật Ký Hoạt Động Theo Ngày

| Ngày | Hoạt động | Ngày thực hiện |
|------|-----------|----------------|
| Thứ Hai | Nghiên cứu **Tổng quan ETL AWS Glue**: xem xét khái niệm Glue Data Catalog, runtime Spark-based job, Glue connection đến RDS qua JDBC và phân bổ chi phí DPU. | 25/05 |
| Thứ Ba | Tạo **Raw Ingestion Glue Job** (`de-fashion-rds-extract`): cấu hình kết nối JDBC đến `fashion-rds`, viết script PySpark đọc bảng `transactions` và ghi output vào schema staging trong `training-db`. | 26/05 |
| Thứ Tư | Viết và triển khai **Feature Engineering Glue Job** (`glue_feature_engineering.py`): xây dựng các chuyển đổi PySpark để tính rolling average 7 ngày và 30 ngày, lag values (`lag_1`, `lag_7`) và số liệu tốc độ hàng ngày theo SKU theo cửa hàng. | 27/05 |
| Thứ Năm | Thực thi cả hai Glue job tuần tự và giám sát tiến trình trong **Glue Console**: kiểm tra trạng thái job run, xem CloudWatch log tìm lỗi và xác nhận hoàn thành thành công cả hai giai đoạn. | 28/05 |
| Thứ Sáu | Kết nối `training-db` với `psql` và chạy query xác thực để xác nhận bảng `features` được điền đúng: kiểm tra số lượng row khớp và kiểm tra spot các giá trị lag/rolling feature. | 29/05 |

---

### Kết Quả Học Tập Chính

- **Kết nối JDBC Glue:** Cấu hình Glue connection đến RDS yêu cầu Security Group RDS phải cho phép inbound từ CIDR của **Glue Elastic Network Interface (ENI)**, không chỉ từ Security Group EC2.
- **Feature Engineering PySpark:** Tính toán lag và rolling window được triển khai bằng Spark Window function (`Window.partitionBy("store_id", "sku").orderBy("date")`), cho phép thực thi song song hiệu quả qua các phân vùng dữ liệu.
- **Nhận thức chi phí DPU:** Mỗi lần chạy Glue job tính phí theo phút mỗi DPU tiêu thụ. Đặt DPU tối thiểu là 2 cho ETL job nhẹ giảm chi phí đáng kể so với phân bổ mặc định.
- **Job Bookmark:** Bật Glue Job Bookmark cho phép xử lý tăng dần—chỉ các bản ghi mới được thêm kể từ lần chạy cuối được thu nạp—ngăn việc quét lại toàn bộ bảng ở mỗi lần thực thi.
- **CloudWatch Monitoring:** Stream log Glue job vào CloudWatch Log Group cung cấp khả năng debug thời gian thực; các lỗi phổ biến (JDBC driver class not found, schema mismatch) được hiển thị ngay trong log stream.

---

*Nguồn tài liệu chính: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
