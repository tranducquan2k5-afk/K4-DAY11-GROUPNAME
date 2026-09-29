# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| **Lát cắt 1: Vùng biên cong và chạm viền ảnh** (`adasind_060000.jpg`, `adasind_086220.jpg`) | 4 ca: `BOX_GEOMETRY` và `ATTRIBUTE` (`060000` L3, `086220` L1, M5, M6) | Độ méo quang học mắt cá tăng phi tuyến ở vùng `edge`/`mid`, khiến annotator dễ vẽ hở đáy bánh xe và cắt thiếu đầu xe chạm biên ảnh, đồng thời bỏ sót thuộc tính `truncated=true`. | Ảnh chụp `060000_threewheeler_edge.png`, `086220_threewheeler_truncation.png`, tọa độ xbr trong file XML, báo cáo IoU cục bộ. |
| **Lát cắt 2: Che khuất tiền cảnh và nhận diện đa lớp** (`adasind_102750.jpg`, `adasind_060000.jpg`) | 5 ca: `BOX_GEOMETRY`, `WRONG_CLASS`, `SPURIOUS` (`102750` L3, M9, M10, M2) | Hiện tượng che khuất (occlusion) bởi cột/vật cản dẫn đến lỗi vẽ bao trùm vật cản; mô hình nhầm lẫn class `ThreeWheeler` thành `Truck`/`Car` và vi phạm quy ước Rider R03. | Ảnh chụp `102750_truck_occlusion.png`, ma trận nhầm lẫn `local_quality_confusion.csv`, các dòng findings `round=r3_diag`. |

**Giới hạn của kết luận từ ba frame ADASIND:** Cỡ mẫu 3 frame trong slice `B2-mid` mang tính chất cục bộ, chỉ phản ánh điều kiện giao thông ban ngày trên góc nhìn camera trước đơn lẻ. Không thể ngoại suy trực tiếp phân bố lỗi này cho 3 camera còn lại (Rear, Left, Right) hay các điều kiện vận hành ban đêm, mưa phùn, đường cao tốc.

---

## Chuyển sang kế hoạch bốn camera giả lập

- **Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv`:**
  - Áp dụng phương pháp lấy mẫu phân tầng (Stratified Sampling) theo 4 hướng camera (`front`, `rear`, `left`, `right`) và 2 nhóm độ khó (`normal`, `hard`).
  - Áp dụng bộ lọc khoảng cách thời gian (Temporal Subsampling): Giữ khoảng cách tối thiểu giữa các frame được chọn là 3–5 giây (tương đương cách nhau ít nhất 90–150 frame gốc) để triệt tiêu hiện tượng tự tương quan (auto-correlation), ngăn chặn việc đếm nhiều frame liền kề của cùng một tình huống dừng đèn đỏ hay kẹt xe như nhiều ca độc lập.
  - Kiểm tra độ đa dạng về môi trường (thời gian sáng/tối, thời tiết, mật độ giao thông đô thị và nông thôn).
- **Vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:**
  - Kế hoạch 200 frame chủ động tăng trọng số cho các lát cắt khó (hard slice chiếm 110/200 frame = 55% tổng mẫu kiểm tra, trong khi trong 50.000 frame thực tế các ca khó chỉ chiếm khoảng 5%–10%). Sự thiên lệch có chủ đích (intentional sampling bias) này giúp đội ngũ QA và AI phát hiện nhanh các góc chết (corner cases/failure modes) của thuật toán và quy chuẩn nhãn, nhưng không thể dùng tỷ lệ lỗi trên tập 200 frame này để đại diện cho sai số ngẫu nhiên (unbiased error rate) của toàn bộ 50.000 frame trong sản xuất.
