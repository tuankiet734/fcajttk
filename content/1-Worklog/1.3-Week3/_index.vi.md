---
title: "Tuần 3"
date: 2026-05-04
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

# Nhật Ký Làm Việc: Phân Tích Kiến Trúc Hệ Thống & Lập Bản Thiết Kế Hạ Tầng Workshop

> **Tuần 3 - Thứ Hai, ngày 04/05/2026:** Tiến hành phân tích chuyên sâu kiến trúc hệ thống hai vùng cho dự án Fashion Retail & ML Forecasting. Tuần này cho ra bản thiết kế hạ tầng cuối cùng với đầy đủ phân công thành phần, cấu trúc mạng và luồng giao tiếp giữa các dịch vụ.

---

### Mục Tiêu Học Tập Trong Tuần

- Phân tích toàn bộ kiến trúc hệ thống qua hai vùng: **Nhóm Web App & API** và **Nhóm Data & ML Pipeline**.
- Hiểu vị trí của từng dịch vụ AWS: CloudFront → External ALB → Web ASG → Internal ALB → API ASG → RDS.
- Lập bản đồ luồng dữ liệu ML pipeline: RDS (`fashion-rds`) → AWS Glue ETL → Training DB → ML Server → S3 → Lambda API → EventBridge.
- Ghi lại trách nhiệm thành phần, thông số cấu hình và ranh giới truy cập mạng cho mỗi tài nguyên trước khi bắt đầu triển khai.

---

### Nhật Ký Hoạt Động Theo Ngày

| Ngày | Hoạt động | Ngày thực hiện |
|------|-----------|----------------|
| Thứ Hai | Nghiên cứu chuyên sâu **Lớp Web App & API**: xem xét cài đặt CloudFront distribution, origin groups và cache behaviors cho ALB origin. | 04/05 |
| Thứ Ba | Phân tích cấu hình **Auto Scaling Group** cho tầng Web App và tầng RESTful API, bao gồm cấu hình launch template, cài đặt dung lượng tối thiểu/tối đa/mong muốn. | 05/05 |
| Thứ Tư | Nghiên cứu các thành phần **Data & ML Pipeline**: xem xét cách các Glue Spark job thực hiện ETL từ production RDS sang training database. | 06/05 |
| Thứ Năm | Lập bản đồ **pipeline suy luận mô hình**: ML-Forecast-Server (EC2) huấn luyện mô hình LightGBM, lưu trữ lên S3, và Lambda API tải và phục vụ dự đoán. | 07/05 |
| Thứ Sáu | Hoàn thiện tài liệu bản thiết kế kiến trúc. Chuẩn bị danh sách dịch vụ AWS cần cấp phát, subnet VPC mục tiêu, quy tắc Security Group và yêu cầu IAM role. | 08/05 |

---

### Tóm Tắt Kiến Trúc: Hệ Thống Hai Vùng

**Vùng 1 – Lớp Truy Cập Web App & API:**
- **Amazon CloudFront:** Điểm vào CDN, lưu đệm tài nguyên tĩnh và định tuyến yêu cầu động đến External ALB.
- **External ALB:** Phân phối lưu lượng HTTP/HTTPS qua các EC2 Web App trong Auto Scaling Group.
- **Internal ALB:** Định tuyến bảo mật các lệnh gọi API từ Web App đến RESTful API Auto Scaling Group trong private subnet.

**Vùng 2 – Dữ Liệu & ML Pipeline:**
- **Amazon RDS (`fashion-rds`):** Cơ sở dữ liệu PostgreSQL lưu trữ dữ liệu giao dịch, đơn hàng, người dùng và tồn kho.
- **AWS Glue ETL:** Hai Spark job tự động trích xuất bản ghi giao dịch thô và kỹ thuật đặc trưng chuỗi thời gian.
- **Amazon RDS (`training-db`):** Instance PostgreSQL chuyên dụng lưu các bảng đặc trưng đã xử lý.
- **ML-Forecast-Server (EC2):** Instance tính toán chuyên dụng chạy script huấn luyện LightGBM.
- **Amazon S3 (`fashion-retail-model-storage`):** Lưu trữ bền vững các file artifact mô hình đã huấn luyện.
- **AWS Lambda + API Gateway:** Endpoint suy luận serverless tải mô hình từ S3 và trả về dự đoán.
- **Amazon EventBridge:** Trigger cron hàng ngày tự động làm mới toàn bộ pipeline.

---

### Kết Quả Học Tập Chính

- **Truy vết đầu cuối:** Mỗi yêu cầu đi vào hệ thống qua CloudFront đều có thể theo dõi qua các ALB, EC2 App Server, tầng API và đến lớp dữ liệu RDS.
- **Glue Spark Job:** AWS Glue cung cấp môi trường Spark được quản lý hoàn toàn, loại bỏ nhu cầu cấp phát và quản lý cluster Hadoop/Spark riêng cho khối lượng công việc ETL.
- **S3 làm Model Registry:** Dùng S3 làm kho artifact mô hình là giải pháp thay thế nhẹ cho MLflow server chuyên dụng.
- **EventBridge Scheduling:** Quy tắc cron EventBridge kích hoạt toàn bộ pipeline ETL + Training + Deployment hàng ngày mà không cần quản lý hạ tầng máy chủ.

---

*Nguồn tài liệu chính: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
