# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Lê Đức Minh Quân   Nhóm: G03  Ngày: 16/9/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 2358/1104/31 |
| Thời gian trung bình mỗi ảnh | 15.93 |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear (48%)
2. right_ear (41%)
3. right_wrist (31%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Theo tôi các khớp left ear và right ear khá khó vì nhiều ảnh sẽ bị che khuất do tóc, mũ, quay mặt ... còn right wrist thỉnh thoảng bị che hoặc khó xác định chính xác vị trí vì tay nhiều khi bị che bởi quần áo hoặc vật dụng khác
<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.7971 | 0.8357 |
| OKS@0.50 | 0.8264 | 0.8730 |
| OKS@0.75 | 0.8966 | 0.9286 |
| Lỗi `dao_trai_phai` | 1 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- train_04 Người 109 khớp right ankle đặt outside cho khớp
- train_09 Người 199 tất cả các lớp right left đảo ngược các lớp trái phải với nhau

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Lỗi đảo trái phải ở ảnh train_09. Ảnh này dễ, nhưng trong quá trình làm tôi nhầm lẫn do họ quay cùng hường với tôi chứ không phải đối diện

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

## 3. Kiểm chéo

Bạn cùng nhóm: gold

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| left_ear | 14 | 1 | +13 | Guideline: Bạn gán v=1 (Occluded) khi tai bị tóc/mũ che hoặc ở góc nghiêng, trong khi Gold coi là v=0 (Outside) hoặc không gán. |
| right_ear | 12 | 4 | +8 | Guideline: Bạn gán v=1 cho tai phía xa khi quay mặt nghiêng, Gold ưu tiên v=0. |
| right_wrist | 8 | 14 | -6 | Gold thường xác định được cổ tay mặc dù bị che một phần hoặc ở xa, trong khi bạn có xu hướng gán v=0 (Outside) sớm hơn khi có vật thể che phủ. |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- Chỉ gán nhãn cho con người không gán nhãn cho manacanh, búp bê, tượng...
- left-wrist và right_wrist sẽ tính từ cổ tay, trong trường hợp áo dài không nhìn thấy cổ tay sẽ gán v=1 tại vị trí cổ tay áo
- left-hip và right-hip sẽ tính từ vị trí hông, trong trường hợp áo dài không nhìn thấy hông sẽ gán v=1 tại vị trí hông áo
## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   - **Trả lời:** `pose_mAP50-95` **tăng +0.0055** (từ `0.6853` lên `0.6908`, tức tăng ~0.55%). 
   - **Giải thích:** Do `pose_mAP50-95` tăng (không giảm), 20 ảnh được gán nhãn kĩ lưỡng đã dạy cho model nhận biết tốt hơn vị trí các khớp bị che lấp (`v=1` occluded do trang phục áo dài/tóc/mũ) - trường hợp mà bộ dữ liệu COCO chuẩn chưa bao phủ đủ sâu. Độ chính xác `pose_precision` cũng tăng từ `0.9734` lên `0.9792` (+0.0058).

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?
   - **Trả lời:** Sau fine-tune, `box_mAP50-95` đạt **0.8041** (80.41%) trong khi `pose_mAP50-95` đạt **0.6908** (69.08%), chênh lệch **0.1133 (11.33%)** (ở mức mAP50: `box_mAP50` 0.9600 so với `pose_mAP50` 0.8450, chênh 11.5%).
   - **Kết luận:** Model tìm **người (box) dễ hơn nhiều** so với tìm **khớp (pose)**.
   - **Lý do:** Bounding box của người bao phủ toàn bộ cơ thể, có diện tích lớn và đường biên dạng rõ ràng nên AI phát hiện rất dễ. Ngược lại, các khớp keypoint là những điểm mốc rất nhỏ, dễ bị che khuất bởi trang phục (áo dài), bị giấu sau thân mình/vật thể, hoặc nhầm lẫn trái/phải khi người xoay/nghiêng.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   - **Trả lời:** Ở ảnh `train_13` (ảnh có OKS thấp nhất 0.537): Model bị lỗi **Nhầm người** (model phát hiện 2 người trong khi nhãn thực tế chỉ có 1 người), đồng thời gặp lỗi **Trượt hẳn** và **Lệch nhẹ** ở các khớp cổ tay/hông bị che bởi trang phục rộng.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   - **Trả lời:** Ảnh có OKS thấp nhất giữa nhãn của bạn và model là **`train_13`** với điểm OKS là **`0.537`**.
   - **Đánh giá:** **Nhãn của bạn đúng hơn**. Dựa vào thực tế ảnh `train_13` chỉ có 1 người nằm trọn trong khung hình, nhưng model phát hiện thừa 1 bbox rác ở bên cạnh (`model 2 / bạn 1`) làm OKS bị tụt mạnh.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
   - **Trả lời:** **Có.** Các ảnh bạn gặp cảnh báo/khó gán (như `train_13`, `train_03`) cũng chính là các ảnh model đạt điểm OKS thấp nhất.
   - **Ý nghĩa:** Điều này chứng tỏ đây là những **bức ảnh vô cùng thách thức (edge case)** thực sự: người bị che khuất phần lớn cơ thể, bị góc chụp quá nghiêng hoặc quần áo quá rộng làm mờ ranh giới giải phẫu, khiến cả người gán nhãn thủ công lẫn model AI đều gặp khó khăn.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

- **Ảnh & Khớp:** Trong ảnh `train_01.jpg`, người thứ 19, khớp `left_ear`.
- **Căn cứ thị giác:** Người trong ảnh đứng quay nghiêng góc 3/4 và có phần tóc che khuất vùng tai bên trái. Mặc dù bề mặt tai không nhìn thấy trực tiếp, nhưng khuôn mặt, mắt trái và phần vành đầu vẫn hiển thị rõ ràng trong khung hình.
- **Lý do chọn v=1 (Occluded):** Do vị trí tai trái hoàn toàn nằm bên trong khung hình (không bị mép ảnh cắt mất), ta hoàn toàn có thể tự tin ước lượng được vị trí giải phẫu tương quan của tai dựa trên vị trí mắt trái và chân tóc. Vì vậy chọn `v=1` (vẫn chấm vị trí ước lượng) thay vì `v=0` (Outside).
