---
title: "Khởi tạo Cơ sở dữ liệu huấn luyện"
date: 2024-07-07
weight: 3
chapter: false
pre : " <b> 5.3.2. </b> "
---

### 5.3.2. Khởi tạo Cơ sở dữ liệu huấn luyện (`training-db`)

Cơ sở dữ liệu huấn luyện chuyên dụng được sử dụng để lưu trữ các bảng đặc trưng (features) phục vụ trực tiếp cho tiến trình máy học. Dưới đây là hướng dẫn khởi tạo chi tiết trên AWS Console:

1. Trong giao diện tạo cơ sở dữ liệu mới (Standard create / PostgreSQL), tại mục **Engine Version**, lựa chọn phiên bản **PostgreSQL 18.3** (phiên bản mới nhất).

![Chọn PostgreSQL Engine 18.3](/images/5-Workshop/5.3-Database-setup/5.3.2-step01-engine-postgresql18.png)

2. Tại mục Settings, đặt tên DB instance identifier là `training-db` và nhập thông tin tài khoản master username là `dbadmin`.

![Cấu hình Settings training-db](/images/5-Workshop/5.3-Database-setup/5.3.2-step02-settings-trainingdb.png)

3. Tại mục Connectivity, chọn Default VPC, cấu hình Public access là **Yes** và chọn **Create new** VPC Security Group.

![Cấu hình Connectivity cho training-db](/images/5-Workshop/5.3-Database-setup/5.3.2-step03-connectivity-public.png)

4. Mở rộng phần Additional configuration và đặt tên CSDL ban đầu là `fashiondb`.

![Cấu hình database name fashiondb](/images/5-Workshop/5.3-Database-setup/5.3.2-step04-additional-fashiondb.png)

5. Sau khi hoàn thành khởi tạo, kiểm tra danh sách Databases trên RDS để đảm bảo cả hai CSDL `fashion-rds` và `training-db` đều ở trạng thái **Available**.

![Danh sách CSDL RDS Available](/images/5-Workshop/5.3-Database-setup/5.3.2-step05-training-db-available.png)