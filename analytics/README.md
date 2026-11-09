# analytics/: TV3, phân tích và dashboard

Đọc [README gốc](../README.md) và [luật chung](../CONTRIBUTING.md) trước. Thư mục này làm theo
luật chung; mục "Quy ước riêng" ở cuối chỉ bổ sung.

## Mục tiêu

Cho thấy dữ liệu bằng dashboard và bảng 5V. Trả lời bằng số câu hỏi E4: lưu dữ liệu dạng CSV hay
Parquet, truy vấn bằng Spark hay bằng kho SQL thì nhanh hơn, rẻ hơn.

## Việc phải làm

| Mã | Việc | Hạn | Xong khi |
| --- | --- | --- | --- |
| A1 | Tạo kho SQL và bảng đọc `raw/` (CSV) và `clean/` (Parquet) | S10 | Truy vấn được các bảng |
| A2 | E4 phía kho SQL: 3 truy vấn chung, mỗi câu 3 lần, ghi thời gian và dung lượng bị tính tiền | S10 | Có bảng E4 |
| A3 | Dashboard: sự kiện theo ngày, phễu xem, giỏ, mua, danh mục, thương hiệu | S10 | Link dashboard chạy trên dữ liệu thật |
| A4 | Bảng 5V có biểu đồ | S10 | Đã giao TV5 |
| A5 | Viết phần phân tích dữ liệu, lựa chọn lưu trữ cho báo cáo | S11 | Đã giao TV5 |

Ba truy vấn chung của E4 ghi trong [docs/experiments.md](../docs/experiments.md).

## Làm trên hạ tầng

| Việc | GCP | AWS |
| --- | --- | --- |
| Kho SQL đọc file trên bucket | BigQuery (bảng ngoài và bảng nạp vào) | Athena (bảng khai báo trong Glue Data Catalog) |
| Số đo cho E4 | Thời gian, bytes billed trong lịch sử truy vấn | Thời gian, data scanned trong lịch sử truy vấn |
| Dashboard | Looker Studio | QuickSight |

Trước truy vấn lớn, xem trước số dữ liệu sẽ quét (dry run trên BigQuery, hoặc chạy thử có
`LIMIT` và xem data scanned trên Athena).

## Nhận từ và giao cho

- Nhận từ TV1: `<bucket>/clean/` và `<bucket>/reports/eda/`.
- Nhận từ TV2: quyền vào kho SQL và dashboard.
- Giao TV5: link dashboard, bảng E4, bảng 5V, phần viết; để trong `report/figures/`.

Định dạng chi tiết: [docs/handoff.md](../docs/handoff.md).

## Cấu trúc thư mục

```
analytics/
├── sql/         câu lệnh tạo bảng và các truy vấn, kể cả 3 truy vấn E4
├── dashboard/   link dashboard, ảnh chụp màn hình
└── notebooks/   tính bảng 5V, vẽ biểu đồ
```

## Cách chạy

TV3 điền khi có code: tạo bảng thế nào, chạy truy vấn E4 thế nào, mở dashboard ở đâu.

## Quy ước riêng

- Mỗi file SQL có một dòng chú thích đầu file nói nó dùng để làm gì.
- Kết quả truy vấn cần lưu lại thì ghi vào `<bucket>/analytics/`.
