# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `1b882aa0ebebceb51631aba0e4b8af2e388b840b28c8151e529528b613b7d264`; slice `B2-mid`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_060000.jpg, adasind_086220.jpg, adasind_102750.jpg. Frame thiếu trong export: không.
TP=16; FP=3; FN=4; số lần đối chiếu=23; mean IoU của TP=0.843.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.696 | 0.939 | 0.870 |
| precision | 0.842 | 0.708 | 0.000 |
| recall | 0.800 | 0.639 | 0.000 |
| jaccard | 0.696 | 0.590 | 0.000 |
| dice | 0.821 | 0.669 | 0.000 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 0 | 1 | 0.957 | 1.000 | 0.750 | 0.750 | 0.857 |
| Car | 0 | 1 | 0 | 0.957 | 0.000 | 0.000 | 0.000 | 0.000 |
| Pedestrian | 4 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| ThreeWheeler | 7 | 1 | 2 | 0.870 | 0.875 | 0.778 | 0.700 | 0.824 |
| Truck | 2 | 1 | 1 | 0.913 | 0.667 | 0.667 | 0.500 | 0.667 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_060000.jpg | 9 | 0 | 1 | 0.900 | 1.000 | 0.900 |
| adasind_086220.jpg | 4 | 2 | 1 | 0.571 | 0.667 | 0.800 |
| adasind_102750.jpg | 3 | 1 | 2 | 0.500 | 0.750 | 0.600 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 0 | 0 | 0 | 0 | 1 |
| Car | 0 | 0 | 0 | 0 | 0 | 0 |
| Pedestrian | 0 | 0 | 4 | 0 | 0 | 0 |
| ThreeWheeler | 0 | 0 | 0 | 7 | 0 | 2 |
| Truck | 0 | 0 | 0 | 0 | 2 | 1 |
| <extra> | 0 | 1 | 0 | 1 | 1 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
