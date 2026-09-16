# Mini guideline - nhóm: G03  |  người gán: Lê Đức Minh Quân  |  ngày: 16/9/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Chỉ gán nhãn cho con người không gán nhãn cho manacanh, búp bê, tượng... | Chỉ gán nhãn cho con người không gán nhãn cho manacanh, búp bê, tượng... | Vì chỉ tracking người không tracking đồ vật|
| Hông của người mặc quần áo dài | left-hip và right-hip sẽ tính từ vị trí hông, trong trường hợp áo dài không nhìn thấy hông sẽ gán v=1 tại vị trí hông áo | Vì áo dài che mất hông |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Vẫn gán v=1 cho tại nhưng đặt trạng thái occluded | Vì vẫn ước lượng được vị trí tai |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Vẫn gán các điểm nhìn thấy các điểm không nhìn thấy đặt trạng thái outside | Vì vẫn nhìn thấy các điểm nhìn thấy |
| Cổ tay nằm sau tay lái / sau thân mình | Vẫn gán v=1 cho tại nhưng đặt trạng thái occluded | Vì vẫn ước lượng được vị trí cổ tay |
| Hai người chồng lên nhau | Vẫn gán v=1 cho các điểm bị khuất tại nhưng đặt trạng thái occluded | Vì vẫn ước lượng được vị trí cổ tay |
| Người nhỏ đến mức nào thì không gán nữa | người nhỏ tới mức không thể nhìn thấy rõ các khớp | Vì không nhìn rõ các khớp để gán dẫn tới dữ liệu sai |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01`, người thứ `19`, khớp `left_ear`

- Mơ hồ ở chỗ nào: Tai bị tóc che khuất
- Bạn quyết thế nào: Vẫn ước lượng và gán nhãn left_ear nhưng đặt trạng thái occluded
- Vì sao: Nhìn tỉ lệ người rõ ràng, ước lượng được vị trí của tai
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu quyết ngược lại thì model sẽ học được mô hình người bị khuyết thiếu tai mỗi khi bị che bới tóc, mũ ... dẫn tới sai số lớn

### Ca 2 - ảnh `train_02`, người thứ `37`, khớp `right_ear`

- Mơ hồ ở chỗ nào: Người quay đầu nhìn sau lưng dẫn tới trái phải bị đổi chiều và tai bị che khuất
- Bạn quyết thế nào: Dựa vào quy ước trái phải với người, nếu đang quay lưng về phía mình thì phải của họ là trái của mình và ngược lại, sau đó vẫn ước lượng và gán nhãn right_ear nhưng đặt trạng thái occluded
- Vì sao: Để đảm bảo gán nhãn chuẩn dữ liệu và model hiểu đúng quy ước trái phải với người
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ học sai quy ước trái phải với người dẫn tới sai số lớn

### Ca 3 - ảnh `train_03`, người thứ `con búp bê ở vỉa hè`, khớp `không dán nhãn`

- Mơ hồ ở chỗ nào: Có một con búp bê ở vỉa hè giống người
- Bạn quyết thế nào: Không dán nhãn cho búp bê
- Vì sao: Vì búp bê không phải người
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ học sai phân biệt người và vật, dẫn tới sai số lớn

## 4. Sau khi so visibility report với bạn cùng nhóm (Gold)

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `48%` / họ `3%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Guideline giữa hai bên chưa thống nhất khi xử lý tai bị che bởi tóc/mũ hoặc khi người quay nghiêng mặt (Bạn ưu tiên gán v=1 Occluded, Gold ưu tiên v=0 Outside hoặc không gán).
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Khớp tai/mắt bị che khuất hoàn toàn do góc quay hoặc vật che không thể xác định chính xác vị trí giải phẫu thì chọn `v=0` (Outside). Chỉ gán `v=1` (Occluded) khi khớp bị che một phần nhưng vẫn tự tin xác định đúng điểm giải phẫu.
