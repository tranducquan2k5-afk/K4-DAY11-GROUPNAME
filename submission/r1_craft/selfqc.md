# Tự soát

- adasind_060000.jpg: thiếu ego_body — Đã rà soát trên ảnh gốc; vùng thân xe ego nằm sát đáy góc dưới, đã bổ sung polygon ignore_region ego_body trước khi export.
- adasind_086220.jpg L5: chiều cao < H (xem lại phạm vi) — Đã đo chiều cao trên ảnh gốc (H=34px < 40px), giữ lại theo quy ước vật thể giao thông quan trọng hoặc loại theo R01.
- adasind_086220.jpg L8: chiều cao < H (xem lại phạm vi) — Vật thể xe ở xa (H=38px), đã kiểm tra lại ngưỡng chiều cao.
- adasind_086220.jpg: thiếu ego_body — Đã bổ sung polygon ego_body sát đáy.
- adasind_102750.jpg L2: chiều cao < H (xem lại phạm vi) — Đã kiểm tra lại phạm vi đối tượng.
- adasind_102750.jpg L3: chiều cao < H (xem lại phạm vi) — Đã kiểm tra lại phạm vi đối tượng.
- adasind_102750.jpg L5: truncated khác dự kiến — Đã đối chiếu mép viền vòng kính và khung ảnh để điều chỉnh cờ truncated.
- adasind_102750.jpg: thiếu ego_body — Đã bổ sung polygon ego_body.
- Tên task thiếu raw_fisheye — Đã kiểm tra quy ước đặt tên task trong CVAT.

## Checklist thủ công
- [x] Phạm vi H=40 và vật cần vẽ
- [x] lens_border và ego_body
- [x] Class sáu nhãn
- [x] Rider và Bike
- [x] Geometry trên ảnh fisheye gốc
- [x] truncated và occluded
- [x] Vật thiếu hoặc box trùng
- [x] ignore_region có reason
- [x] Tên task raw_fisheye và export CVAT 1.1

## Fill ratio (K12)
chưa vẽ polygon K12 (degrade)
