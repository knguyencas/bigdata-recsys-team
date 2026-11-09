# Bàn giao giữa các phần

File này là thoả thuận chung: ai đưa gì cho ai, ở đâu, định dạng ra sao. Đổi bất kỳ dòng nào
thì mở pull request và được người nhận đồng ý (xem [luật chung](../CONTRIBUTING.md), mục 4).

## Thông tin dự án (TV2 điền)

| Mục | Giá trị |
| --- | --- |
| Cloud | GCP hoặc AWS: |
| Project ID (GCP) hoặc Account ID (AWS) | |
| Region | |
| Bucket | |
| Link web demo (TV4 điền) | |
| Link dashboard (TV3 điền) | |
| Nhãn chi phí | `owner=<tv1..tv5>`, `exp=<mã thí nghiệm hoặc dev>` |

Trong file này `<bucket>/` nghĩa là `gs://<tên bucket>/` trên GCP hoặc `s3://<tên bucket>/` trên AWS.

## Ai ghi vào đâu trên bucket

Mỗi người chỉ ghi vào phần của mình; phần của người khác chỉ đọc.

| Thư mục trên bucket | Người ghi | Người đọc |
| --- | --- | --- |
| `raw/`, `clean/`, `subset/`, `splits/`, `sequences/`, `recs/` | TV1 | TV1; TV3 đọc `raw/`, `clean/` |
| `handoff/` | TV1 | TV4 |
| `metrics/`, `reports/` | TV1 | TV2, TV5 |
| `infra/` | TV2 | Cả nhóm |
| `analytics/` | TV3 | Cả nhóm |
| `app/` | TV4 | Cả nhóm |

## TV1 giao

### Dữ liệu sạch, cho TV3

`<bucket>/clean/`, Parquet, chia thư mục theo ngày `event_date=YYYY-MM-DD/`.

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

Kèm theo: bảng số liệu EDA dạng CSV ở `<bucket>/reports/eda/` (theo ngày, theo giờ, theo
danh mục, phân bố hoạt động người dùng).

### Gợi ý, cho TV4

`<bucket>/handoff/p10_<mô hình>/part-*.json`, mỗi dòng một JSON:

```json
{"user_id": "512386086", "items": [{"product_id": 1004767, "category": "electronics.smartphone", "brand": "samsung", "price": 254.82}]}
```

- `user_id` là chuỗi. `items` có tối đa 20 sản phẩm, đã xếp từ gợi ý tốt nhất xuống.
- Có một dòng `user_id` = `__popular__`: danh sách sản phẩm phổ biến, dùng cho người dùng
  không có trong file.
- `category` có thể là null.
- File mẫu (dữ liệu giả, cùng định dạng) giao trước để TV4 làm sớm.

### Số đo thời gian, cho TV2 (E2, E3)

`<bucket>/metrics/<loại>/<run_id>/`, mỗi lần chạy một dòng JSON có thời gian từng bước, số máy,
loại máy, nhãn thí nghiệm. Bản tổng hợp CSV ở `<bucket>/reports/`.

### Kết quả mô hình, cho TV5

Bảng CSV và biểu đồ PNG cho E1, E4 (phía Spark), E6, EDA, đặt trong `report/figures/` của repo.

## TV2 giao

- Quyền vào cloud cho cả nhóm, bảng thông tin dự án ở trên.
- Cluster theo cấu hình E3 (1 máy, 2 worker, 3 worker) khi TV1 cần chạy E3.
- Biểu đồ E2, E3 và bảng chi phí thật, cho TV5.

## TV3 giao

- Link dashboard, bảng 5V và kết quả E4 phía kho SQL, cho TV5.

## TV4 giao

- Link web demo và kết quả E5 (p50, p95), cho TV5.

## Mọi người giao TV5

- Phần viết của mình trong báo cáo, trên Google Doc báo cáo.
