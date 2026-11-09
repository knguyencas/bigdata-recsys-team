# Kế hoạch thí nghiệm

Mỗi cấu hình chạy 3 lần, báo trung bình và độ lệch chuẩn. Tên dịch vụ ghi theo cả GCP và AWS;
nhóm dùng bên nào thì đo trên bên đó.

| Mã | Câu hỏi | Thay đổi | Đo | Ai làm |
| --- | --- | --- | --- | --- |
| E1 | Phương pháp nào gợi ý tốt nhất, tốn bao nhiêu tính toán? | Popularity, Item-CF, ALS, SASRec | Recall@K, NDCG@K, coverage, thời gian | TV1 |
| E2 | Thời gian chạy tăng thế nào khi dữ liệu tăng? | subset 10, 25, 50, 100% | thời gian từng bước | TV1 chạy, TV2 phân tích |
| E3 | Thêm máy nhanh hơn bao nhiêu, có đáng tiền không? | 1 máy, 2 worker, 3 worker | thời gian, speedup, tiền thật | TV2 dựng cluster và tính tiền, TV1 chạy |
| E4 | CSV hay Parquet, Spark hay kho SQL (BigQuery hoặc Athena)? | định dạng, công cụ | dung lượng, thời gian, dung lượng bị tính tiền | TV1 (Spark), TV3 (kho SQL) |
| E5 | API trả lời đủ nhanh không? | số request đồng thời | p50, p95 (ms) | TV4 |
| E6 | Mô hình nào dự đoán mua hàng tốt nhất? | Logistic Regression, GBT | AUC, F1, thời gian | TV1 |

## Ba câu truy vấn của E4

TV1 chạy trên Spark, TV3 chạy trên kho SQL, cùng trên dữ liệu CSV gốc (`raw/`) và Parquet (`clean/`).

| Mã | Truy vấn |
| --- | --- |
| q1 | Đếm số sự kiện theo `event_type` |
| q2 | Giá trung bình của sự kiện `purchase` theo `category_l1` |
| q3 | Số người dùng khác nhau từ ngày 2019-11-24 trở đi |

Trên CSV gốc chưa có `category_l1` và `event_date`: tính `category_l1` là phần trước dấu
chấm đầu tiên của `category_code` (trống thì `unknown`), `event_date` là ngày của `event_time`.
