# Luật chung của repo

Mọi thành viên làm theo file này. README trong từng thư mục chỉ được bổ sung quy ước riêng
cho thư mục đó, không được đặt luật trái với file này. Muốn đổi luật chung thì mở pull
request sửa file này và cả nhóm đồng ý.

## 1. Thư mục

- Mỗi người chỉ thêm và sửa file trong thư mục của mình (xem bảng trong [README.md](README.md)).
- File chung (`README.md`, `CONTRIBUTING.md`, `docs/`, `.github/`, `.gitignore`) chỉ sửa qua pull
  request, và người bị ảnh hưởng phải đồng ý trước khi merge.
- Mỗi thư mục có README theo cùng một khung: Mục tiêu, Việc phải làm, Làm trên hạ tầng,
  Nhận từ và giao cho, Cấu trúc thư mục, Cách chạy, Quy ước riêng.
- Thư viện và phụ thuộc khai báo trong thư mục của mình (`requirements.txt`, `package.json`...),
  không dùng chung một file ở gốc repo, để hai người không sửa cùng một file.

## 2. Nhánh, commit, pull request

Lệnh Git cho từng bước: [docs/git-workflow.md](docs/git-workflow.md).

- Không push thẳng vào `main`.
- Mỗi việc một nhánh, tên `<mã việc>-<mô tả ngắn>`, ví dụ `A1-bigquery-table`, `S2-api`.
  Mã việc lấy từ sheet phân công.
- Commit message ngắn, mở đầu bằng mã việc: `A2: add E4 queries`.
- Mở pull request vào `main`, điền mẫu có sẵn, ít nhất một người khác xem rồi mới merge.
- Pull request chỉ đụng thư mục của mình. Cần sửa file chung thì tách thành pull request riêng.

## 3. Cloud và hạ tầng

- Cả nhóm dùng AWS. Thông tin tài khoản ghi trong [docs/handoff.md](docs/handoff.md).
- Account ID, region, tên bucket chỉ ghi ở `docs/handoff.md`. Code không ghi
  cứng các giá trị này; mỗi thư mục đọc chúng từ file cấu hình hoặc biến môi trường của mình.
- Mỗi người dùng tài khoản IAM của chính mình, không chia sẻ key hay mật khẩu.
- Mọi cluster, máy ảo, dịch vụ đều gắn tag `owner=<tv1..tv5>` và
  `exp=<mã thí nghiệm hoặc dev>`, để đọc hoá đơn theo người và theo thí nghiệm.
- Ai tạo cluster hoặc máy ảo thì người đó xoá ngay khi chạy xong.
- Thử trên dữ liệu nhỏ và cấu hình nhỏ trước, chạy toàn bộ dữ liệu sau.

## 4. Dữ liệu và bàn giao

- Không commit dữ liệu vào git. Dữ liệu nằm trên bucket chung.
- Mỗi người chỉ ghi vào phần bucket của mình (bảng "Ai ghi vào đâu" trong
  [docs/handoff.md](docs/handoff.md)), các phần khác chỉ đọc.
- Bàn giao giữa các phần chỉ qua bucket, đúng đường dẫn và định dạng trong `docs/handoff.md`.
  Giao xong thì báo trong nhóm chat và đổi trạng thái trong sheet.
- Muốn đổi đường dẫn hoặc định dạng đã thoả thuận: sửa `docs/handoff.md` qua pull request và
  được người nhận đồng ý trước.
- Hình đưa vào `report/figures/` phải nhỏ (PNG dưới 1 MB) và ghi nguồn trong tên file hoặc README.

## 5. Bí mật

- Không commit file key, `kaggle.json`, `.env`, mật khẩu, token. `.gitignore` đã chặn sẵn các
  tên phổ biến; tên khác thì tự kiểm tra trước khi commit.
- Lỡ commit một key thì báo TV2 để thu hồi key đó ngay, xoá khỏi git chưa đủ.

## 6. Chạy lại được

- Mục "Cách chạy" trong README thư mục phải đủ để người khác làm theo từ máy mới hoặc từ
  Cloud Shell: cần cài gì, chạy lệnh nào, theo thứ tự nào, ra kết quả gì.
- Cuối dự án TV5 làm theo toàn bộ README như người ngoài (việc R7). Chỗ nào không chạy được
  thì người phụ trách thư mục đó sửa.

## 7. Tiến độ và AI

- Mỗi người tự cập nhật cột Trạng thái của việc mình trong sheet phân công.
- Họp mỗi tuần, ghi biên bản vào [docs/meetings.md](docs/meetings.md).
- Phần nào có dùng AI thì ghi ngay vào [docs/ai-use-statement.md](docs/ai-use-statement.md).
- Ai viết code nào phải giải thích được code đó, vì giảng viên có thể hỏi bất kỳ ai.
