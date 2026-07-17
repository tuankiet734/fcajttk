---
title: "Tuần 8"
date: 2026-06-08
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

# Nhật Ký Làm Việc: Forecast REST API với AWS Lambda & Lập Lịch Hàng Ngày với Amazon EventBridge

> **Tuần 8 - Thứ Hai, ngày 08/06/2026:** Biến đổi mô hình LightGBM đã huấn luyện thành REST API endpoint serverless production-grade. Tuần này bao gồm viết Lambda inference function, đóng gói Python dependencies vào deployment layer, cấu hình API Gateway routing và xác thực API key, đồng thời kết nối EventBridge daily cron rule để tự động hóa chu trình làm mới ML pipeline.

---

### Mục Tiêu Học Tập Trong Tuần

- Viết **Python Lambda function** tải artifact mô hình từ S3, nạp vào bộ nhớ và thực thi LightGBM inference trên các yêu cầu dự đoán JSON.
- Đóng gói Python dependencies (`lightgbm`, `scikit-learn`, `pandas`, `boto3`) vào **Lambda Layer** để thỏa mãn giới hạn kích thước deployment package 250 MB.
- Triển khai Lambda function phía sau **Amazon API Gateway** với route POST `/predict` bảo mật bằng API key.
- Cấu hình **Amazon EventBridge** rule với cron expression hàng ngày để kích hoạt toàn bộ pipeline: Glue ETL → Huấn luyện → Tải model.
- Xác thực API đầu cuối bằng cách gửi yêu cầu inference thử nghiệm qua `curl` và kiểm tra định dạng output dự báo.

---

### Nhật Ký Hoạt Động Theo Ngày

| Ngày | Hoạt động | Ngày thực hiện |
|------|-----------|----------------|
| Thứ Hai | Viết **Lambda inference handler** (`lambda_handler.py`): triển khai logic tải mô hình S3 lúc cold start, xây dựng feature vector từ JSON payload đầu vào và gọi `.predict()` trả về nhu cầu dự báo dưới dạng JSON. | 08/06 |
| Thứ Ba | Xây dựng và công bố **Lambda Layer**: tạo Python virtual environment, cài đặt `lightgbm`, `scikit-learn`, `pandas`, nén thư mục `site-packages` và tải lên làm Lambda Layer `ml-inference-layer`. | 09/06 |
| Thứ Tư | Cấu hình **Amazon API Gateway**: tạo cấu trúc REST API resource (`/predict`), liên kết POST method đến Lambda qua Lambda Proxy integration, triển khai lên stage `dev` và tạo **API Key** với Usage Plan giới hạn 1.000 yêu cầu/ngày. | 10/06 |
| Thứ Năm | Thiết lập **Amazon EventBridge Rule**: tạo scheduled rule với cron expression `cron(0 1 * * ? *)` (kích hoạt hàng ngày lúc 01:00 UTC), cấu hình invoke Lambda orchestrator chạy tuần tự Glue → EC2 training → S3 upload. | 11/06 |
| Thứ Sáu | Thực hiện **xác thực API đầu cuối**: dùng `curl -X POST` với header `x-api-key` và payload mẫu, nhận response 200 hợp lệ với payload nhu cầu dự báo và xác nhận test invocation EventBridge thành công trong log thực thi Lambda. | 12/06 |

---

### Kết Quả Học Tập Chính

- **Tối ưu Lambda Cold Start:** Tải file model pickle lớn (>50 MB) từ S3 ở mỗi cold start gây trễ. Lưu cache đối tượng mô hình trong scope global Lambda (thư mục `/tmp`) đảm bảo mô hình chỉ tải một lần mỗi vòng đời execution environment.
- **Lambda Layer cho ML Dependencies:** Các thư viện Python ML (`lightgbm`, `scikit-learn`) với C extension native phải được biên dịch cho môi trường Lambda (Amazon Linux 2). Xây dựng layer trên Docker container Amazon Linux đảm bảo tương thích binary.
- **Lambda Proxy Integration API Gateway:** Dùng proxy integration chuyển toàn bộ ngữ cảnh HTTP (headers, body, path) đến Lambda dưới dạng event object có cấu trúc, cho function kiểm soát đầy đủ cấu trúc response.
- **Cú pháp Cron EventBridge:** AWS EventBridge dùng cú pháp cron 6 trường (thêm ký tự `?` wildcard). Test trigger với tính năng "Send test event" trong console xác thực hành vi rule trước khi chờ lần thực thi đầu tiên theo lịch.
- **API Key vs. Cognito Authorizer:** API Key phù hợp cho các lệnh gọi server-to-server nội bộ; tuy nhiên cho xác thực người dùng cuối, Cognito User Pool authorizer cung cấp xác thực danh tính mạnh hơn.

---

*Nguồn tài liệu chính: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
