# Hệ thống gợi ý sản phẩm trên Google Cloud

Repo chung của nhóm cho đồ án môn Big Data in Cloud Computing. Dữ liệu: eCommerce
Behavior Data from Multi Category Store (Kaggle), tháng 10 và 11 năm 2019, khoảng
110 triệu sự kiện. Bài toán chính: gợi ý top-K sản phẩm cho từng người dùng. Bài toán
phụ: dự đoán một phiên có giỏ hàng có thành đơn hay không.

Mục tiêu, phân tích và phân công chi tiết nằm trong Google Doc của nhóm. Tiến độ từng
việc nằm trong Google Sheet phân công.

## Thư mục và người phụ trách

Mỗi người chỉ sửa code trong thư mục của mình. Muốn đổi thứ người khác đang dùng
(đường dẫn, định dạng file bàn giao) thì sửa `docs/handoff.md` qua pull request và
báo người liên quan.

| Thư mục | Người phụ trách | Chứa gì |
| --- | --- | --- |
| `pipeline/` | Cas | Spark trên Dataproc: làm sạch, EDA, chia tập, mô hình gợi ý (Popularity, Item-CF, ALS), dự đoán mua hàng, SASRec |
| `infra/` | TV2 | Dựng project GCP, IAM, bucket, quota, cấu hình cluster cho E3, theo dõi chi phí |
| `analytics/` | TV3 | BigQuery (bảng, truy vấn), E4 phía BigQuery, dashboard Looker Studio, bảng 5V |
| `app/` | TV4 | Nạp gợi ý vào cơ sở dữ liệu NoSQL, API, trang web demo, đo độ trễ E5 |
| `report/` | TV5 | Báo cáo tiếng Anh, slide, hình vẽ dùng trong báo cáo |
| `docs/` | Cả nhóm | Mô tả bàn giao, kế hoạch thí nghiệm, AI-use statement, biên bản họp |

## Quy tắc làm việc

- Không push thẳng vào `main`. Mỗi việc một nhánh, đặt tên `<mã việc>-<mô tả ngắn>`,
  ví dụ `A1-bigquery-table`. Mã việc lấy từ sheet phân công.
- Mở pull request vào `main`, ít nhất một người khác xem rồi mới merge.
- Không commit dữ liệu, file key, `kaggle.json`, file `.env`. Dữ liệu nằm trên Cloud
  Storage, không nằm trong repo.
- Bàn giao giữa các phần qua bucket, theo đúng đường dẫn trong `docs/handoff.md`.
  Bàn giao xong thì báo trong nhóm chat và cập nhật trạng thái trong sheet.
- Ghi lại phần nào có dùng AI ngay khi làm, vào `docs/ai-use-statement.md`.
- Ai viết code nào phải giải thích được code đó, vì giảng viên có thể hỏi bất kỳ ai.

## Bắt đầu

1. Nhận lời mời vào repo, rồi `git clone` về máy hoặc về Cloud Shell.
2. Nhận quyền vào project GCP từ TV2. Tên project, region và bucket ghi trong
   `docs/handoff.md`.
3. Đọc phần việc của mình trong Google Doc, mở README trong thư mục của mình.
