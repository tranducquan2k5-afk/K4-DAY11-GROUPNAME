# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 9 | 3 | 2 | 5 | 8 | MISSING (3) |
| mid | 8 | 0 | 1 | 4 | 10 | SPURIOUS (1) |
| edge | 3 | 1 | 0 | 2 | 0 | MISSING (1) |

## Nhận xét

- **Zone người (L) và model (M) gãy nhiều nhất:**
  - **Model (M):** Gãy nặng nhất tại zone `mid` (10 ca thừa `LM_noR` + `M_only`, 4 ca bỏ sót) và zone `center` (8 ca thừa, 5 ca bỏ sót). Ở zone `mid`, mô hình sinh ra lượng lớn box thừa do phân loại sai đối tượng (gán nhầm `ThreeWheeler` kích thước lớn thành `Truck` hoặc `Car`, và vi phạm luật Rider R03 khi tách người lái thành box `Pedestrian` riêng biệt).
  - **Người gán nhãn (L):** Gãy tập trung ở zone `center` với 3 ca `MISSING` và 2 ca `SPURIOUS`. Đây chủ yếu là các vật thể nhỏ ở cự ly xa gần ngưỡng chiều cao `H=40 px` (như xe máy hoặc người đi bộ nhỏ), khiến annotator dễ bỏ sót hoặc ước lượng sai biên độ box.
- **Giả thuyết nguyên nhân và giới hạn của slice ba frame:**
  - *Mô hình (Domain Gap):* YOLO26m được huấn luyện trên không gian ảnh pinhole thông thường, không thích nghi tốt với độ cong quang học dạng mắt cá (barrel distortion) ở vùng `mid`/`edge`, đồng thời thiếu class đặc thù `ThreeWheeler` của giao thông Nam Á / Đông Nam Á.
  - *Con người (Cognitive/Visual Load):* Khó phân biệt ranh giới chiều cao chính xác của các vật thể ở xa tâm bức ảnh khi chưa có công cụ zoom/đo tự động trong CVAT.
  - *Giới hạn của slice 3 frame:* Cỡ mẫu 3 frame trong slice `B2-mid` chỉ phản ánh một lát cắt cục bộ ở góc camera trước vào ban ngày, chưa đủ tính đại diện thống kê cho mọi góc camera (Rear, Left, Right), các điều kiện thời tiết phức tạp hay các tình huống che khuất động cao trên toàn hệ thống SVM 360°.
