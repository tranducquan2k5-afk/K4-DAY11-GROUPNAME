# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm

- Khóa/lớp: K4 VinAI
- Tên nhóm: GROUPNAME
- Repo Public: https://github.com/tranducquan2k5-afk/K4-DAY11-GROUPNAME
- Máy giữ hồ sơ chính / người quản lý: tranducquan2k5-afk (Trần Đức Quân)
- Slice chung lấy từ mode.json: B2-mid
- Tên định danh vai A dùng cho --self: DuongLong-205
- Kênh trao đổi nội bộ: Discord / Zalo Group
- Đại diện nộp (vai C): Trần Đức Quân - 2A202602260

## 2. Bảng phân vai

| Vai | Thành viên (Họ tên, MSSV, GitHub) | Nhiệm vụ chính | Đầu ra cần kiểm tra trước khi nộp |
|---|---|---|---|
| **A — Gán nhãn & sửa lỗi** | Dương Văn Long — 2A202602156 — @DuongLong-205 | Gán nhãn slice chính B2-mid, self-QC, sửa ca rework | `submission/r1_craft/`, `submission/rework/` |
| **B — QA độc lập** | Nguyễn Quang Tùng — 2A202603022 — @nqtung30624-ship-it | Rà soát độc lập, đối chiếu rule, lưu bằng chứng ảnh | `submission/r2_qa/qa_review.md`, `submission/screenshots/` |
| **C — Chẩn đoán & kế hoạch** | Trần Đức Quân — 2A202602260 — @tranducquan2k5-afk | Điều phối, chạy compare/model/quality, lập kế hoạch sampling/gold set | `submission/r3_diag/`, `submission/45_*.csv`, `submission/46_*.md` |

## 3. Xác nhận bàn giao nội bộ

- [x] **Vai A xác nhận:** Đã export bản cuối từ CVAT, chạy lock r1_craft, ghi nhận self-QC, và sửa đúng các ca rework thống nhất.
- [x] **Vai B xác nhận:** Đã hoàn thành QA độc lập trên bản khóa của A, có đủ ảnh minh chứng trong screenshots/, và kiểm tra lại sau rework.
- [x] **Vai C xác nhận:** Đã chạy triage, status, check đạt exit code 0, không còn TODO trong submission/, và sẵn sàng nộp repo.
