# app/: TV4, backend phục vụ gợi ý

Đọc [README gốc](../README.md) và [luật chung](../CONTRIBUTING.md) trước. Thư mục này làm theo
luật chung; mục "Quy ước riêng" ở cuối chỉ bổ sung.

Thư mục `app/web/` bên trong là của TV3 (trang web, phần frontend), xem
[app/web/README.md](web/README.md). TV4 không sửa `app/web/`, chỉ đóng gói nó.

## Mục tiêu

Gõ một `user_id` trên trang web là thấy danh sách gợi ý trong dưới 1 giây. Đo độ trễ (E5) và
giải thích vì sao dùng cơ sở dữ liệu NoSQL cho việc tra theo khoá.

API không chạy mô hình. TV1 chạy mô hình trên cụm Spark một lần, tính sẵn top-20 sản phẩm cho
từng người dùng và xuất ra file. API chỉ tra kết quả đã tính sẵn đó. Làm vậy vì cụm Spark bật
liên tục thì rất tốn tiền, và chạy mô hình mỗi lần hỏi mất vài phút, còn tra kết quả có sẵn chỉ
mất vài chục mili giây.

## Việc phải làm

| Mã | Việc | Hạn | Xong khi |
| --- | --- | --- | --- |
| S1 | Tạo cơ sở dữ liệu NoSQL, viết chương trình đọc file gợi ý từ bucket và ghi mỗi người dùng một bản ghi (thử với file mẫu) | Buổi 10 | Nạp được file mẫu, đọc lại đúng |
| S2 | Viết API: `GET /api/recommend/{user_id}`, người dùng lạ trả sản phẩm phổ biến; `GET /api/health`; có kiểm thử | Buổi 10 | Cả `user_id` có và `user_id` lạ đều trả kết quả đúng |
| S3 | Chốt định dạng API cùng TV3, ghi vào `docs/handoff.md` | Buổi 7 | TV3 làm trang web trên dữ liệu giả được |
| S4 | Đóng gói API và trang web của TV3 vào một container, đưa lên cloud, cấp quyền đọc NoSQL | Buổi 10 | Link web demo mở được |
| S5 | Nạp file gợi ý thật của TV1 | Buổi 10 | Demo trả gợi ý thật |
| S6 | E5: gửi 1.000 lần hỏi với `user_id` ngẫu nhiên, ghi p50, p95 | Buổi 11 | Có p50, p95 |
| S7 | Viết phần Serving, NoSQL; chuẩn bị demo phần backend | Buổi 11 | Đã giao TV5 |

E5 ghi hai số: p50 là thời gian mà một nửa số lần hỏi nhanh hơn (người dùng bình thường chờ bao
lâu), p95 là thời gian mà 95% số lần hỏi nhanh hơn (5% chậm nhất chờ bao lâu).

## Làm trên hạ tầng

| Việc | Dịch vụ AWS |
| --- | --- |
| Cơ sở dữ liệu NoSQL | DynamoDB (chế độ on-demand, khoá chính `user_id`) |
| Kho image container | ECR |
| Chạy API và trang web | ECS Express Mode trên Fargate |
| Đọc file gợi ý | S3 |

Không dùng App Runner: AWS ngừng nhận khách hàng mới cho App Runner từ 30/4/2026 và khuyên
chuyển sang ECS Express Mode. ECS Express Mode tự tạo service Fargate, load balancer có HTTPS
và tự co giãn. Load balancer và Fargate tính tiền theo giờ kể cả khi không ai dùng, nên chỉ bật
khi thử, demo và đo E5, xong thì xoá service.

Nạp thử vài trăm dòng trước khi nạp cả file, vì DynamoDB tính tiền theo số lượt ghi.

## Nhận từ và giao cho

- Nhận từ TV1: `<bucket>/handoff/`, file mẫu trước, file thật sau.
- Nhận từ TV3: file trang web trong `app/web/`.
- Nhận từ TV2: quyền vào NoSQL, kho image, dịch vụ chạy container.
- Giao TV3: định dạng API đã chốt, trong `docs/handoff.md`.
- Giao TV5: link web demo, bảng p50, p95, phần viết; để trong `report/figures/`.

Định dạng chi tiết: [docs/handoff.md](../docs/handoff.md).

## Cấu trúc thư mục

```
app/
├── loader/     chương trình nạp file gợi ý vào NoSQL (TV4)
├── api/        API trả gợi ý theo user_id, phục vụ luôn file của web/ (TV4)
├── web/        trang web demo (TV3)
└── loadtest/   script đo độ trễ E5 và kết quả (TV4)
```

API và trang web chạy chung một container, cùng một địa chỉ: trang web ở `/`, API ở `/api/...`.
Nhờ vậy trang web gọi API không bị trình duyệt chặn vì khác tên miền.

## Cách chạy

TV4 điền khi có code: chạy API ở máy thế nào, nạp dữ liệu thế nào, deploy lên cloud thế nào,
đo E5 thế nào.

## Quy ước riêng

- `user_id` không có trong dữ liệu thì trả danh sách của dòng `__popular__` với
  `"source": "popular"`, không trả lỗi.
- Đổi định dạng API thì sửa `docs/handoff.md` qua pull request và được TV3 đồng ý trước.
- Link web demo ghi vào bảng thông tin trong `docs/handoff.md` khi đã chạy.
