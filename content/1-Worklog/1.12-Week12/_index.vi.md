---
title: "Tuần 12"
date: 2026-07-06
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

# Nhật Ký Làm Việc: Tài Liệu Hóa Cuối Kỳ, Báo Cáo Workshop & Tổng Kết Chương Trình

> **Tuần 12 - Thứ Hai, ngày 06/07/2026:** Dành tuần cuối cùng của chương trình First Cloud AI Journey để hoàn thiện toàn bộ tài liệu còn lại, viết báo cáo kỹ thuật dự án toàn diện, tổng kết học tập qua 12 tuần và chuẩn bị bài trình bày cuối kỳ cho ban điều phối chương trình. Tuần này đánh dấu sự kết thúc chính thức của kỳ thực tập, ngày kết thúc chính thức: **12/07/2026**.

---

### Mục Tiêu Học Tập Trong Tuần

- Hoàn thiện tất cả **trang tài liệu workshop Hugo** cho website workshop đã xuất bản, đảm bảo mỗi phần được viết đầy đủ, chính xác và nhất quán với triển khai thực tế.
- Viết **báo cáo kỹ thuật dự án cuối kỳ** tóm tắt các quyết định kiến trúc, thách thức triển khai, kết quả hiệu suất và bài học rút ra qua 10 giai đoạn workshop.
- Thực hiện **tổng kết hồi tưởng 12 tuần**: xác định các kỹ năng có tác động nhất được tích lũy, các trở ngại khó khăn nhất đã vượt qua và các lĩnh vực cần tiếp tục phát triển.
- Chuẩn bị và nộp tất cả **deliverable chương trình**: worklog entries, website tài liệu workshop, link video demo dự án và báo cáo cuối kỳ.
- Phản ánh về **quy trình cloud AWS đầu cuối** được thực hành trong suốt chương trình và phác thảo lộ trình cá nhân cho chứng chỉ AWS.

---

### Nhật Ký Hoạt Động Theo Ngày

| Ngày | Hoạt động | Ngày thực hiện |
|------|-----------|----------------|
| Thứ Hai | Hoàn thiện và xuất bản tất cả **phần tài liệu Workshop Hugo**: hoàn chỉnh các trang subsection còn lại cho Network Setup, Database Setup, Feature Extraction, Model Training, Model API, Web Portal và EC2 Deployment; xác minh tất cả internal link, cú pháp code block và tham chiếu diagram chính xác. | 06/07 |
| Thứ Ba | Thảo thảo **Báo cáo Kiến trúc Kỹ thuật**: ghi lại cơ sở lý luận kiến trúc hệ thống (thiết kế dual-zone, mẫu dual-database, serverless inference), chi tiết các dịch vụ AWS sử dụng và cấu hình, tóm tắt benchmark hiệu suất (Lambda p99 inference latency, MAPE LightGBM, ASG scaling event log). | 07/07 |
| Thứ Tư | Viết **Báo cáo ML Pipeline**: chi tiết phương pháp feature engineering (lag features, rolling averages, velocity metrics), quá trình điều chỉnh hyperparameter LightGBM, kết quả đánh giá mô hình theo cửa hàng và phân khúc SKU, và khuyến nghị cải thiện độ chính xác dự báo. | 08/07 |
| Thứ Năm | Hoàn thiện **Worklog 12 Tuần**: hoàn thành tất cả tóm tắt entry tuần, đối chiếu các hoạt động với lịch chương trình gốc, đảm bảo định dạng nhất quán qua tất cả entry và tích hợp link video demo dự án vào tài liệu Tuần 11. | 09/07 |
| Thứ Sáu | Thực hiện **tự đánh giá cuối cùng** theo mục tiêu học tập chương trình, tổ chức tất cả tài sản dự án (repository code, file báo cáo, video demo, URL website workshop) thành gói nộp bài và thực hiện kiểm tra chất lượng nội bộ. | 11/07 |
| Thứ Bảy | Nộp tất cả **deliverable chương trình cuối kỳ**: tải lên URL website workshop Hugo, link video demo Google Drive, worklog hoàn chỉnh và báo cáo kỹ thuật lên cổng nộp bài. Hoàn tất bàn giao chương trình. **Ngày kết thúc chính thức: 12/07/2026.** | 12/07 |

