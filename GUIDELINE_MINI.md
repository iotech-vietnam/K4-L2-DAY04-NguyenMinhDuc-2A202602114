# Mini guideline - nhóm: K4-L2  |  người gán: Nguyen Minh Duc (2A202602114)  |  ngày: 16/09/2026

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
| Hông của người mặc quần áo dài | Căn cứ vào vị trí thắt lưng hoặc nếp gấp đũng quần nối với khớp háng; đặt chấm tại mấu chuyển lớn xương đùi với cờ `v = 1`. | Khớp hông không lộ bề mặt giải phẫu khi mặc đồ, nhưng vị trí trục xoay chân vẫn suy luận được từ hướng chân và thân mình. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu vị trí giải phẫu của tai vẫn nằm trọn trong chu vi đầu/mũ thì đặt chấm tại tâm tai với cờ `v = 1`. | Khớp vẫn nằm trong khung hình, chỉ bị che khuất bề mặt. Không dùng `v = 0` vì tai chưa bay ra ngoài ảnh. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp từ đầu gối, cổ chân bị tràn ra ngoài biên ảnh thì đánh dấu `v = 0` và không đặt chấm. | Tuân thủ đúng chuẩn COCO: ngoài khung hình bắt buộc `v = 0` để model không học tọa độ ảo ở biên. |
| Cổ tay nằm sau tay lái / sau thân mình | Ước lượng vị trí cổ tay dựa theo đường nối cẳng tay và bàn tay, đặt chấm với cờ `v = 1`. | Toàn bộ cánh tay vẫn nằm trong khung hình, chỉ bị vật thể phía trước (ghi-đông xe, túi xách) che khuất. |
| Hai người chồng lên nhau | Phân định ranh giới người dựa trên màu áo và hướng xương sống; gán xong 17 điểm của người này rồi mới sang người kia. | Tránh lỗi nghiêm trọng nhất là "nhầm người" (kéo xương người này sang cơ thể người bên cạnh). |
| Người nhỏ đến mức nào thì không gán nữa | Trong bộ 20 ảnh core, gán tất cả các cá thể người có thể phân biệt được đầu và thân mình. | Đảm bảo độ bao phủ (coverage) trùng khớp với tập gold chuẩn. |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_04.jpg`, người thứ `1` (bên trái), khớp `left_wrist`

- **Mơ hồ ở chỗ nào:** Cánh tay trái của người mặc hoodie xám vươn sang phải, vùng cổ tay nằm sát ghi-đông và chồng lấn lên khu vực găng tay của người bên phải.
- **Bạn quyết thế nào:** Đặt cờ `v = 1` tại vị trí tiếp giáp giữa cẳng tay xám và ghi-đông xe.
- **Vì sao:** Cẳng tay vẫn hướng về ghi-đông và nằm trọn trong ảnh, nhưng bàn tay bị góc nhìn che khuất một phần.
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Nếu nhầm sang găng tay của người bên phải, model học lỗi "nhầm người"; nếu để `v = 0`, model sẽ bỏ qua khớp còn nằm trong khung hình.

### Ca 2 - ảnh `train_10.jpg`, người thứ `1`, khớp `left_hip` & `right_hip`

- **Mơ hồ ở chỗ nào:** Vận động viên lướt sóng mặc quần đùi rộng và cúi thấp trọng tâm, bọt sóng và góc nghiêng che khuất vị trí hông.
- **Bạn quyết thế nào:** Dựa trên đường cong thắt lưng và điểm bắt đầu của cơ đùi để chấm 2 điểm hông với cờ `v = 1`.
- **Vì sao:** Khớp hông là tâm điểm chịu lực của cơ thể khi lướt sóng, trục xương chậu có thể ước lượng hình học được.
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Nếu để `v = 0` (Outside), model sẽ không thể học được sự liên kết giữa thân trên và đôi chân ở tư thế uốn lượn thể thao.

### Ca 3 - ảnh `train_16.jpg`, người thứ `2` (áo trắng #15), khớp `left_shoulder` & `right_shoulder`

- **Mơ hồ ở chỗ nào:** Người này quay lưng về phía máy ảnh nhưng ngoái đầu sang bên để đón đĩa bay, khiến trục mắt và trục vai trông như lệch hướng nhau.
- **Bạn quyết thế nào:** Giữ nguyên quy ước trái/phải theo cơ thể người: tay vươn lên ném đĩa bên phải là `right_shoulder`, tay chúc xuống bên trái là `left_shoulder` với cờ `v = 2`.
- **Vì sao:** Trái/phải phải tính theo cơ thể người thực tế (anatomical left/right), không phụ thuộc vào hướng mặt quay hay chiều nhìn của ảnh.
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Nếu gán theo chiều mắt hoặc chiều nhìn màn hình, sẽ phạm lỗi "đảo trái/phải" — khi chạy data augmentation lật ảnh (`fliplr=0.5`), model sẽ bị dạy cái sai đó hai lần.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `68%` / họ `45%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Guideline ban đầu chưa quy định cụ thể khi nào tai bị che bởi tóc/mũ thì được tính là `v=1` hay `v=2`.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Chỉ đánh dấu `v=2` khi nhìn thấy trọn vẹn vành tai; nếu vành tai bị tóc/mũ che lấp trên 30% diện tích thì chuyển sang `v=1` (Occluded).
