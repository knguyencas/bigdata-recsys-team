# Hướng dẫn Git cho nhóm

Cách đưa code lên repo chung mà không đè lên việc của người khác. Luật chung nằm ở
[CONTRIBUTING.md](../CONTRIBUTING.md); file này chỉ là các bước và lệnh.

Repo: https://github.com/knguyencas/bigdata-recsys-team (private, chỉ người được mời mới vào được).

## Lần đầu: mỗi người làm một lần

1. Có tài khoản GitHub, gửi tên GitHub cho TV1 để được mời vào repo.
2. Chấp nhận lời mời: mở email GitHub gửi, hoặc vào
   https://github.com/knguyencas/bigdata-recsys-team/invitations rồi bấm Accept.
3. Cài Git (https://git-scm.com), rồi khai báo tên và email một lần:

   ```bash
   git config --global user.name "Ten Cua Ban"
   git config --global user.email "email-dung-cho-github@example.com"
   ```

4. Tải repo về máy:

   ```bash
   git clone https://github.com/knguyencas/bigdata-recsys-team.git
   cd bigdata-recsys-team
   ```

   Lần đầu push, Git trên Windows và macOS mở cửa sổ đăng nhập GitHub. Trên AWS
   CloudShell thì đăng nhập bằng personal access token: GitHub > Settings >
   Developer settings > Personal access tokens, quyền `repo`; dán token khi Git hỏi mật khẩu.

## Mỗi lần làm một việc

Một việc trong sheet = một nhánh = một pull request. Ví dụ dưới đây là việc A1 của TV3.

1. Lấy bản mới nhất của `main`:

   ```bash
   git switch main
   git pull
   ```

2. Tạo nhánh mới, tên bắt đầu bằng mã việc:

   ```bash
   git switch -c A1-bigquery-table
   ```

3. Sửa hoặc thêm file **trong thư mục của mình** (`analytics/` với TV3).
4. Xem mình đã đổi gì:

   ```bash
   git status
   git diff
   ```

5. Lưu thay đổi (commit), message bắt đầu bằng mã việc:

   ```bash
   git add analytics/
   git commit -m "A1: create tables over raw and clean"
   ```

6. Đẩy nhánh lên GitHub:

   ```bash
   git push -u origin A1-bigquery-table
   ```

7. Mở pull request: vào trang repo, bấm nút "Compare & pull request" vừa hiện ra
   (hoặc tab Pull requests > New pull request), base là `main`, điền mẫu có sẵn, chọn một
   người review ở cột bên phải.
8. Người review mở tab "Files changed", góp ý nếu cần, rồi bấm Review changes > Approve.
9. Được duyệt thì bấm "Squash and merge". Xoá nhánh trên GitHub khi được hỏi.
10. Về máy, dọn nhánh cũ:

    ```bash
    git switch main
    git pull
    git branch -d A1-bigquery-table
    ```

## Các tình huống hay gặp

**Pull request chưa merge mà muốn sửa thêm.** Sửa tiếp trên cùng nhánh, `git add`,
`git commit`, `git push`. Pull request tự cập nhật, không cần mở cái mới.

**`main` đã có thay đổi mới trong lúc mình đang làm.** Lấy về và gộp vào nhánh của mình:

```bash
git switch A1-bigquery-table
git pull origin main
```

**Xung đột (conflict).** Mỗi người sửa thư mục riêng nên hiếm gặp. Nếu có, Git báo tên
file; mở file, giữ đoạn đúng, xoá các dòng `<<<<<<<`, `=======`, `>>>>>>>`, rồi:

```bash
git add <file>
git commit
git push
```

Không chắc thì hỏi người sửa file đó trước khi gộp.

**Lỡ commit nhầm file (dữ liệu, file lớn) nhưng chưa push.** `git reset --soft HEAD~1` để
bỏ commit cuối mà giữ nguyên file, rồi bỏ file đó ra và commit lại.

**Lỡ commit key hay mật khẩu.** Báo TV2 thu hồi key đó ngay. Xoá khỏi git là chưa đủ, vì
key vẫn nằm trong lịch sử.

## Không được làm

- Push thẳng vào `main`, hoặc `git push --force` lên `main`.
- Sửa thư mục của người khác trong pull request của mình.
- Commit dữ liệu, file key, `kaggle.json`, `.env`.

## Cài đặt repo (chỉ chủ repo làm)

- **Mời thành viên:** Settings > Collaborators > Add people, nhập tên GitHub từng người.
- **Gộp gọn và dọn nhánh:** Settings > General > Pull Requests: bật "Allow squash merging"
  và "Automatically delete head branches".
- **Khoá nhánh `main`** (bắt buộc pull request và một người duyệt): Settings > Branches >
  Add branch protection rule, tên nhánh `main`, bật "Require a pull request before merging"
  và "Require approvals". Với repo private của tài khoản cá nhân, mục này cần GitHub Pro
  (miễn phí cho sinh viên qua GitHub Student Developer Pack). Chưa bật được thì cả nhóm
  tự giữ luật "không push thẳng vào main".