---

### Tổng Kết 12 Tuần Chương Trình

**Kỹ Năng Tích Lũy:**
- Cấp phát hạ tầng cloud đầu cuối trên AWS (VPC, EC2, RDS, S3, CloudFront, ALB, ASG).
- Thiết kế và triển khai kiến trúc serverless (Lambda, API Gateway, EventBridge).
- Phát triển ETL pipeline big data với AWS Glue (PySpark, JDBC connection, job bookmark).
- Huấn luyện, đánh giá và triển khai mô hình machine learning production trên hạ tầng cloud.
- Phát triển ứng dụng web full-stack với Node.js, JWT authentication, TOTP MFA và RBAC.
- Quản lý chi phí hạ tầng qua cảnh báo ngân sách, nguyên tắc dọn dẹp tài nguyên và chiến lược right-sizing.

**Thách Thức Khó Khăn Đã Vượt Qua:**
- **Cấu hình Security Group JDBC Glue:** Phát hiện CIDR ENI của Glue phải được cho phép rõ ràng trong Security Group RDS (không chỉ Security Group ứng dụng) đòi hỏi hiểu biết về kiến trúc mạng nền tảng của Glue.
- **Tương thích Binary Lambda Layer:** Xây dựng layer thư viện Python ML (LightGBM với C extension) trên máy non-Amazon-Linux tạo ra binary không tương thích; chuyển sang build layer bằng Docker đã giải quyết vấn đề.
- **Chuỗi phụ thuộc xóa RDS:** Cố xóa RDS cluster trước khi xóa instance bên trong liên tục gây lỗi `DependencyViolation`, đòi hỏi trình tự xóa instance-trước, cluster-sau đúng đắn.
- **Tích hợp PM2 Startup Systemd:** Quy trình hai bước PM2 startup (chạy `pm2 startup` sau đó thực thi thủ công lệnh `sudo env PATH=...` được tạo ra) không trực quan ngay từ đầu nhưng thiết yếu cho tính bền vững sau reboot.

**Lộ Trình Chứng Chỉ AWS:**
1. **AWS Cloud Practitioner (CLF-C02)** — Xác nhận nền tảng (đã chuẩn bị qua các hoạt động Tuần 1–3).
2. **AWS Solutions Architect Associate (SAA-C03)** — Mục tiêu tiếp theo (6–8 tuần học tập, tập trung VPC, IAM, storage, database và compute chuyên sâu).
3. **AWS Machine Learning Specialty (MLS-C01)** — Mục tiêu dài hạn sau SAA, xây dựng trên kinh nghiệm ML pipeline từ Tuần 6–8.

---

### Tóm Tắt Chương Trình & Bài Học Cuối Cùng

- **Tổng số dịch vụ AWS đã sử dụng:** 12 dịch vụ phân tán trên compute, storage, networking, data, ML và bảo mật.
- **Tổng số giai đoạn workshop hoàn thành:** 10 giai đoạn (Kiến trúc → Mạng → CSDL → ETL → Huấn luyện → API → Portal → Triển khai → Dọn dẹp → Demo).
- **Tổng credit đã sử dụng:** $200 credit chương trình được sử dụng hiệu quả qua tất cả giai đoạn workshop và task thực hành hướng dẫn.
- **Bài Học Quý Giá Nhất:** Insight có tác động nhất từ chương trình này là **kiến trúc cloud về cơ bản là quản lý các phụ thuộc**—giữa các dịch vụ, giữa ranh giới bảo mật, giữa các tầng dữ liệu và giữa trình tự xóa tài nguyên. Mỗi quyết định cấu hình đều mang lại hệ quả downstream phải được lên kế hoạch trước khi cấp phát tài nguyên đầu tiên.

---

*Nguồn tài liệu chính: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
