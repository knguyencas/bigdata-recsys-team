# pipeline/: TV1, dữ liệu và mô hình

Đọc [README gốc](../README.md) và [luật chung](../CONTRIBUTING.md) trước. Thư mục này làm theo
luật chung; mục "Quy ước riêng" ở cuối chỉ bổ sung.

## Mục tiêu

Biến 14 GB click thô thành dữ liệu sạch và danh sách top-20 gợi ý cho mỗi người dùng. Chứng minh
bằng số mô hình nào tốt hơn và vì sao (E1, E6). Cung cấp số liệu thật cho bảng 5V và proposal.

## Việc phải làm

| Mã | Việc | Hạn | Xong khi |
| --- | --- | --- | --- |
| C1 | Viết khung pipeline Spark và test | Ngay | Xong |
| C2 | Viết code SASRec | Ngay | Xong |
| C3 | Viết bản nháp proposal | Ngay | Xong |
| C4 | Đẩy code lên repo nhóm, mời thành viên | Ngay | Cả nhóm clone được repo |
| C5 | Đưa dataset Kaggle lên bucket | S7 | `raw/` có đủ 2 file CSV |
| C6 | Chạy thử pipeline trên dữ liệu nhỏ | S7 | Chạy hết các bước không lỗi |
| C7 | Làm sạch toàn bộ dữ liệu | S7 | `clean/` đã giao TV3 |
| C8 | EDA, lấy subset 10%, chia train/test | S7 | Bảng EDA đã giao TV3 |
| C9 | Gửi TV4 file gợi ý mẫu | S7 | TV4 đọc được file |
| C10 | Cùng TV5 đưa số liệu thật vào proposal | S7 | Proposal có số thật |
| C11 | Train Popularity, Item-CF, ALS; thí nghiệm E1 | S10 | Có bảng và biểu đồ E1 |
| C12 | Xuất file gợi ý thật cho TV4 | S10 | TV4 nạp được file |
| C13 | E4 phía Spark; E6 dự đoán mua hàng | S10 | Có bảng E4 phía Spark và bảng E6 |
| C14 | Chạy job E2, E3 trên cluster TV2 dựng | S10 | Đủ cấu hình, mỗi cấu hình 3 lần |
| C15 | Train SASRec trên Colab, chấm điểm | S11 | SASRec có trong bảng E1 |
| C16 | Gom kết quả, vẽ biểu đồ | S11 | Đã giao TV5 |
| C17 | Viết Dataset, Method, Results | S11 | Đã giao TV5 |

## Làm trên hạ tầng

| Việc | GCP | AWS |
| --- | --- | --- |
| Tải dữ liệu Kaggle | Compute Engine (máy ảo tạm, xoá sau) | EC2 (máy ảo tạm, xoá sau) |
| Lưu dữ liệu | Cloud Storage | S3 |
| Chạy Spark | Dataproc | EMR |
| Train SASRec | Colab GPU | Colab GPU |

Code Spark chạy được trên cả hai bên. Phần phải đổi nếu nhóm chọn AWS: script tạo cluster và
gửi job (Dataproc sang EMR), và tiền tố đường dẫn (`gs://` sang `s3://`).

## Nhận từ và giao cho

- Nhận từ TV2: quyền, tên bucket, region; cluster theo cấu hình E3.
- Giao TV3: `<bucket>/clean/` (Parquet) và `<bucket>/reports/eda/` (CSV).
- Giao TV4: `<bucket>/handoff/` (JSON lines), file mẫu trước, file thật sau.
- Giao TV2: `<bucket>/metrics/` (số đo thời gian mỗi lần chạy).
- Giao TV5: bảng kết quả và biểu đồ E1, E4, E6 trong `report/figures/`; phần viết.

Định dạng chi tiết: [docs/handoff.md](../docs/handoff.md).

## Cấu trúc thư mục

```
pipeline/
├── src/        code Spark: làm sạch, EDA, chia tập, mô hình, đánh giá, xuất bàn giao
├── models/     SASRec (PyTorch, chạy trên Colab)
├── configs/    cấu hình chạy ở máy và trên cloud (tên bucket đọc từ đây)
├── scripts/    tạo cluster, gửi job, chạy thí nghiệm
└── tests/      kiểm thử
```

Code hiện có (viết cho GCP) sẽ được chuyển vào đây ở việc C4.

## Cách chạy

TV1 điền khi đưa code vào: cài gì, chạy thử trên dữ liệu mẫu thế nào, chạy trên cloud theo
thứ tự bước nào, mỗi bước ghi ra đâu.

## Quy ước riêng

- Mỗi lần chạy ghi một bản số đo vào `<bucket>/metrics/` để E2, E3 và báo cáo dùng lại.
- Chạy thí nghiệm thì gắn nhãn `exp=<mã thí nghiệm>` lên cluster.
