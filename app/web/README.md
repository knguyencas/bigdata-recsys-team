# app/web/: TV3, trang web demo (frontend)

Đọc [README gốc](../../README.md) và [luật chung](../../CONTRIBUTING.md) trước. Việc chính của
TV3 nằm ở [analytics/README.md](../../analytics/README.md); file này chỉ nói phần trang web.

## Mục tiêu

Trang web mà người dùng và giảng viên nhìn thấy khi demo: nhập một `user_id`, bấm tìm, hiện ra
danh sách sản phẩm gợi ý. Trang web chỉ hiển thị; dữ liệu lấy từ API của TV4.

## Việc phải làm

| Mã | Việc | Hạn | Xong khi |
| --- | --- | --- | --- |
| A6 | Viết trang web: ô nhập `user_id`, nút tìm, thẻ sản phẩm (danh mục, thương hiệu, giá), thông báo khi API không trả lời; ghi rõ khi kết quả là danh sách phổ biến cho người dùng lạ | Buổi 10 | Trang hiển thị đúng gợi ý lấy từ API thật |

Thứ tự làm:
1. Cùng TV4 chốt định dạng API (việc S3 của TV4), ghi trong `docs/handoff.md`.
2. Làm trang web trên dữ liệu giả đúng định dạng đó, chưa cần API thật.
3. Khi TV4 có API, đổi sang gọi API thật, kiểm tra cả `user_id` có và `user_id` lạ.
4. Giao file cho TV4 đóng gói chung container (TV4 làm việc S4).

## Làm trên hạ tầng

Không cần tạo dịch vụ cloud riêng. Trang web chạy chung container với API của TV4 (Cloud Run
trên GCP hoặc App Runner trên AWS), cùng một địa chỉ: trang web ở `/`, API ở `/api/...`.

## Nhận từ và giao cho

- Nhận từ TV4: định dạng API đã chốt, rồi địa chỉ API thật để thử.
- Giao TV4: các file trong thư mục này để đóng gói.
- Giao TV5: ảnh chụp trang web cho báo cáo; nói phần giao diện khi demo.

## Cấu trúc thư mục

```
app/web/
├── index.html   trang chính
├── app.js       gọi API, hiển thị kết quả
├── style.css    giao diện
└── mock.json    dữ liệu giả đúng định dạng API, dùng khi chưa có API thật
```

Dùng HTML, CSS, JavaScript thuần là đủ cho demo; muốn dùng thư viện khác thì báo TV4 trước, vì
TV4 phải đóng gói được.

## Cách chạy

TV3 điền khi có code: mở trang ở máy với dữ liệu giả thế nào, đổi sang API thật thế nào.

## Quy ước riêng

- Gọi API bằng đường dẫn tương đối `/api/recommend/<user_id>`, không ghi cứng địa chỉ máy chủ.
