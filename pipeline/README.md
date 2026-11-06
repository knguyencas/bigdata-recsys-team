# pipeline (Cas)

Spark trên Dataproc và mô hình Deep Learning.

- Đưa dataset từ Kaggle lên `raw/`, làm sạch thành `clean/` (Parquet theo ngày).
- EDA cho bảng 5V, lấy subset người dùng, chia train/test theo thời gian (tuần cuối làm test).
- Mô hình gợi ý: Popularity, Item-CF, ALS (Spark MLlib), SASRec (PyTorch trên GPU Colab).
- Dự đoán mua hàng: Logistic Regression, GBT.
- Thí nghiệm E1, E4 phía Spark, E6; chạy job cho E2, E3.

Đầu vào và đầu ra: xem `docs/handoff.md`.
