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

*(Điền số từ outputs/eval_model.json sau khi chạy Colab Chặng 6)*

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | | | |
| pose_mAP50-95 | | | |
| pose_precision | | | |
| pose_recall | | | |
| box_mAP50-95 | | | |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?
3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?

## 5. Một rule evidence bạn đã dùng

- **Ảnh & Khớp:** `train_04.jpg` - Người số 1 (người lái xe mặc áo hoodie xám) - Khớp `left_wrist` (cổ tay trái).
- **Căn cứ thị giác:** Người này đang vươn cánh tay trái sang ngang/phía trước, cẳng tay hướng về phía ghi-đông xe cạnh người thứ 2. Vùng cổ tay bị che khuất một phần bởi thân xe và người đối diện nhưng toàn bộ cánh tay vẫn nằm trọn trong bức ảnh (không hề bị tràn ra mép ngoài khung hình).
- **Lý do chọn trạng thái:** Vì khớp cổ tay chắc chắn còn nằm bên trong không gian khung hình nên phải chọn trạng thái `v = 1` (Occluded) và đặt chấm ước lượng tại vị trí giải phẫu đầu cẳng tay, thay vì chọn `v = 0` (Outside).
