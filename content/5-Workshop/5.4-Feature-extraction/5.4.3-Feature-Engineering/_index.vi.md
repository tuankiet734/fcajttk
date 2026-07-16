---
title: "Script Spark ETL kỹ nghệ đặc trưng"
date: 2024-07-07
weight : 4
chapter: false
pre : " <b> 5.4.3. </b> "
---

### 5.4.3. Script Spark ETL Kỹ nghệ Đặc trưng (`glue_feature_engineering.py`)

Tác vụ chính được thực hiện bằng **Apache Spark** trên **AWS Glue** để tính toán các đặc trưng chuỗi thời gian phục vụ trực tiếp cho huấn luyện mô hình.

#### Các bước cấu hình Spark ETL Job:

1. Để kết nối với PostgreSQL RDS, chúng ta tải file PostgreSQL JDBC Driver `postgresql-42.7.3.jar` lên thư mục S3, sau đó khai báo đường dẫn tại **Job details** -> **Libraries** -> **Dependent jars path**.

![Cấu hình Dependent Jars Path](/images/5-Workshop/5.4-Feature-extraction/5.4.3-step04-dependent-jars.png)

2. Copy nội dung mã nguồn của Spark ETL từ [glue_feature_engineering.py](file:///d:/b%C3%A1o%20c%C3%A1o%20AWS/source_code/glue_feature_engineering.py) dán vào trình soạn thảo Script của Glue Job.

![Nhập mã nguồn Spark trong Script editor](/images/5-Workshop/5.4-Feature-extraction/5.4.3-step05-spark-code.png)