# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyen Minh Duc   Mã sinh viên: 2A202602114   Nhóm: K4-L2   Ngày: 16/09/2026

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 327 / 100 / 49 |
| Thời gian trung bình mỗi ảnh | ~4 phút |

Ba khớp có `%v=1` cao nhất (lấy từ `reports/visibility_report.md`):

1. `left_ear`: 68% (19/28 người bị che tai trái do góc nghiêng hoặc tóc/mũ)
2. `right_ear`: 46% (13/28 người bị che tai phải)
3. `right_eye`: 29% và `left_eye`: 25%

**Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích:**
Các khớp có `%v=1` cao nhất ở trên (tai, mắt) chủ yếu là do góc chụp nghiêng, góc quay đầu hoặc bị tóc/mũ bảo hiểm che khuất một phần. Tuy nhiên, khớp khó gán nhất trên thực tế lại là **khớp hông (`left_hip`, `right_hip`) và đầu gối/cổ chân bị che**, vì người thường mặc quần áo rộng hoặc bị che bởi vật cản (xe máy, ván trượt, túi xách), đòi hỏi phải ước lượng vị trí giải phẫu xương thay vì nhìn thấy bề mặt rõ ràng.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9294 | 0.9294 |
| OKS@0.50 | 0.9655 | 0.9655 |
| OKS@0.75 | 0.9655 | 0.9655 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 1 | 1 |
| Lỗi `xoa_khop_bi_che` | 4 | 4 |

**Tôi đã sửa gì giữa hai lần chạy:**
- Bài nộp lần 1 đạt mức Xuất sắc (OKS trung bình 0.9294 > 0.85, không có lỗi đảo trái/phải), được giữ nguyên làm baseline để chuyển sang fine-tune model.
- Các điểm ghi nhận từ đánh giá gold:
  - `train_13.jpg`: Gold có gán 1 người phụ đi bộ xa ở lề đường bên trái.
  - `train_04.jpg`: Khớp cổ tay trái của người đi xe hoodie xám bị trùng vùng với găng tay người bên phải.
  - `train_10.jpg`, `train_11.jpg`: Khớp hông bị che do tư thế/quần áo dài được gold ghi nhận là v=1 ước lượng thay vì v=0.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?**
Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh đã gán (`dao_trai_phai: 0`). Tất cả các khớp trái/phải đều được xác định đúng theo góc nhìn cơ thể của người trong ảnh.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| left_ear | 68% | | | Thống nhất quy ước tai bị che bởi tóc/mũ |
| right_ear | 46% | | | Góc nhìn nghiêng khuôn mặt |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:
- Với tai bị tóc/mũ che: Nếu vành tai hoặc vị trí ước lượng của tai vẫn nằm trọn trong vùng đầu/ảnh thì đánh dấu `v=1` (Occluded), không dùng `v=0`.
- Với hông người mặc quần áo dài: Dựa trên thắt lưng hoặc nếp gấp quần tại điểm nối xương chậu để đặt chấm ước lượng với `v=1`.

## 4. Model

*(Dữ liệu trích xuất từ outputs/eval_model.json sau khi chạy Colab Chặng 6)*

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. **`pose_mAP50-95` thay đổi bao nhiêu? Nếu nó thay đổi, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?**
   - Chỉ số `pose_mAP50-95` tăng từ `0.6853` lên `0.6908` (+0.0055, tăng khoảng +0.55%), đồng thời `pose_precision` tăng từ `0.9734` lên `0.9792` (+0.0058).
   - Dù tập train chỉ gồm 20 ảnh (rất nhỏ), việc nhãn gán chuẩn xác, không có lỗi đảo trái/phải (`dao_trai_phai: 0`) và áp dụng tốt cờ `v=1` ước lượng các khớp bị che đã giúp model củng cố độ tự tin ở các tư thế vận động khó. Ngược lại, `box_mAP50-95` giảm nhẹ (-0.0078) do mạng dồn trọng số tối ưu hóa cho keypoint loss của tập dữ liệu mới, dẫn đến ranh giới hộp bao tổng quát bị co kéo nhẹ.

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?**
   - `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) tới 11.33% (ở mAP50, box đạt 0.9600 còn pose đạt 0.8450, chênh 11.5%).
   - Model tìm **người (bounding box)** dễ hơn tìm **khớp (keypoints)** rất nhiều.
   - **Lý do:** Bounding box chỉ cần bắt được silhouette tổng thể của cơ thể (dựa vào texture quần áo, khuôn mặt, độ tương phản so với nền). Trong khi đó, keypoint pose đòi hỏi tọa độ cực kỳ chính xác của 17 điểm nhỏ, vốn thường xuyên bị biến dạng phi tuyến tính theo tư thế (uốn éo, gập người, nhảy), bị che khuất hoặc lẫn vào các nếp gấp trang phục.

3. **Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):**
   - Quan sát ảnh `test_04.jpg` (người đàn ông ngồi sau bàn ăn trong quán):
   - Kiểu lỗi: **Trượt hẳn (khớp hông) & Thiếu khớp (đầu gối, cổ chân)**. Do người này ngồi bị che khuất toàn bộ nửa thân dưới bởi mặt bàn tròn bày nhiều đĩa thức ăn, model dự đoán khớp hông (`left_hip`, `right_hip`) trượt hẳn lên bề mặt bàn giữa các ly nước, và không phát hiện được khớp chân dưới gầm bàn.

4. **Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?**
   - Ảnh có độ lệch OKS lớn nhất giữa nhãn sinh viên và model dự đoán là `train_13.jpg` và `train_04.jpg`.
   - Ở `train_13.jpg`: Người đi bộ ở lề đường bên trái rất nhỏ và tối màu, model hoàn toàn bỏ sót. Ở 2 người chính giữa, nhãn người gán chính xác hơn model ở các điểm khớp chân bị che bởi thùng rác/túi xách vì con người hiểu cấu trúc giải phẫu cơ thể khi đứng thẳng, còn model bị hoa văn nếp nhăn quần và vật cản đánh lừa.

5. **Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?**
   - **Có.** Cả người gán nhãn và model đều gặp thách thức lớn nhất ở `train_13.jpg` (người nhỏ ở xa, che khuất nhiều) và `train_04.jpg` (hai người chồng chéo nhau khi đi xe máy trong rừng).
   - Điều này chứng tỏ: Các bức ảnh này có **độ mơ hồ thị giác cao (high visual ambiguity)** — gồm che khuất nặng (occlusion), chồng lấn nhiều người (crowded/overlapping), hoặc điều kiện ánh sáng phức tạp. Đây là bài toán khó chung của cả người gán nhãn lẫn thuật toán thị giác máy tính.

## 5. Một rule evidence bạn đã dùng

- **Ảnh & Khớp:** `train_04.jpg` - Người số 1 (người lái xe mặc áo hoodie xám) - Khớp `left_wrist` (cổ tay trái).
- **Căn cứ thị giác:** Người này đang vươn cánh tay trái sang ngang/phía trước, cẳng tay hướng về phía ghi-đông xe cạnh người thứ 2. Vùng cổ tay bị che khuất một phần bởi thân xe và người đối diện nhưng toàn bộ cánh tay vẫn nằm trọn trong bức ảnh (không hề bị tràn ra mép ngoài khung hình).
- **Lý do chọn trạng thái:** Vì khớp cổ tay chắc chắn còn nằm bên trong không gian khung hình nên phải chọn trạng thái `v = 1` (Occluded) và đặt chấm ước lượng tại vị trí giải phẫu đầu cẳng tay, thay vì chọn `v = 0` (Outside).
