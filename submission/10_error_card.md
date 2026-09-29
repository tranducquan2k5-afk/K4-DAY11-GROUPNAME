# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B2 | MISSING | 8 |
| center | B2 | SPURIOUS | 7 |
| edge | B2 | BOX_GEOMETRY | 2 |
| edge | B2 | MISSING | 3 |
| edge | B2 | SPURIOUS | 1 |
| mid | B2 | ATTRIBUTE | 4 |
| mid | B2 | BOX_GEOMETRY | 3 |
| mid | B2 | SPURIOUS | 8 |
| mid | B2 | WRONG_CLASS | 1 |

## Top defects
- SPURIOUS: 16 (ví dụ frame adasind_060000.jpg)
- MISSING: 11 (ví dụ frame adasind_060000.jpg)
- BOX_GEOMETRY: 5 (ví dụ frame adasind_060000.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- **Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy:**
  - *Lỗi SPURIOUS vượt trội (16 ca):* Nguyên nhân chính là do độ lệch miền của mô hình (`why: E4_model_domain`). Mô hình YOLO26m gốc được tiền huấn luyện trên ảnh pinhole phối cảnh phẳng, không nhận biết class đặc thù `ThreeWheeler` nên sinh ra các box `Truck`/`Car` đè lên nhau (ví dụ frame `adasind_060000.jpg` `object_ref: M9, M10` và frame `adasind_086220.jpg` `object_ref: M5, M6`). Ngoài ra, mô hình vi phạm quy ước Rider (R03) khi tự ý tách người lái xe hai bánh thành một box `Pedestrian` độc lập (`M2, M3` tại `adasind_060000.jpg`).
  - *Lỗi MISSING (11 ca) và BOX_GEOMETRY (5 ca):* Nguyên nhân đến từ sai sót của annotator (`why: E1_annotator_error`) do khó khăn khi ước lượng hình học trên ảnh méo fisheye. Tiêu biểu là việc vẽ hở chân bánh xe ở mép cong (`adasind_060000.jpg` `L3`), cắt thiếu đầu xe chạm biên (`adasind_086220.jpg` `L1`), và vẽ bao trùm vật cản che khuất (`adasind_102750.jpg` `L3`).
- **Cách sửa và ai nhận việc (`owner`):**
  - *Về phía thuật toán/mô hình (`owner: ai_team`):* Cần fine-tune mô hình Object Detection trực tiếp trên dữ liệu ảnh fisheye với bộ taxonomy đầy đủ class `ThreeWheeler`, huấn luyện quy tắc gộp Rider + Bike theo R03, và bổ sung bộ lọc NMS liên lớp (cross-class NMS) để triệt tiêu các box thừa chồng lấn.
  - *Về phía gán nhãn (`owner: annotator`):* Thực hiện rework các ca P1 tại Phase P5: mở rộng box `L1` ở `086220` chạm biên x=1080 và kích hoạt cờ `truncated=true`; thu hẹp box `L3` ở `102750` chỉ bao quanh phần xe tải nhìn thấy thực tế; tinh chỉnh đáy box `L3` ở `060000` bám sát tiếp xúc mặt đường của bánh xe.
- **Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule):**
  - *Ảnh chụp minh chứng:* `submission/screenshots/060000_threewheeler_edge.png`, `submission/screenshots/086220_threewheeler_truncation.png`, `submission/screenshots/102750_truck_occlusion.png`.
  - *Dòng findings:* Các dòng `r1_craft` (dòng 2–5), `r2_qa` (dòng 6–8) và `r3_diag` (dòng 9–22) trong `submission/findings.csv`.
  - *Quy tắc đối chiếu:* Quy tắc `R02` (vẽ bám sát thực tế trên ảnh fisheye gốc), `R03` (Rider + Bike thành một box), `R04` (class ThreeWheeler), `R05` (thuộc tính truncated vs occluded).
