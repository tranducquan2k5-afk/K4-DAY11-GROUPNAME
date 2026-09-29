# Báo cáo QA độc lập (Phase P3) - Slice B2-mid

- **Người thực hiện (Vai B):** nqtung30624-ship-it (Nguyễn Quang Tùng - MSSV: 2A202603022)
- **Bản nhãn rà soát (Vai A):** DuongLong-205 (Dương Văn Long - Mã khóa: 1B88-2AA0)
- **Điều phối / Chẩn đoán (Vai C):** tranducquan2k5-afk (Trần Đức Quân)
- **Phương thức:** Cold review độc lập theo guideline nhãn (rules v1.0.0), chưa mở tham chiếu/mô hình.

---

## 1. Đánh giá tổng quan theo từng frame

- **`adasind_060000.jpg`:** Các đối tượng ở khu vực trung tâm (Pedestrian, Bike, ThreeWheeler) được gán nhãn tốt và đúng phân lớp. Tuy nhiên ở khu vực rìa ảnh chịu méo quang học mắt cá (fisheye), đối tượng ThreeWheeler ở sát mép cong (x=987..1079) có hiện tượng viền box chưa ôm sát đáy bánh xe và biên cong của thân xe.
- **`adasind_086220.jpg`:** Phân loại class phương tiện cơ bản chính xác. Tuy nhiên xe ThreeWheeler màu vàng-đen ở bên phải tiền cảnh (x=657..1061) bị vẽ cắt thiếu phần cản trước và bánh trước sát mép phải (dừng ở x=1061 trong khi xe kéo dài tới mép khung hình x=1080), đồng thời chưa kích hoạt thuộc tính `truncated=true`.
- **`adasind_102750.jpg`:** Xe tải Truck thùng đỏ tím bên phải (x=869..1080) có hiện tượng bị che khuất (occlusion) bởi vật cản/cột phía trước, nhưng annotator đã vẽ bounding box bao trùm toàn bộ cả vật cản thay vì chỉ vẽ phần thân xe nhìn thấy thực tế.

---

## 2. Chi tiết các ca nghi ngờ lỗi

### 1. Frame `adasind_060000.jpg` - ThreeWheeler (Vùng rìa cong mép ảnh)
- **Đối tượng rà soát:** `object_ref: L3` (ThreeWheeler sát rìa méo fisheye bên phải)
- **Hiện tượng:** Bounding box chưa bám sát đáy bánh xe do độ méo cong của ống kính fisheye, tạo khoảng hở viền dưới và mép ngoài.
- **Quy tắc vi phạm:** 
  - `R02` (Vẽ bám sát phần nhìn thấy thực tế trên ảnh fisheye gốc, không nắn thẳng hay ước lượng vùng méo).
  - `R10` (Độ ưu tiên P1 - lỗi hình học bounding box).
- **Ảnh minh chứng:** `submission/screenshots/060000_threewheeler_edge.png`

### 2. Frame `adasind_086220.jpg` - ThreeWheeler (Vùng rìa phải / Chạm biên khung hình)
- **Đối tượng rà soát:** `object_ref: L1` (ThreeWheeler auto-rickshaw vàng-đen xbr=1061.22)
- **Hiện tượng:** Bounding box bị cắt thiếu phần đầu xe, cản trước và bánh trước sát mép phải (dừng ở x=1061 trong khi xe kéo dài chạm biên x=1080). Thuộc tính `truncated` bị bỏ quên (hiện đặt là `false` thay vì `true`).
- **Quy tắc vi phạm:**
  - `R02` (Vẽ bao trùm toàn bộ phần nhìn thấy thực tế).
  - `R05` (Vật bị cắt bởi vòng kính hoặc biên khung hình bắt buộc phải bật thuộc tính `truncated=true`).
  - `R10` (Độ ưu tiên P1).
- **Ảnh minh chứng:** `submission/screenshots/086220_threewheeler_truncation.png`

### 3. Frame `adasind_102750.jpg` - Truck (Vật cản che khuất - Occlusion)
- **Đối tượng rà soát:** `object_ref: L3` (Truck thùng hàng đỏ tím bên phải xbr=1080)
- **Hiện tượng:** Bounding box vẽ quá rộng, bao trùm cả vật cản/cột che khuất ở tiền cảnh thay vì chỉ vẽ phần nhìn thấy thực tế của xe tải.
- **Quy tắc vi phạm:**
  - `R02` (Chỉ vẽ phần nhìn thấy trên ảnh gốc, không vẽ trùm vật cản khác).
  - `R05` (Phân biệt thuộc tính `occluded` khi bị vật khác che khuất).
  - `R10` (Độ ưu tiên P1).
- **Ảnh minh chứng:** `submission/screenshots/102750_truck_occlusion.png`

---

## 3. Bảng tổng hợp QA Finding (đối chiếu findings.csv)

| frame | object_ref | rule_id | what | nhận xét |
|---|---|---|---|---|
| `adasind_060000.jpg` | L3 | R02 | BOX_GEOMETRY | Box ThreeWheeler ở mép cong chưa bám sát đáy bánh xe do mép fisheye |
| `adasind_086220.jpg` | L1 | R05 | ATTRIBUTE | ThreeWheeler chạm mép ảnh bị cắt thiếu đầu xe và thiếu thuộc tính truncated |
| `adasind_102750.jpg` | L3 | R02 | BOX_GEOMETRY | Bounding box Truck vẽ bao trùm cả vật cản che khuất phía trước |

*(Ghi chú: Theo quy chuẩn QA Phase P3, các dòng finding `round=r2_qa` trong `submission/findings.csv` có `cell=L_only`, `rule_id` được ghi rõ ràng, và cột `why` được để trống do QA ghi nhận quan sát độc lập trước khi mở đối chiếu chẩn đoán).*

---

## 4. Đề xuất hành động

1. Đưa cả 3 ca trên vào thảo luận chẩn đoán Phase P4 (`r3_diag`) để đối chiếu với teaching reference và dự đoán của mô hình YOLO26m (`model_compare`).
2. Yêu cầu annotator (Vai A) thực hiện rework tại Phase P5 (`rework`):
   - Mở rộng box ThreeWheeler ở `086220` chạm mép ảnh x=1080 và bật cờ `truncated=true`.
   - Thu hẹp box Truck ở `102750` để bám sát thân xe nhìn thấy, loại trừ phần cột/vật cản che khuất.
   - Tinh chỉnh biên dưới của ThreeWheeler ở `060000` để ôm sát điểm tiếp xúc mặt đất của bánh xe.