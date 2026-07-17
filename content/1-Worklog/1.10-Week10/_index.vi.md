---
title: "Tuần 10"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

# Nhật Ký Làm Việc: Triển Khai EC2 Production, Nginx Reverse Proxy, PM2 & Cài Đặt SSL

> **Tuần 10 - Thứ Hai, ngày 22/06/2026:** Triển khai ứng dụng Web Portal lên môi trường production cloud trên Amazon EC2. Tuần này bao gồm cấp phát EC2 production instance, cài đặt Node.js runtime stack, triển khai code từ GitHub, cấu hình PM2 quản lý process liên tục, thiết lập Nginx làm reverse proxy và cấp phát chứng chỉ SSL/TLS miễn phí với Certbot/Let's Encrypt.

---

### Mục Tiêu Học Tập Trong Tuần

- Khởi tạo **EC2 production instance** (`fashion-web-server`) với AMI, instance type, quy tắc Security Group và cấu hình storage phù hợp.
- Thiết lập **kết nối SSH** và cài đặt tất cả dependencies cơ sở cần thiết (Node.js 20.x, Nginx, PM2, AWS CLI, PostgreSQL client).
- Clone repository ứng dụng, cấu hình **file `.env` bảo mật** với endpoint RDS production, tên S3 bucket, API Gateway URL và JWT secret.
- Cấu hình **PM2** để khởi động ứng dụng Express.js khi boot và tự động khởi động lại khi crash.
- Cấu hình **Nginx** làm reverse proxy định tuyến lưu lượng port 80 đến ứng dụng Node.js local trên port 3000.
- Cấp phát **chứng chỉ SSL/HTTPS** dùng Certbot và Let's Encrypt, bật truy cập HTTPS bảo mật.

---

### Nhật Ký Hoạt Động Theo Ngày

| Ngày | Hoạt động | Ngày thực hiện |
|------|-----------|----------------|
| Thứ Hai | Khởi tạo **EC2 production instance**: chọn instance type `t3.large` (2 vCPU, 8 GiB RAM), AMI `Ubuntu Server 22.04 LTS`, tạo key pair `fashion-key.pem` và cấu hình Security Group `web-app-sg` với quy tắc inbound cho SSH (22), HTTP (80), HTTPS (443) và VPC internal cho Node.js (3000) và FastAPI (8000). Cấp phát 20 GiB gp3 SSD storage. | 22/06 |
| Thứ Ba | Kết nối SSH và cài đặt tất cả **dependencies cơ sở**: cập nhật system package, cài đặt Nginx, thêm Node.js 20.x repository và cài đặt Node, cài PM2 global (`npm install -g pm2`), cài AWS CLI và PostgreSQL client tools. | 23/06 |
| Thứ Tư | Clone repository vào `/home/ubuntu/app` và tạo **file `.env` production** bằng Nano: cấu hình `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD` cho `training-db`, `AWS_REGION`, `S3_BUCKET_NAME`, `API_GATEWAY_URL`, `JWT_SECRET` và `NODE_ENV=production`. Chạy `npm install --production` cài đặt dependencies. | 24/06 |
| Thứ Năm | Cấu hình **PM2**: khởi động ứng dụng Express.js với `pm2 start app.js --name "fashion-web" --env production`, xác nhận process hiển thị trạng thái `online` trong `pm2 list`, chạy `pm2 startup` đăng ký PM2 daemon với systemd và lưu process list với `pm2 save`. | 25/06 |
| Thứ Sáu | Cấu hình **Nginx reverse proxy**: tạo site config tại `/etc/nginx/sites-available/fashion-app` với `proxy_pass` đến `http://localhost:3000`, kích hoạt qua symlink, kiểm tra cú pháp với `nginx -t` và restart Nginx. Cấp phát **chứng chỉ SSL** với `certbot --nginx -d your-domain.com` và xác minh tự động gia hạn với `certbot renew --dry-run`. | 26/06 |

---

### Kết Quả Học Tập Chính

- **t3.large cho Workload Web Portal:** Portal render biểu đồ Plotly.js và bản đồ 3D Mapbox GL JS phía server; các instance `t3.micro` không đủ (lỗi OOM), khiến `t3.large` (8 GiB RAM) là class instance tối thiểu khả thi.
- **PM2 Startup Integration:** Chạy `pm2 startup` tạo lệnh `sudo env PATH=...` phải được sao chép và thực thi thủ công để đăng ký PM2 với systemd. Thiếu bước này, ứng dụng sẽ không khởi động lại sau khi EC2 reboot.
- **Nginx Proxy Buffering:** Đặt `proxy_read_timeout 90` trong config Nginx ngăn connection drop trong các yêu cầu fetch dữ liệu Plotly có độ trễ cao (Lambda cold start có thể thêm 3–5 giây vào lần inference đầu tiên).
- **Let's Encrypt Auto-Renewal:** Certbot cài đặt systemd timer (`certbot.timer`) tự động gia hạn chứng chỉ trước khi hết hạn 90 ngày. Flag `--dry-run` mô phỏng quá trình gia hạn mà không tiêu thụ API rate limit.
- **Thứ tự phụ thuộc Security Group:** Quy tắc EC2 Security Group phải được cấu hình trước khi khởi tạo instance. Chỉnh sửa Security Group sau khi triển khai yêu cầu xác minh các process Nginx đang chạy phản ánh quy tắc tường lửa đã cập nhật.

---

*Nguồn tài liệu chính: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
