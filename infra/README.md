# infra/: TV2, cloud và hạ tầng

Đọc [README gốc](../README.md) và [luật chung](../CONTRIBUTING.md) trước. Thư mục này làm theo
luật chung; mục "Quy ước riêng" ở cuối chỉ bổ sung.

## Mục tiêu

Hạ tầng an toàn, rẻ, mọi người có đúng quyền. Trả lời bằng số: thêm dữ liệu, thêm máy thì
nhanh hơn bao nhiêu và tốn thêm bao nhiêu (E2, E3). Lập luận vì sao chọn PaaS cho phần xử lý.

## Việc phải làm

| Mã | Việc | Hạn | Xong khi |
| --- | --- | --- | --- |
| H1 | Tạo tài khoản AWS, gắn billing hoặc credit, cảnh báo ngân sách 25%, 50%, 90% | Ngay | Có thông tin trong `docs/handoff.md` |
| H2 | Tạo IAM user hoặc IAM Identity Center cho từng người, cấp quyền | Ngay | Mỗi người đăng nhập được console |
| H3 | Tạo bucket và các thư mục, điền bảng thông tin trong `docs/handoff.md` | Ngay | Cả nhóm biết tên bucket, region |
| H4 | Kiểm tra quota vCPU, chuẩn bị cấu hình cluster cho E3 | S10 | Quota đủ hoặc đã xin tăng |
| H5 | E2: phân tích thời gian theo cỡ dữ liệu từ số đo của TV1 | S10 | Có bảng và biểu đồ E2 |
| H6 | E3: dựng cluster 1 máy, 2 worker, 3 worker; tính speedup và chi phí | S10 | Có bảng speedup và chi phí mỗi lần chạy |
| H7 | Bảng chi phí thật và ước tính toàn dự án | S11 | Đã giao TV5 |
| H8 | So sánh IaaS (tự dựng Spark trên máy ảo) với PaaS (Spark dịch vụ) | S11 | Có bảng so sánh giá |
| H9 | Viết Architecture, service model, cost cho báo cáo | S11 | Đã giao TV5 |

## Làm trên hạ tầng

| Việc | Dịch vụ AWS |
| --- | --- |
| Tài khoản, billing, ngân sách | AWS account, AWS Budgets |
| Quyền | IAM (mỗi người một user hoặc dùng IAM Identity Center) |
| Bucket chung | S3 |
| Cluster cho E3 | EMR; Service Quotas để xem và xin tăng giới hạn vCPU |
| Đọc chi phí theo tag | Cost Explorer; bật tag `owner`, `exp` thành cost allocation tag |

Tag chỉ hiện trong Cost Explorer sau khi bật thành cost allocation tag, và chỉ tính cho chi phí
phát sinh từ lúc bật, nên bật ngay ở H1.

Quyền gợi ý cho từng người (đơn giản nhất là cho cả nhóm policy `PowerUserAccess` trong đồ án):

| Người | Cần quyền với |
| --- | --- |
| TV1 | Bucket (đọc ghi), Spark cluster, máy ảo |
| TV2 | Toàn quyền tài khoản, xem hoá đơn |
| TV3 | Bucket (đọc), kho SQL, dashboard |
| TV4 | Bucket (đọc), NoSQL, chạy container, kho image |
| TV5 | Chỉ xem |

## Nhận từ và giao cho

- Giao cả nhóm (đầu tiên): quyền vào cloud, bảng thông tin dự án trong `docs/handoff.md`.
- Giao TV1: cluster theo cấu hình E3, báo tên cluster trong nhóm chat.
- Nhận từ TV1: số đo thời gian ở `<bucket>/metrics/`.
- Giao TV5: biểu đồ E2, E3, bảng chi phí, phần viết; để trong `report/figures/`.

Định dạng chi tiết: [docs/handoff.md](../docs/handoff.md).

## Cấu trúc thư mục

```
infra/
├── setup/      lệnh hoặc script dựng tài khoản, quyền, bucket (ghi lại để người khác dựng lại được)
├── clusters/   cấu hình cluster cho E3
└── cost/       bảng chi phí, so sánh IaaS và PaaS, notebook phân tích E2, E3
```

## Cách chạy

TV2 điền khi có lệnh: dựng lại hạ tầng từ đầu theo thứ tự nào, mất khoảng bao lâu.

## Quy ước riêng

- Không ghi key, mật khẩu, số tài khoản ngân hàng vào thư mục này; chỉ ghi lệnh và tên tài nguyên.
- Thêm hoặc xoá quyền của ai thì báo người đó trong nhóm chat.
