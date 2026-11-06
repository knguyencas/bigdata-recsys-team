# Kế hoạch thí nghiệm

Mỗi cấu hình chạy 3 lần, báo trung bình và độ lệch chuẩn.

| Mã | Câu hỏi | Thay đổi | Đo | Ai làm |
| --- | --- | --- | --- | --- |
| E1 | Phương pháp nào gợi ý tốt nhất, tốn bao nhiêu tính toán? | Popularity, Item-CF, ALS, SASRec | Recall@K, NDCG@K, coverage, thời gian | Cas |
| E2 | Thời gian chạy tăng thế nào khi dữ liệu tăng? | subset 10, 25, 50, 100% | thời gian từng bước | Cas chạy, TV2 phân tích |
| E3 | Thêm máy nhanh hơn bao nhiêu, có đáng tiền không? | 1 máy, 2 worker, 3 worker | thời gian, speedup, tiền thật | TV2 dựng cluster và tính tiền, Cas chạy |
| E4 | CSV hay Parquet, Spark hay BigQuery? | định dạng, công cụ | dung lượng, thời gian, byte bị tính tiền | Cas (Spark), TV3 (BigQuery) |
| E5 | API trả lời đủ nhanh không? | số request đồng thời | p50, p95 (ms) | TV4 |
| E6 | Mô hình nào dự đoán mua hàng tốt nhất? | Logistic Regression, GBT | AUC, F1, thời gian | Cas |
