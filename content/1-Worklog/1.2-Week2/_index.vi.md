---
title: "Tuần 2"
date: 2026-04-27
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

# Nhật Ký Làm Việc: Lập Kế Hoạch Workshop, Thiết Kế Kiến Trúc & Lý Thuyết Nền Tảng AWS

> **Tuần 2 - Thứ Hai, ngày 27/04/2026:** Chuyển từ các bài thực hành riêng lẻ sang giai đoạn nghiên cứu và lập kế hoạch cho dự án workshop tổng hợp. Tuần này tập trung vào việc tìm hiểu các mẫu kiến trúc AWS cốt lõi, nghiên cứu các dịch vụ cần thiết và phác thảo bản thiết kế hệ thống.

---

### Mục Tiêu Học Tập Trong Tuần

- Tìm hiểu kiến trúc tổng thể của Web App bán lẻ thời trang tích hợp ML Pipeline sẽ được xây dựng trong workshop.
- Nghiên cứu mục đích và khả năng của từng dịch vụ AWS liên quan: CloudFront, ALB, EC2, RDS, Glue, Lambda, S3 và EventBridge.
- Hiểu chiến lược phân vùng mạng (public subnet, private subnet, CIDR VPC).
- Phác thảo bản thiết kế hạ tầng ban đầu và xác định các phụ thuộc giữa các dịch vụ.

---

### Nhật Ký Hoạt Động Theo Ngày

| Ngày | Hoạt động | Ngày thực hiện |
|------|-----------|----------------|
| Thứ Hai | Nghiên cứu tài liệu tổng quan workshop. Xác định hai vùng hệ thống chính: **Lớp Web App & API** và **ML Pipeline**. | 27/04 |
| Thứ Ba | Nghiên cứu cơ chế bộ đệm CDN của Amazon CloudFront và cách tích hợp với Application Load Balancer làm điểm gốc. | 28/04 |
| Thứ Tư | Tìm hiểu launch template Auto Scaling Group, chính sách mở rộng (target tracking) và thời gian grace period trong mối quan hệ với EC2 sau ALB. | 29/04 |
| Thứ Năm | Xem xét kiến trúc Amazon RDS PostgreSQL (primary instance, Multi-AZ, read replica) và thiết kế quy tắc Security Group cho việc cô lập truy cập cơ sở dữ liệu. | 30/04 |
| Thứ Sáu | Tạo bản thảo đầu tiên của sơ đồ kiến trúc, liệt kê tất cả thành phần, mũi tên luồng dữ liệu và phân công subnet cho hạ tầng workshop. | 01/05 |

---

### Kết Quả Học Tập Chính

- **Phân vùng kiến trúc:** Hiểu lý do tách biệt lớp web người dùng và ML pipeline là thiết yếu để cô lập bảo mật và mở rộng độc lập.
- **Chiến lược CDN:** CloudFront giảm tải cho origin bằng cách lưu đệm tài nguyên tĩnh (HTML, CSS, JS, ảnh sản phẩm) tại các điểm edge gần người dùng.
- **Lợi ích Auto Scaling:** EC2 Auto Scaling Group tự động điều chỉnh dung lượng dựa trên số liệu nhu cầu thực tế (CPU utilization, request count), loại bỏ can thiệp thủ công.
- **Cô lập RDS:** Database instance phải nằm trong private subnet, với quy tắc Security Group chỉ cho phép traffic port 5432 từ Security Group ID của tầng ứng dụng - không phải từ dải IP công khai.

---

*Nguồn tài liệu chính: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
