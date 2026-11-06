# infra (TV2)

Hạ tầng Google Cloud cho cả nhóm. Code, lệnh `gcloud` và ghi chú cấu hình để ở đây.

- Tạo project, gắn billing (free trial hoặc credit), đặt cảnh báo ngân sách.
- Cấp quyền IAM cho từng thành viên theo việc của họ.
- Bật các dịch vụ: Cloud Storage, Dataproc, Compute Engine, BigQuery, Firestore, Cloud Run.
- Tạo bucket và các thư mục chính (`raw/`, `clean/`, `handoff/`, `metrics/`, `reports/`).
- Kiểm tra quota vCPU cho E3, chuẩn bị cấu hình cluster 1 máy, 2 worker, 3 worker.
- Quy ước nhãn chi phí, đọc hoá đơn theo nhãn, làm bảng chi phí thật.
- So sánh IaaS (tự dựng Spark trên VM) với PaaS (Dataproc) bằng bảng giá.

Đầu vào và đầu ra: xem `docs/handoff.md`.
