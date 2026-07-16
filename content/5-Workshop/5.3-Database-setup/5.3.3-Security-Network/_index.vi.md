---
title: "Cấu hình Mạng và Bảo mật"
date: 2024-07-07
weight: 3
chapter: false
pre : " <b> 5.3.3. </b> "
---

### 5.3.3. Cấu hình Mạng và Bảo mật (Security & Network Groups)

Để đảm bảo kết nối an toàn cho hai cơ sở dữ liệu, chúng ta cần cấu hình quy tắc Inbound Rules cho VPC Security Group. Dưới đây là hướng dẫn chi tiết từng bước:

1. Đăng nhập vào AWS Console, tìm kiếm dịch vụ **EC2** và chọn **Security Groups** trong mục **Network & Security** ở menu điều hướng bên trái.

![Tìm Security Groups trên EC2 Console](/images/5-Workshop/5.3-Database-setup/5.3.3-step01-ec2-security-groups.png)

2. Chọn đúng Security Group tương ứng đang được áp dụng cho các RDS instance của bạn.

![Chọn Security Group của RDS](/images/5-Workshop/5.3-Database-setup/5.3.3-step02-select-sg.png)

3. Nhấp chọn tab **Inbound rules** ở nửa dưới màn hình và chọn **Edit inbound rules**.

![Chỉnh sửa Inbound Rules](/images/5-Workshop/5.3-Database-setup/5.3.3-step03-edit-inbound.png)

4. Thêm quy tắc đầu tiên (Rule 1): Chọn Type là `PostgreSQL` (cổng `5432`), chọn Source là **My IP** để cho phép máy cá nhân kết nối.

![Cấu hình Rule 1 cho IP cá nhân](/images/5-Workshop/5.3-Database-setup/5.3.3-step04-rule1-myip.png)

5. Thêm quy tắc thứ hai (Rule 2): Chọn Type là `PostgreSQL`, chọn Source là **Custom** và nhập mã ID Security Group của máy chủ huấn luyện `ML-Forecast-Server`.

![Cấu hình Rule 2 cho EC2 Server](/images/5-Workshop/5.3-Database-setup/5.3.3-step05-rule2-ec2sg.png)

6. Thêm quy tắc thứ ba (Rule 3): Chọn Type là `PostgreSQL`, chọn Source là **Custom** và nhập lại chính mã ID Security Group hiện tại để tự tham chiếu (Self-referencing) phục vụ kết nối AWS Glue.

![Cấu hình Rule 3 cho AWS Glue tự tham chiếu](/images/5-Workshop/5.3-Database-setup/5.3.3-step06-rule3-selfreference.png)

7. Kiểm tra lại toàn bộ quy tắc đã thêm và nhấp vào nút **Save rules** để áp dụng.

![Lưu cấu hình Security Group thành công](/images/5-Workshop/5.3-Database-setup/5.3.3-step07-saved-rules.png)