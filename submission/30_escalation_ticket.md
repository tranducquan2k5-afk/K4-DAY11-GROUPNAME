# Escalation ticket

## Ticket 1

- **Frame:** `adasind_060000.jpg` (đối tượng `object_ref: M1`, cell `M_only`, `what: SPURIOUS`, vi phạm `R07`)
- **Ảnh chụp:** `submission/screenshots/060000_threewheeler_edge.png`
- **Expected impact:** Mô hình YOLO26m phát hiện vật thể giả (false positive) nằm đè trực tiếp lên phần thân xe thử nghiệm (`ego_body`) ở góc đáy khung hình. Trong hệ thống lái tự động / ADAS, lỗi này gây ra hiện tượng phanh khẩn cấp giả lập (phantom braking) hoặc báo động va chạm ảo liên tục do nhầm thân xe mình thành chướng ngại vật ngoại cảnh.
- **Owner:** `ai_team`
- **Recommendation:** Bổ sung bước lọc mặt nạ không gian (spatial mask post-filtering) trong pipeline triển khai mô hình: tự động triệt tiêu (zero-out / suppress) mọi bounding box có tâm hoặc diện tích giao cắt ≥ 50% với vùng polygon tĩnh của thân xe `ego_body` đã được cân chỉnh cho từng vị trí camera.
