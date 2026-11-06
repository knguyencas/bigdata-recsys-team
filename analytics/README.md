# analytics (TV3)

Phân tích trên BigQuery và dashboard. File SQL, notebook, ảnh chụp dashboard để ở đây.

- Tạo dataset BigQuery, tạo bảng đọc từ `gs://<bucket>/clean/` (bảng ngoài hoặc nạp vào).
- E4 phía BigQuery: chạy cùng 3 câu truy vấn mà Cas chạy trên Spark, ghi thời gian và
  số byte bị tính tiền, so CSV với Parquet.
- Dashboard Looker Studio: lượng sự kiện theo ngày, phễu xem, giỏ, mua, danh mục và
  thương hiệu bán chạy.
- Bảng 5V có biểu đồ cho báo cáo.

Đầu vào và đầu ra: xem `docs/handoff.md`.
