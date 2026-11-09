# report/: TV5, báo cáo và thuyết trình

Đọc [README gốc](../README.md) và [luật chung](../CONTRIBUTING.md) trước. Thư mục này làm theo
luật chung; mục "Quy ước riêng" ở cuối chỉ bổ sung.

## Mục tiêu

Báo cáo tiếng Anh 8 đến 12 trang đúng rubric, slide 15 phút có đủ 5 người nói, AI-use statement
đầy đủ, và người ngoài chạy lại được hệ thống theo README.

## Việc phải làm

| Mã | Việc | Hạn | Xong khi |
| --- | --- | --- | --- |
| R1 | Proposal tiếng Anh, nộp | S7 | Đã nộp |
| R2 | Khung báo cáo theo rubric, chia đề mục cho từng người | S10 | Mỗi người biết mục mình viết |
| R3 | Vẽ sơ đồ kiến trúc | S10 | Cả nhóm đồng ý |
| R4 | Ghép và sửa báo cáo 8 đến 12 trang | S11 | Có báo cáo PDF |
| R5 | Làm slide, tổ chức tập thuyết trình | S11 | Tập ít nhất một lần đủ 15 phút |
| R6 | Gom AI-use statement | S11 | Đủ 5 người |
| R7 | Làm theo README từ đầu như người ngoài, ghi chỗ vướng | S11 | Không còn chỗ vướng |

R7: clone repo, làm đúng từng bước trong README gốc rồi README từng thư mục. Bước nào thiếu,
lệnh nào lỗi, chỗ nào khó hiểu thì ghi lại và báo người phụ trách thư mục đó sửa.

## Làm trên hạ tầng

Chỉ cần quyền xem: mở bucket, dashboard, console để chụp hình cho báo cáo. Với R7 thì cần
Cloud Shell (GCP) hoặc CloudShell (AWS) để chạy lệnh.

## Nhận từ và giao cho

- Nhận từ TV1: bảng kết quả, biểu đồ E1, E4, E6; mục Dataset, Method, Results.
- Nhận từ TV2: biểu đồ E2, E3, bảng chi phí; mục Architecture, service model, cost.
- Nhận từ TV3: link dashboard, bảng E4, bảng 5V; mục phân tích dữ liệu và lưu trữ.
- Nhận từ TV4: link web demo, kết quả E5; mục Serving, NoSQL.
- Giao cả nhóm: bản báo cáo và slide để duyệt, lịch tập.

## Cấu trúc thư mục

```
report/
├── figures/    hình và bảng mọi người giao (PNG, CSV), đặt tên theo mã thí nghiệm, ví dụ e1_models.png
└── slides/     file slide hoặc link
```

Bản thảo báo cáo viết chung trên Google Doc; link ghi ở đây khi tạo.

## Cách chạy

Không có code. Ghi ở đây link bản thảo báo cáo, link slide, và kết quả R7.

## Quy ước riêng

- Hình đưa vào `figures/` phải ghi người giao và nguồn số liệu (thí nghiệm nào, lần chạy nào).
