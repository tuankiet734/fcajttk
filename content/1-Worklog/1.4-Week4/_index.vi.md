---
title: "Tuần 4"
date: 2026-05-11
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

# Nhật Ký Làm Việc: Thiết Lập Mạng – CloudFront, ALB, EC2 Web Server, Target Group & Auto Scaling

> **Tuần 4 - Thứ Hai, ngày 11/05/2026:** Bắt đầu triển khai thực tế toàn bộ lớp hạ tầng mạng cho hệ thống Fashion Retail. Tuần này bao gồm cấu hình CloudFront CDN distribution, triển khai Application Load Balancer, đăng ký Target Group, khởi tạo EC2 web server và thiết lập Auto Scaling Group với chính sách mở rộng dựa trên health check.

---

### Mục Tiêu Học Tập Trong Tuần

- Cấu hình **Amazon CloudFront Distribution** trỏ đến External ALB làm origin chính.
- Triển khai **External Application Load Balancer** phân phối lưu lượng HTTP/HTTPS công khai.
- Khởi tạo **EC2 Web Server** (`Web-Application-Server`) sử dụng AMI tùy chỉnh với ứng dụng đã cài đặt sẵn.
- Đăng ký web server vào **Target Group** và gắn vào các quy tắc listener ALB.
- Cấu hình **Auto Scaling Group** (ASG) với dung lượng tối thiểu/tối đa và chính sách mở rộng target tracking.
- Kết nối tất cả các lớp mạng để đảm bảo luồng yêu cầu đầu cuối: CloudFront → ALB → Target Group → EC2 ASG.

---

### Nhật Ký Hoạt Động Theo Ngày

| Ngày | Hoạt động | Ngày thực hiện |
|------|-----------|----------------|
| Thứ Hai | Tạo **Amazon CloudFront Distribution**: cấu hình origin domain trỏ đến ALB DNS, vô hiệu hóa cache cho các path API động và bật chính sách HTTPS-only viewer. | 11/05 |
| Thứ Ba | Cấp phát **External Application Load Balancer** trong public subnet: cấu hình listener cổng 80 và 443, thiết lập SSL certificate từ ACM và bật access logging vào S3 bucket. | 12/05 |
| Thứ Tư | Khởi tạo **EC2 Web Server** (`Web-Application-Server`): chọn instance `t3.medium`, cấu hình mạng trong public subnet, gán Security Group công khai và áp dụng user-data bootstrap script. | 13/05 |
| Thứ Năm | Tạo và cấu hình **Target Group**: đăng ký EC2 web server, đặt đường dẫn health check `/health`, cấu hình healthy/unhealthy threshold và xác nhận trạng thái trong console. | 14/05 |
| Thứ Sáu | Cấu hình **Auto Scaling Group** cho tầng web: tạo launch template từ AMI đã kiểm tra, đặt desired=2, min=1, max=4 và gắn Target Tracking Scaling Policy dựa trên request count theo target (ngưỡng 500 RPS). | 15/05 |

---

### Kết Quả Học Tập Chính

- **Cấu hình CloudFront Origin:** Đặt ALB DNS làm CloudFront origin cho phép CDN định tuyến cache miss trực tiếp đến load balancer mà không lộ IP công khai của EC2.
- **Quy tắc Listener ALB:** Quy tắc redirect HTTP sang HTTPS đảm bảo toàn bộ lưu lượng người dùng được mã hóa ngay từ yêu cầu đầu tiên.
- **Health Check Target Group:** Cấu hình endpoint health check chính xác (`/health` trả về HTTP 200) là then chốt—đường dẫn sai sẽ khiến ASG liên tục vòng lặp thay thế instance.
- **Launch Template vs Launch Configuration:** Launch Template hỗ trợ versioning và mixed instance type (Spot + On-Demand), là cách tiếp cận được khuyến nghị cho triển khai ASG mới.
- **Kiểm tra kết nối liên dịch vụ:** Kiểm thử đầu cuối qua CloudFront URL và theo dõi định tuyến qua X-Ray xác nhận tất cả hop (CloudFront → ALB → Target Group → EC2) hoạt động đúng.

---

*Nguồn tài liệu chính: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
