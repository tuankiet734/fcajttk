---
title: "Tuần 7"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

# Nhật Ký Làm Việc: Pipeline Huấn Luyện Mô Hình ML & Lưu Trữ Artifact Trên Amazon S3

> **Tuần 7 - Thứ Hai, ngày 01/06/2026:** Thực thi toàn bộ chu trình huấn luyện machine learning trên EC2 instance `ML-Forecast-Server`. Tuần này bao gồm thiết lập môi trường Python, tải dữ liệu đặc trưng từ `training-db`, chạy script huấn luyện LightGBM, đánh giá số liệu hiệu suất mô hình và tải artifact đã tuần tự hóa lên S3 bucket.

---

### Mục Tiêu Học Tập Trong Tuần

- Cấu hình **EC2 ML-Forecast-Server**: cài đặt Python 3.10, LightGBM, scikit-learn, psycopg2 và boto3.
- Tải bảng đặc trưng đã kỹ thuật từ `training-db` và chuẩn bị phân chia train/validation dataset.
- Huấn luyện **mô hình LightGBM gradient-boosted tree** tối ưu hóa cho dự báo nhu cầu chuỗi thời gian.
- Đánh giá mô hình bằng RMSE, MAE và MAPE trên tập validation.
- Tuần tự hóa mô hình đã huấn luyện thành artifact `.pkl` và tải lên S3 bucket cho Lambda inference.

---

### Nhật Ký Hoạt Động Theo Ngày

| Ngày | Hoạt động | Ngày thực hiện |
|------|-----------|----------------|
| Thứ Hai | Cấu hình môi trường **ML-Forecast-Server**: SSH vào EC2 instance, cài đặt Python 3.10 qua deadsnakes PPA, tạo virtual environment với `pip install lightgbm scikit-learn psycopg2-binary boto3 pandas`. | 01/06 |
| Thứ Ba | Phát triển **module tải dữ liệu**: viết Python script kết nối `training-db` qua psycopg2 connection pool, tải bảng đặc trưng vào Pandas DataFrame, áp dụng phân chia train/validation theo thứ tự thời gian (80/20 theo ngày). | 02/06 |
| Thứ Tư | Huấn luyện **mô hình dự báo LightGBM**: cấu hình hyperparameter (num_leaves=63, learning_rate=0.05, n_estimators=500, subsample=0.8), chạy vòng huấn luyện với early stopping trên tập validation. | 03/06 |
| Thứ Năm | Đánh giá hiệu suất mô hình trên tập validation: tính **RMSE**, **MAE** và **MAPE** theo cửa hàng và theo phân khúc SKU. Xác định các SKU lỗi cao để xem xét chẩn đoán. | 04/06 |
| Thứ Sáu (lên văn phòng) | Tuần tự hóa mô hình đã huấn luyện với `pickle.dump()` thành ba file artifact và tải lên S3 với `boto3`: `model_global.pkl`, `model_store.pkl`, `model_sku.pkl`. Xác minh tính toàn vẹn upload với `aws s3 ls`. | 05/06 |

---

### Kết Quả Học Tập Chính

- **Tính phù hợp của LightGBM cho dự báo bán lẻ:** Chiến lược phát triển cây leaf-wise của LightGBM nắm bắt hiệu quả các mẫu mùa vụ phi tuyến trong dữ liệu giao dịch bán lẻ, vượt trội hơn mô hình hồi quy tuyến tính đơn giản trên MAPE.
- **Phân chia Train/Validation theo thứ tự thời gian:** Với bài toán chuỗi thời gian, **phải tránh xáo trộn ngẫu nhiên**—các phân chia phải tuân thủ thứ tự thời gian để mô phỏng điều kiện dự báo thực tế không bị rò rỉ dữ liệu.
- **Phân tích Feature Importance:** LightGBM tính feature importance theo số lần split và gain, tiết lộ `rolling_avg_7d` và `lag_7` là đặc trưng dự đoán tốt nhất, xác nhận tính đúng đắn của thiết kế feature engineering với Glue.
- **S3 làm Model Registry:** Tải artifact mô hình có phiên bản lên S3 với quy ước đường dẫn cho phép Lambda function tải phiên bản mới nhất một cách linh hoạt mà không cần tái triển khai code.
- **IAM Instance Profile cho truy cập S3:** Gắn IAM Instance Profile với quyền `AmazonS3FullAccess` vào EC2 instance cho phép `boto3` xác thực hoạt động S3 mà không nhúng access key vào codebase.

---

*Nguồn tài liệu chính: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
