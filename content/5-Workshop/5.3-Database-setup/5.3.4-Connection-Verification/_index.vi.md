---
title: "Kiểm tra kết nối"
date: 2024-07-07
weight: 4
chapter: false
pre : " <b> 5.3.4. </b> "
---

### 5.3.4. Kiểm tra Kết nối và Thiết lập Bảng mẫu

Sau khi cấu hình hoàn tất mạng và cơ sở dữ liệu, chúng ta thực hiện kết nối cơ sở dữ liệu bằng DBeaver để thiết lập các bảng nghiệp vụ.

#### CLI Connection (Tham khảo):
Kết nối CSDL nghiệp vụ bằng CLI:
```bash
psql -h fashion-rds.c7846wiue0od.ap-southeast-1.rds.amazonaws.com -p 5432 -U dbadmin -d fashiondb
```
Kết nối CSDL huấn luyện bằng CLI:
```bash
psql -h training-db.c7846wiue0od.ap-southeast-1.rds.amazonaws.com -p 5432 -U dbadmin -d fashiondb
```

---

#### Các bước kết nối bằng DBeaver và khởi tạo bảng mẫu:

1. Mở phần mềm DBeaver, nhấp vào **Database** > **New Database Connection** và lựa chọn driver là **PostgreSQL**.

![Tạo kết nối mới trong DBeaver](/images/5-Workshop/5.3-Database-setup/5.3.4-step03-dbeaver-newconn.png)

2. Nhập các thông tin kết nối chi tiết bao gồm Host endpoint, Database name (`fashiondb`), Username (`dbadmin`) và Password.

![Cấu hình thông tin kết nối](/images/5-Workshop/5.3-Database-setup/5.3.4-step04-dbeaver-config.png)

3. Nhấp vào nút **Test Connection...** và xác nhận thông báo kết nối thành công (`Connected`).

![Test Connection thành công](/images/5-Workshop/5.3-Database-setup/5.3.4-step05-dbeaver-testconnection.png)

4. Thực hiện các bước tương tự cho cơ sở dữ liệu còn lại, đảm bảo DBeaver hiển thị đầy đủ cả hai kết nối cơ sở dữ liệu.

![Cả hai kết nối cơ sở dữ liệu đã sẵn sàng](/images/5-Workshop/5.3-Database-setup/5.3.4-step06-dbeaver-both-connected.png)

5. Mở SQL Editor trong DBeaver và chạy đoạn code SQL sau trên CSDL nghiệp vụ (`fashion-rds`) để tạo các bảng mẫu:

```sql
-- 1. Bảng sản phẩm (products)
CREATE TABLE products (
    product_id VARCHAR(50) PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10, 2)
);

-- 2. Bảng cửa hàng (stores)
CREATE TABLE stores (
    store_id VARCHAR(50) PRIMARY KEY,
    store_name VARCHAR(100),
    city VARCHAR(50)
);

-- 3. Bảng lịch sử giao dịch (transactions)
CREATE TABLE transactions (
    transaction_id VARCHAR(50) PRIMARY KEY,
    store_id VARCHAR(50) REFERENCES stores(store_id),
    product_id VARCHAR(50) REFERENCES products(product_id),
    date DATE,
    qty INT
);
```

![Chạy SQL tạo bảng và kiểm tra cấu trúc bảng](/images/5-Workshop/5.3-Database-setup/5.3.4-step08-create-tables.png)