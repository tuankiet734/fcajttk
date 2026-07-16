---
title: "Khởi tạo Cơ sở dữ liệu nghiệp vụ"
date: 2024-07-07
weight: 1
chapter: false
pre : " <b> 5.3.1. </b> "
---

### 5.3.1. Khởi tạo Cơ sở dữ liệu nghiệp vụ (`fashion-rds`)

Dưới đây là hướng dẫn chi tiết các bước khởi tạo CSDL nghiệp vụ bằng Amazon RDS PostgreSQL trên AWS Console:

1. Đăng nhập vào AWS Management Console, tại thanh tìm kiếm trên cùng, gõ `RDS` và chọn dịch vụ **RDS**.

![Truy cập dịch vụ RDS](/images/5-Workshop/5.3-Database-setup/5.3.1-step01-search-rds.png)

2. Trong giao diện RDS Dashboard, nhấp vào nút **Create database** màu cam.

![RDS Dashboard](/images/5-Workshop/5.3-Database-setup/5.3.1-step02-rds-dashboard.png)

3. Chọn phương thức khởi tạo **Standard create** để có thể tùy biến cấu hình chi tiết.

![Chọn phương thức Standard Create](/images/5-Workshop/5.3-Database-setup/5.3.1-step03-standard-create.png)

4. Chọn loại Engine là **PostgreSQL** và chọn phiên bản mặc định phù hợp (ví dụ PostgreSQL 15.18-R1).

![Chọn Engine PostgreSQL](/images/5-Workshop/5.3-Database-setup/5.3.1-step04-engine-postgresql.png)

5. Chọn Template là **Free Tier** để đảm bảo tối ưu chi phí và sử dụng tài nguyên miễn phí.

![Chọn Template Free Tier](/images/5-Workshop/5.3-Database-setup/5.3.1-step05-template-freetier.png)

6. Tại mục Settings, nhập tên định danh CSDL là `fashion-rds` và cấu hình tài khoản quản trị master username là `dbadmin`.

![Cấu hình Settings định danh](/images/5-Workshop/5.3-Database-setup/5.3.1-step06-settings-identifier.png)

7. Cấu hình loại instance mặc định `db.t3.micro` và loại lưu trữ `gp2` với dung lượng `20 GiB`, đồng thời bật Storage Autoscaling.

![Cấu hình Instance và Storage](/images/5-Workshop/5.3-Database-setup/5.3.1-step07-instance-storage.png)

8. Cấu hình Connectivity: Chọn Default VPC, cấu hình Public access là **Yes** và chọn **Create new** VPC Security Group.

![Cấu hình Connectivity](/images/5-Workshop/5.3-Database-setup/5.3.1-step08-connectivity.png)

9. Cuộn xuống phần Additional configuration và nhập tên CSDL khởi tạo ban đầu là `fashiondb`.

![Cấu hình Additional configuration](/images/5-Workshop/5.3-Database-setup/5.3.1-step09-additional-config.png)

10. Xem lại ước tính chi phí ở cuối trang và nhấp vào nút **Create database** để bắt đầu tạo CSDL.

![Nhấp nút Create Database](/images/5-Workshop/5.3-Database-setup/5.3.1-step10-create-button.png)

11. Hệ thống bắt đầu khởi tạo CSDL với trạng thái **Creating**. Quá trình này mất khoảng 5-7 phút.

![Trạng thái Creating trên RDS](/images/5-Workshop/5.3-Database-setup/5.3.1-step11-creating-status.png)