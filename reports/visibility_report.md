# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.93 khớp có v > 0 mỗi người
- Tổng: v=2 357 | v=1 105 | v=0 31

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 24 | 5 | 0 | 17% |
| 1 | left_eye | 21 | 8 | 0 | 28% |
| 2 | right_eye | 21 | 8 | 0 | 28% |
| 3 | left_ear | 15 | 14 | 0 | 48% |
| 4 | right_ear | 17 | 12 | 0 | 41% |
| 5 | left_shoulder | 27 | 2 | 0 | 7% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 25 | 3 | 1 | 10% |
| 8 | right_elbow | 25 | 4 | 0 | 14% |
| 9 | left_wrist | 19 | 9 | 1 | 31% |
| 10 | right_wrist | 19 | 9 | 1 | 31% |
| 11 | left_hip | 22 | 7 | 0 | 24% |
| 12 | right_hip | 23 | 6 | 0 | 21% |
| 13 | left_knee | 21 | 3 | 5 | 10% |
| 14 | right_knee | 21 | 3 | 5 | 10% |
| 15 | left_ankle | 15 | 5 | 9 | 17% |
| 16 | right_ankle | 14 | 6 | 9 | 21% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
