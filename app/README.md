# app (TV4)

Phần phục vụ gợi ý cho người dùng. Code nạp dữ liệu, API, trang web, script đo tải để ở đây.

- Tạo cơ sở dữ liệu NoSQL (Firestore chế độ Native), viết chương trình nạp file gợi ý
  từ `gs://<bucket>/handoff/` vào, mỗi người dùng một document.
- Viết API: nhận `user_id`, trả danh sách sản phẩm; người dùng không có thì trả danh sách `__popular__`.
- Trang web demo: ô nhập `user_id`, hiện danh sách sản phẩm.
- Đóng gói và chạy trên Cloud Run.
- E5: đo độ trễ với 1.000 người dùng ngẫu nhiên, ghi p50 và p95.

Đầu vào và đầu ra: xem `docs/handoff.md`.
