# Quan sát vạch ô đỗ

- **Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh):** Các đoạn vạch sơn phân chia các ô đỗ xe riêng biệt tại khu vực tiền cảnh bãi đỗ (ví dụ: đoạn vạch chéo phân chia giữa ô đỗ từ tọa độ `(171.75, 521.21)` đến `(245.70, 562.77)` và đoạn vạch lân cận từ `(281.30, 518.83)` đến `(419.77, 553.98)`). Các vạch này định vị rõ rệt hai cạnh bên của từng vị trí đỗ xe.
- **Một vạch/dấu sơn hoặc biên không vẽ, và vì sao:** Vạch chỉ hướng lối đi chung / ranh giới làn xe chạy xuyên suốt ở giữa hai dãy ô đỗ và các vệt gờ mép vỉa hè phía xa; không vẽ vì đây là làn lưu thông xe chạy nội bộ, không có chức năng ngăn cách phân định khoang đỗ xe (`parking_line`).
- **Polygon `free_space` dừng ở đâu; có phần bị che nào không:** Polygon `free_space` được vẽ bao quanh các khoang ô đỗ trống không có xe đỗ ở tiền cảnh; polygon dừng sát mép trong của các vạch kẻ ô và dừng lại trước mép các vật cản/xe đỗ lân cận, không lan sang làn đường xe chạy và không bao trùm các vùng bị che khuất điểm mù.
- **Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”):** Không có (các vạch ô đỗ ở tiền cảnh hiển thị rõ ràng, độ tương phản mặt đường cao, không bị che khuất phức tạp).
