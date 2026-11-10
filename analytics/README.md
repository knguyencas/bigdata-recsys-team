# analytics/: TV3, phân tích và dashboard

Đọc [README gốc](../README.md) và [luật chung](../CONTRIBUTING.md) trước. Thư mục này làm theo
luật chung; mục "Quy ước riêng" ở cuối chỉ bổ sung.

## Mục tiêu

Cho thấy dữ liệu bằng dashboard và bảng 5V. Trả lời bằng số câu hỏi E4: lưu dữ liệu dạng CSV hay
Parquet, truy vấn bằng Spark hay bằng kho SQL thì nhanh hơn, rẻ hơn. Làm trang web demo mà
người dùng nhìn thấy (phần frontend), gọi API của TV4.

## Việc phải làm

| Mã | Việc | Hạn | Xong khi |
| --- | --- | --- | --- |
| A1 | Tạo kho SQL và bảng đọc `raw/` (CSV) và `clean/` (Parquet) | S10 | Truy vấn được các bảng |
| A2 | E4 phía kho SQL: 3 truy vấn chung, mỗi câu 3 lần, ghi thời gian và dung lượng bị tính tiền | S10 | Có bảng E4 |
| A3 | Dashboard: sự kiện theo ngày, phễu xem, giỏ, mua, danh mục, thương hiệu | S10 | Link dashboard chạy trên dữ liệu thật |
| A4 | Bảng 5V có biểu đồ | S10 | Đã giao TV5 |
| A5 | Viết phần phân tích dữ liệu, lựa chọn lưu trữ cho báo cáo | S11 | Đã giao TV5 |
| A6 | Viết trang web demo (frontend), chi tiết ở [app/web/README.md](../app/web/README.md) | S10 | Trang hiển thị đúng gợi ý lấy từ API thật |

Ba truy vấn chung của E4 ghi trong [docs/experiments.md](../docs/experiments.md).

## Làm trên hạ tầng

| Việc | Dịch vụ AWS |
| --- | --- |
| Kho SQL đọc file trên S3 | Athena, bảng khai báo trong Glue Data Catalog (một bảng đọc CSV ở `raw/`, một bảng đọc Parquet ở `clean/`) |
| Số đo cho E4 | Thời gian chạy và data scanned trong lịch sử truy vấn Athena (Athena tính tiền theo dung lượng quét) |
| Dashboard | Quick Sight (trước đây tên là QuickSight), nguồn là bảng Athena |

Trước truy vấn lớn, chạy thử có `LIMIT` và xem data scanned. Quick Sight chỉ miễn phí trong
thời gian dùng thử (khoảng 30 ngày, kiểm tra lại trên trang giá của AWS), nên bật nó khi đã có
dữ liệu sạch, để còn hạn đến buổi 10 và buổi 11.

## Nhận từ và giao cho

- Nhận từ TV1: `<bucket>/clean/` và `<bucket>/reports/eda/`.
- Nhận từ TV2: quyền vào kho SQL và dashboard.
- Nhận từ TV4: định dạng API đã chốt (cho trang web).
- Giao TV4: file trang web trong `app/web/` để đóng gói.
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
