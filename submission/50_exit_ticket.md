# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling đã nằm trong file tương ứng nên không hỏi lại ở đây.

### 1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy tắc riêng? Vì sao?
- **Kết luận:** Đây **KHÔNG PHẢI** là lỗi `DUPLICATE`, mà **CẦN MỘT QUY TẮC RIÊNG**.
- **Giải thích:** Mỗi camera trong cụm SVM 360 chụp một luồng ảnh độc lập từ vị trí lắp đặt riêng biệt. Khi một phương tiện hoặc người đi bộ nằm tại vùng chồng lấn thị trường (overlap seam) giữa hai camera (ví dụ giữa camera trước và camera trái), thấu kính của cả hai camera đều thu nhận được ánh sáng từ vật thể đó. Việc mỗi camera có một bounding box 2D trong không gian tọa độ pixel riêng của nó là hoàn toàn chính xác về mặt quang học. Nếu xem đây là `DUPLICATE` và tùy tiện xóa đi một box ở một camera, các module xử lý cục bộ trên camera đó (như cảnh báo va chạm điểm mù 2D hoặc phát hiện chuyển động quang học) sẽ bị mất dữ liệu. 
- **Quy tắc chuẩn:** Trong không gian nhãn 2D từng camera, giữ nguyên cả hai box độc lập và liên kết bằng thuộc tính tham chiếu chéo (`cross_camera_id`). Việc hợp nhất (fusion) hai box thành một đối tượng vật lý duy nhất chỉ được thực hiện ở tầng biểu diễn 3D BEV (Bird’s-Eye View) sau khi đã ánh xạ qua ma trận hiệu chuẩn ngoại vị (Extrinsics).

---

### 2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.
- **Giữ cùng track ID:** Khi vật thể duy trì chuyển động liên tục trong trường nhìn, không bị che khuất hoàn toàn quá lâu, và có thể truy vết mượt mà về mặt động học (quỹ đạo tọa độ và vector vận tốc hợp lý) mà không thay đổi bản chất đối tượng.
- **Thêm keyframe:** Thêm keyframe khi đối tượng có sự biến đổi mạnh về hình học, hướng chuyển động (quay đầu, chuyển làn), hoặc khi bắt đầu/kết thúc trạng thái che khuất (`occluded`) hay chạm vào mép cắt khung hình (`truncated`).
- **Đặt trạng thái Outside:** Đặt `Outside` tại frame mà vật thể tạm thời di chuyển ra ngoài góc nhìn của camera hoặc bị che khuất hoàn toàn 100% bởi vật thể lớn khác trong một số frame, trước khi xuất hiện trở lại.
- **Bằng chứng cần thiết trước khi nối track qua hai camera:**
  1. *Đồng bộ thời gian phần cứng (Hardware Time Synchronization):* Tín hiệu timestamp giữa hai camera phải chênh lệch dưới 5 mili-giây.
  2. *Hình học vùng chồng lấn (Calibrated Seam Overlap):* Ma trận ngoại thông số (Extrinsics) của hai camera phải được cân chỉnh chuẩn xác so với tâm trục xe ego.
  3. *Tính nhất quán thị giác (Visual Re-ID Feature Consistency):* Đặc trưng màu sắc, phân lớp, kiểu dáng của vật thể ở camera rời đi phải trùng khớp với vật thể xuất hiện ở camera đón tiếp.
  4. *Tính liên tục về động học (Kinematic Continuity):* Vector vị trí và vận tốc ngoại suy từ camera 1 khi chạm vào đường seam phải ăn khớp với vector ban đầu của vật thể xuất hiện tại camera 2.

---

### 3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`), bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?
- **Vị trí bất đồng cụ thể:** Tại frame `adasind_086220.jpg`, đối tượng xe con ở phía xa `object_ref: L6` (kết hợp với Model `M8`, tạo thành cell `LM_noR`). Nhãn viên (L) và mô hình YOLO26m (M) đều nhận diện được một chiếc xe `Car` đang di chuyển ở trung tâm xa với chiều cao xấp xỉ 43 px, trong khi teaching reference (R) lại không có box này (báo lỗi `SPURIOUS`).
- **Cách xử lý của nhóm:** Trong Phase P4 chẩn đoán, nhóm không mù quáng xóa box để ép chỉ số khớp 100% với reference. Nhóm đã phân loại nguyên nhân là `why: E0_reference_defect` (lỗi bỏ sót của teaching reference đối với vật thể nhỏ ở cự ly xa gần ngưỡng H=40), chọn hành động `action: keep_with_reason`, và ghi nhận rõ ràng vào `findings.csv` cũng như `decision_log.csv`.
- **Nếu làm lại slice này, sẽ thay đổi gì:**
  1. *Đo chiều cao pixel chủ động:* Dùng công cụ đo thước (ruler) của CVAT để kiểm tra chiều cao đối tượng ngay từ P2 thay vì ước lượng mắt thường.
  2. *Tuân thủ chặt chẽ quy ước biên fisheye:* Nới rộng bounding box chạm hẳn mép khung hình và đánh dấu thuộc tính `truncated=true` ngay từ đầu cho các xe ở rìa (như chiếc ThreeWheeler ở frame `086220`), tránh để lỗi hình học kéo sang tận Phase P3/P4.
