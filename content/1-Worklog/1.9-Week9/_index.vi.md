---
title: "Tuần 9"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

# Nhật Ký Làm Việc: Phát Triển Web Portal – RBAC, JWT Auth, Bản Đồ Tương Tác & Dashboard LightGBM

> **Tuần 9 - Thứ Hai, ngày 15/06/2026:** Phát triển cổng quản lý doanh nghiệp full-stack (**Global Fashion Retail Portal**) bằng Node.js và Express.js. Tuần này bao gồm triển khai xác thực đa lớp (JWT + TOTP 2FA), thiết kế và thực thi ma trận quyền RBAC 7 vai trò, tích hợp Mapbox GL JS để trực quan hóa cửa hàng toàn cầu và kết nối dashboard dự báo Plotly.js với AWS Lambda inference API.

---

### Mục Tiêu Học Tập Trong Tuần

- Triển khai **Data Access Layer đa chế độ** (`db.js`) hỗ trợ REST API mode, kết nối PostgreSQL trực tiếp và fallback Mock JSON cục bộ.
- Xây dựng **quản lý session dựa trên JWT token**: phát token đã ký 8 giờ khi đăng nhập, xác thực tại các route được bảo vệ qua middleware.
- Triển khai **Xác thực Hai Yếu Tố TOTP (2FA)** dùng JavaScript thuần HMAC-SHA1 và Base32 mà không cần thư viện MFA bên ngoài.
- Thiết kế và thực thi **ma trận quyền RBAC 7 vai trò** kiểm soát khả năng hiển thị tab dashboard và quyền thay đổi dữ liệu theo vai trò kinh doanh.
- Tích hợp **Mapbox GL JS** để render bản đồ cửa hàng 3D toàn cầu và kết nối sự kiện click marker để kích hoạt render biểu đồ dự báo **Plotly.js** qua Lambda API.

---

### Nhật Ký Hoạt Động Theo Ngày

| Ngày | Hoạt động | Ngày thực hiện |
|------|-----------|----------------|
| Thứ Hai | Xây dựng **Data Access Layer đa chế độ** (`db.js`): triển khai logic routing ba chế độ (API mode qua `API_BASE_URL`, Direct PostgreSQL Pool mode qua `DB_HOST`, Mock JSON fallback), kiểm tra cả ba chế độ và xác nhận API mode xác thực thành công với `x-api-key` header. | 15/06 |
| Thứ Ba | Triển khai **JWT Authentication**: viết route POST `/login` phát `jsonwebtoken`-signed token với thời hạn 8 giờ chứa claim `{ username, role, store_id }`, tạo Express middleware (`authMiddleware.js`) xác thực Bearer token ở mỗi API call được bảo vệ. | 16/06 |
| Thứ Tư | Triển khai **TOTP 2FA** (MFA): phát triển JavaScript thuần HMAC-SHA1 với Base32 decoding để tạo và xác thực OTP 6 chữ số luân phiên mỗi 30 giây. Tích hợp tạo QR code `otpauth://totp/` tương thích Google Authenticator. Thêm event logging MFA với ghi lại IP. | 17/06 |
| Thứ Năm | Thiết kế và thực thi **Ma trận RBAC**: triển khai route guard theo vai trò qua 7 vai trò (IT Admin, Director, Finance/Auditor, Inventory Manager, Marketing Manager, Store Manager, Sales Staff), xác nhận hạn chế sidebar navigation và endpoint API cho mỗi vai trò. | 18/06 |
| Thứ Sáu | Tích hợp **Mapbox GL JS** và **Plotly.js**: render 3D store marker từ tọa độ database, triển khai click event handler lấy dự báo `/predict` từ Lambda API và render biểu đồ Plotly line Actual vs. Forecasted với banner cảnh báo thiếu hàng tồn kho. | 19/06 |

---

### Kết Quả Học Tập Chính

- **Data Abstraction đa chế độ:** Thiết kế DAL với chuyển đổi chế độ rõ ràng cho phép cùng một codebase chạy trong môi trường local, integration testing và cloud production mà không thay đổi code—chỉ cấu hình biến môi trường khác nhau.
- **TOTP Không Phụ Thuộc:** Triển khai HMAC-SHA1 và Base32 gốc (không dùng `speakeasy` hay `otplib`) chứng minh TOTP chuẩn RFC 6238 đạt được trong ~50 dòng JavaScript, loại bỏ bề mặt tấn công phụ thuộc ngoài.
- **Mẫu Middleware RBAC:** Tập trung kiểm tra quyền trong Express middleware tái sử dụng (thay vì phân tán logic điều kiện trong route handler) giúp ma trận quyền dễ audit và chỉnh sửa từ một file duy nhất.
- **Data Binding Marker Mapbox:** Điền Mapbox GL JS marker từ danh sách cửa hàng PostgreSQL cho phép bản đồ phản ánh dữ liệu trực tiếp thay vì tọa độ hardcoded, giúp portal có thể bảo trì khi mạng lưới cửa hàng mở rộng.
- **Trực quan hóa Nhu Cầu Plotly.js:** Chồng số lượng lịch sử thực tế lên số lượng dự báo do Lambda tạo trong biểu đồ Plotly cung cấp xác thực trực quan tức thời về độ chính xác dự đoán của mô hình ML cho các bên liên quan kinh doanh.

---

*Nguồn tài liệu chính: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
