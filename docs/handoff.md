# Bàn giao giữa các phần

File này là thoả thuận chung: ai đưa gì cho ai, ở đâu, định dạng ra sao. Đổi bất kỳ
dòng nào thì mở pull request và báo người nhận.

## Thông tin dự án (TV2 điền)

| Mục | Giá trị |
| --- | --- |
| Project ID | |
| Region | us-central1 |
| Bucket | gs:// |
| Nhãn chi phí | `owner=<tên>`, `exp=<mã thí nghiệm>` gắn lên mọi cluster, VM, dịch vụ |

## Cas giao

### Dữ liệu sạch, cho TV3

`gs://<bucket>/clean/`, Parquet, chia thư mục theo ngày `event_date=YYYY-MM-DD/`.

| Cột | Kiểu | Ghi chú |
| --- | --- | --- |
| event_time | timestamp | giờ UTC |
| event_type | string | `view`, `cart` hoặc `purchase` |
| product_id | int64 | |
| category_id | int64 | |
| category_code | string | có thể trống |
| brand | string | trống được thay bằng `unknown` |
| price | double | không âm |
| user_id | int64 | |
| user_session | string | |
| event_date | date | cột chia thư mục |
| category_l1 | string | phần đầu của category_code, hoặc `unknown` |

Kèm theo: bảng số liệu EDA dạng CSV ở `gs://<bucket>/reports/eda/` (theo ngày, theo giờ,
theo danh mục, phân bố hoạt động người dùng).

### Gợi ý, cho TV4

`gs://<bucket>/handoff/p10_als/part-*.json`, mỗi dòng một JSON:

```json
{"user_id": "512386086", "items": [{"product_id": 1004767, "category": "electronics.smartphone", "brand": "samsung", "price": 254.82}]}
```

- `user_id` là chuỗi. `items` có tối đa 20 sản phẩm, đã xếp từ gợi ý tốt nhất xuống.
- Có một dòng `user_id` = `__popular__`: danh sách sản phẩm phổ biến, dùng cho người
  dùng không có trong file.
- `category` có thể là null.

### Số đo thời gian, cho TV2 (E2, E3)

`gs://<bucket>/metrics/<loại>/<run_id>/`, mỗi lần chạy một dòng JSON có thời gian từng
bước, số máy, loại máy, nhãn thí nghiệm. Bản tổng hợp CSV ở `gs://<bucket>/reports/`.

### Kết quả mô hình, cho TV5

Bảng CSV ở `gs://<bucket>/reports/` và biểu đồ PNG cho E1, E2, E3, E4, EDA.

## TV2 giao

- Quyền vào project cho cả nhóm, bảng thông tin dự án ở trên.
- Cluster theo cấu hình E3 (1 máy, 2 worker, 3 worker) khi Cas cần chạy E3.
- Bảng chi phí thật theo nhãn, cho TV5.

## TV3 giao

- Link dashboard Looker Studio, bảng 5V và kết quả E4 phía BigQuery, cho TV5.

## TV4 giao

- Link web demo và kết quả E5 (p50, p95), cho TV5.
