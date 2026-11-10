# Hệ thống gợi ý sản phẩm trên cloud

Repo chung của nhóm cho đồ án môn Big Data in Cloud Computing.

- Dữ liệu: eCommerce Behavior Data from Multi Category Store (Kaggle), tháng 10 và 11 năm 2019,
  khoảng 110 triệu sự kiện, 14 GB.
- Bài toán chính: gợi ý top-K sản phẩm cho từng người dùng từ hành vi xem, thêm giỏ, mua.
- Bài toán phụ: dự đoán một phiên có giỏ hàng có thành đơn hay không.
- Kết quả cuối: hệ thống chạy hết trên cloud, web demo trả gợi ý theo `user_id`, báo cáo
  trả lời bằng số đo: mô hình nào tốt nhất, tốn bao nhiêu thời gian và tiền, thêm dữ liệu
  hoặc thêm máy thì thay đổi ra sao.

Tài liệu của nhóm:

- Mục tiêu, phân tích, quy trình: [Google Doc đồ án](https://docs.google.com/document/d/11sDP2XD2eimJkBBHSuptmWKyq7LSPOZwkCbg_s4KeEI/edit)
- Ai làm gì, tiến độ, luồng bàn giao: [Sheet phân công](https://docs.google.com/spreadsheets/d/1ELORh6tE51ZjAWlIqllZc1f8X0tbDABss7m3DygP2HQ/edit)
- Luật chung của repo: [CONTRIBUTING.md](CONTRIBUTING.md)
- Ai giao gì cho ai, định dạng file: [docs/handoff.md](docs/handoff.md)
- Cách đưa code lên repo (lệnh Git từng bước): [docs/git-workflow.md](docs/git-workflow.md)

## Hệ thống gồm những phần nào

```
Kaggle (2 file CSV)
   │  TV1 tải lên
   ▼
Kho lưu trữ chung (bucket): raw/ → clean/ → recs/ → handoff/, metrics/, reports/
   │  TV1 xử lý bằng Spark: làm sạch, phân tích, train mô hình
   │
   ├──► TV3: kho SQL + dashboard (đọc clean/)
   ├──► TV4: cơ sở dữ liệu NoSQL + API, phần backend (đọc handoff/)
   │       └── TV3: trang web demo, phần frontend (gọi API của TV4)
   └──► TV2: đo thời gian và chi phí (đọc metrics/, hoá đơn)
                     │
                     ▼
            TV5: báo cáo, slide (nhận kết quả của mọi người)

TV2 dựng và quản lý toàn bộ hạ tầng phía dưới: tài khoản, quyền, bucket, cluster.
```

## Cloud: AWS

Nhóm đã chốt dùng AWS. TV2 ghi Account ID, region và tên bucket vào
[docs/handoff.md](docs/handoff.md). Các dịch vụ dùng trong đồ án:

| Dùng để | Dịch vụ AWS |
| --- | --- |
| Lưu file (bucket) | S3 (`s3://`) |
| Máy ảo | EC2 |
| Chạy Spark | EMR |
| Truy vấn SQL trên file | Athena (kèm Glue Data Catalog) |
| Dashboard | Quick Sight (trước đây tên là QuickSight) |
| Cơ sở dữ liệu NoSQL | DynamoDB |
| Chạy API và trang web bằng container | ECS Express Mode trên Fargate (image ở ECR) |
| Quyền truy cập | IAM |
| Ngân sách, chi phí | AWS Budgets, Cost Explorer theo tag |

Không dùng App Runner: AWS ngừng nhận khách hàng mới cho App Runner từ 30/4/2026 và khuyên
chuyển sang ECS Express Mode.

Train SASRec dùng Colab GPU, nằm ngoài AWS, đọc và ghi file trên S3.

## Thư mục và người phụ trách

| Thư mục | Người | Vai trò | Hướng dẫn |
| --- | --- | --- | --- |
| `infra/` | TV2 | Cloud và hạ tầng | [infra/README.md](infra/README.md) |
| `pipeline/` | TV1 | Data Engineer, Data Analyst, ML/DL | [pipeline/README.md](pipeline/README.md) |
| `analytics/` | TV3 | Phân tích và dashboard | [analytics/README.md](analytics/README.md) |
| `app/` (trừ `app/web/`) | TV4 | Backend phục vụ gợi ý (NoSQL, API, đưa lên cloud) | [app/README.md](app/README.md) |
| `app/web/` | TV3 | Trang web demo (frontend) | [app/web/README.md](app/web/README.md) |
| `report/` | TV5 | Báo cáo và thuyết trình | [report/README.md](report/README.md) |
| `docs/` | Cả nhóm | Bàn giao, thí nghiệm, AI-use, biên bản họp | [docs/handoff.md](docs/handoff.md) |

Mỗi người chỉ sửa trong thư mục của mình. README của từng thư mục nói việc của người đó;
luật chung nằm ở [CONTRIBUTING.md](CONTRIBUTING.md). Nếu README thư mục và CONTRIBUTING.md
nói khác nhau thì làm theo CONTRIBUTING.md.

## Thứ tự chạy toàn hệ thống

Làm đúng thứ tự này, mỗi bước theo README của thư mục tương ứng:

1. `infra/`: dựng tài khoản, quyền, bucket; ghi thông tin vào `docs/handoff.md`.
2. `pipeline/`: tải dữ liệu, làm sạch, phân tích, train mô hình, xuất file bàn giao.
3. `analytics/` và `app/`: chạy song song, dùng file do `pipeline/` giao. Trong `app/`, TV4 làm
   API, TV3 làm trang web gọi API đó, theo định dạng API chốt trong `docs/handoff.md`.
4. `infra/`: đo thời gian và chi phí cho E2, E3 từ số đo do `pipeline/` ghi.
5. `report/`: gom bảng số, biểu đồ, phần viết; chạy lại cả repo theo README như người ngoài.

## Bắt đầu cho thành viên mới

1. Nhận lời mời vào repo, `git clone` về máy hoặc về AWS CloudShell.
   Các lệnh cụ thể: [docs/git-workflow.md](docs/git-workflow.md).
2. Đọc README này và [CONTRIBUTING.md](CONTRIBUTING.md).
3. Nhận quyền vào cloud từ TV2; thông tin dự án nằm trong [docs/handoff.md](docs/handoff.md).
4. Mở README trong thư mục của mình, làm theo danh sách việc. Mã việc khớp với sheet phân công.
