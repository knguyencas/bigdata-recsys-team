# app/: TV4, phục vụ gợi ý

Đọc [README gốc](../README.md) và [luật chung](../CONTRIBUTING.md) trước. Thư mục này làm theo
luật chung; mục "Quy ước riêng" ở cuối chỉ bổ sung.

## Mục tiêu

Gõ một `user_id` là thấy danh sách gợi ý trong dưới 1 giây. Đo độ trễ (E5) và giải thích vì sao
dùng cơ sở dữ liệu NoSQL cho việc tra theo khoá.

## Việc phải làm

| Mã | Việc | Hạn | Xong khi |
| --- | --- | --- | --- |
| S1 | Tạo cơ sở dữ liệu NoSQL, viết chương trình nạp file gợi ý (thử với file mẫu) | S10 | Nạp được file mẫu |
| S2 | Viết API trả gợi ý theo `user_id` | S10 | `user_id` lạ vẫn nhận danh sách phổ biến |
| S3 | Viết trang web demo | S10 | Trang chạy được |
| S4 | Đóng gói container, chạy trên cloud | S10 | Link web demo mở được |
| S5 | Nạp file gợi ý thật | S10 | Demo trả gợi ý thật |
| S6 | E5: đo độ trễ với 1.000 `user_id` ngẫu nhiên | S11 | Có p50, p95 |
| S7 | Viết phần Serving, NoSQL; chuẩn bị demo | S11 | Đã giao TV5 |

E5 ghi hai số: p50 là thời gian mà một nửa số lần hỏi nhanh hơn (người dùng bình thường chờ bao
lâu), p95 là thời gian mà 95% số lần hỏi nhanh hơn (5% chậm nhất chờ bao lâu).

## Làm trên hạ tầng

| Việc | GCP | AWS |
| --- | --- | --- |
| Cơ sở dữ liệu NoSQL | Firestore (chế độ Native) | DynamoDB |
| Kho image container | Artifact Registry (build bằng Cloud Build) | ECR |
| Chạy API và web | Cloud Run | App Runner |
| Đọc file gợi ý | Cloud Storage | S3 |

Nạp thử vài trăm dòng trước khi nạp cả file, vì cả hai bên đều tính tiền theo số lượt ghi.

## Nhận từ và giao cho

- Nhận từ TV1: `<bucket>/handoff/`, file mẫu trước, file thật sau. Định dạng một dòng: xem
  [docs/handoff.md](../docs/handoff.md).
- Nhận từ TV2: quyền vào NoSQL, kho image, dịch vụ chạy container.
- Giao TV5: link web demo, bảng p50, p95, phần viết; để trong `report/figures/`.

## Cấu trúc thư mục

```
app/
├── loader/     chương trình nạp file gợi ý vào NoSQL
├── api/        API trả gợi ý theo user_id
├── web/        trang web demo
└── loadtest/   script đo độ trễ E5 và kết quả
```

## Cách chạy

TV4 điền khi có code: chạy API ở máy thế nào, nạp dữ liệu thế nào, deploy lên cloud thế nào,
đo E5 thế nào.

## Quy ước riêng

- `user_id` không có trong dữ liệu thì trả danh sách của dòng `__popular__`, không trả lỗi.
- Link web demo ghi vào bảng thông tin trong `docs/handoff.md` khi đã chạy.
