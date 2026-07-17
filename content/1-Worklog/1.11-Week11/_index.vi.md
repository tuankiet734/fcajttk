---
title: "Tuần 11"
date: 2026-06-29
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

# Nhật Ký Làm Việc: Kiểm Thử Đầu Cuối, Dọn Dẹp Tài Nguyên & Video Demo Dự Án

> **Tuần 11 - Thứ Hai, ngày 29/06/2026:** Thực hiện kiểm thử tích hợp đầu cuối toàn diện cho hệ thống Fashion Retail đã triển khai hoàn chỉnh, xác thực quyền vai trò người dùng, độ chính xác dự báo ML, logic kích hoạt cảnh báo tồn kho và khả năng phục hồi hệ thống. Tuần này cũng hoàn thành việc ngừng hoạt động có hệ thống tất cả tài nguyên AWS đã cấp phát để loại bỏ phí liên tục, và sản xuất video demo dự án ghi lại hệ thống trực tiếp đang vận hành.

---

### Mục Tiêu Học Tập Trong Tuần

- Thực thi **kiểm thử truy cập theo vai trò đầu cuối**: xác thực mỗi trong 7 vai trò người dùng trải nghiệm đúng dashboard view, hạn chế tab và quyền thay đổi dữ liệu.
- Xác minh **ML inference pipeline**: xác nhận click cửa hàng trên bản đồ Mapbox lấy dự báo hợp lệ từ Lambda API và render biểu đồ Plotly chính xác.
- Xác thực **logic cảnh báo thiếu hàng tồn kho**: xác nhận banner cảnh báo đỏ xuất hiện khi nhu cầu dự báo vượt mức tồn kho kho hàng hiện tại.
- Thực thi **trình tự dọn dẹp tài nguyên** theo đúng thứ tự phụ thuộc để terminate tất cả tài nguyên AWS mà không để lại phí orphaned.
- Ghi hình và xuất bản **video demo dự án** giới thiệu tất cả tính năng hoàn chỉnh của Fashion Retail Portal.

---

### Nhật Ký Hoạt Động Theo Ngày

| Ngày | Hoạt động | Ngày thực hiện |
|------|-----------|----------------|
| Thứ Hai | Thực hiện **kiểm thử truy cập theo vai trò**: đăng nhập với Sales Staff (xác nhận sidebar hạn chế chỉ *Transactions* và *Products*), Store Manager (xác nhận quyền chỉnh sửa *Discounts* và *Employees* trong phạm vi cửa hàng được giao) và Director (xác nhận toàn quyền truy cập Dashboard với tất cả tab quản lý). | 29/06 |
| Thứ Ba | Xác thực **ML forecast pipeline**: click nhiều store marker trên bản đồ Mapbox, xác nhận Lambda API `/predict` trả về status 200 với payload nhu cầu hợp lệ, xác minh biểu đồ Plotly.js Actual vs. Forecasted render đúng. Kích hoạt EventBridge cron rule thủ công và xác nhận toàn chu trình ETL → Training → S3 upload hoàn thành thành công. | 30/06 |
| Thứ Tư | Kiểm tra **cảnh báo thiếu hàng tồn kho**: giảm nhân tạo mức tồn kho trong database xuống dưới nhu cầu dự báo tuần tới cho các SKU được chọn, xác nhận banner thiếu hàng đỏ xuất hiện trong portal UI và đặt lại mức tồn kho. Xác minh luồng đăng nhập MFA với Google Authenticator trên thiết bị thực. | 01/07 |
| Thứ Năm | Thực thi **trình tự dọn dẹp tài nguyên**: xóa Lambda functions và API Gateway stages, xóa Glue jobs và Data Catalog tables, terminate EC2 instances, xóa Auto Scaling Groups và Launch Templates, xóa ALB và Target Groups, xóa CloudFront distribution, terminate cả hai RDS instance (chờ trạng thái `deleted` trước khi xóa DB subnet group). Xóa sạch và xóa S3 bucket. | 02/07 |
| Thứ Sáu | Ghi hình **video demo dự án**: capture toàn bộ walkthrough hệ thống trực tiếp bao gồm luồng đăng nhập với 2FA, bản đồ cửa hàng Mapbox, biểu đồ dự báo LightGBM, demo chuyển đổi vai trò RBAC, kích hoạt cảnh báo thiếu hàng và trình chuyển đổi đa ngôn ngữ (Tiếng Việt, Tiếng Anh, Tiếng Trung). Tải video lên Google Drive và nhúng link vào tài liệu. | 03/07 |

---

### Kết Quả Học Tập Chính

- **Phương pháp kiểm thử quyền vai trò:** Kiểm thử từng vai trò dựa trên checklist ma trận quyền được định nghĩa sẵn (kiểm tra tab hiển thị, route API có thể truy cập và khả năng thay đổi dữ liệu) là phương pháp duy nhất đáng tin cậy để xác nhận tính đúng đắn RBAC; kiểm tra trực quan đơn thuần là không đủ.
- **Thứ tự phụ thuộc dọn dẹp tài nguyên:** Tài nguyên AWS có ràng buộc phụ thuộc xóa nghiêm ngặt. Trình tự cleanup đúng: Lambda → API Gateway → Glue → EC2 → ASG → ALB → CloudFront → RDS → DB Subnet Group → Security Group → VPC. Sai thứ tự gây lỗi `DependencyViolation`.
- **Kích hoạt EventBridge thủ công để kiểm thử pipeline:** Tính năng "Send test event" trong console EventBridge kích hoạt cron rule ngay lập tức thay vì chờ thời gian đã lên lịch, cho phép xác thực pipeline nhanh chóng.
- **Video Demo làm bằng chứng dự án:** Video demo được ghi lại cung cấp cho các bên liên quan bản ghi chức năng hệ thống có timestamp, có thể tái tạo khi hoàn thành dự án—đặc biệt quan trọng cho deliverable workshop khi môi trường trực tiếp sẽ bị ngừng hoạt động.
- **Điều kiện tiên quyết xóa S3 Bucket:** S3 bucket không thể xóa khi còn chứa object. Chạy `aws s3 rm s3://bucket-name --recursive` trước `aws s3 rb s3://bucket-name` đảm bảo trình tự xóa sạch không lỗi.

---

*Nguồn tài liệu chính: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
