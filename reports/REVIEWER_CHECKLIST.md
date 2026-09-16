# Reviewer checklist - điền khi kiểm bài người khác

Người gán: Bạn cùng nhóm (K4-L2)   Người kiểm: Nguyen Minh Duc (2A202602114)   Ngày: 16/09/2026

Chạy trước khi soi bằng mắt:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels <bài của họ>
python3 tools/visualize_pose.py --images dataset/images/train --labels <bài của họ> --out /tmp/vis_review
python3 tools/visibility_report.py --labels dataset/labels/train --compare <bài của họ>
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | Đạt, đủ 17 điểm/người |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☑ | Không bị đảo trái/phải ở thân |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | Các bộ xương phân định rõ |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☒ | Một vài ca hông bị lỡ để v=0 |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☑ | Đạt chuẩn |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | Không dùng Hidden |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | Đạt chuẩn COCO Keypoints |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | Đạt chuẩn YOLO Pose |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ | Đã so sánh |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ | Đã bổ sung guideline |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | 0 lỗi blocking |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| train_04.jpg | 1 | left_wrist | Chấm lấn sang người bên cạnh | Dời chấm về phía cổ tay hoodie xám |
| train_10.jpg | 1 | left_hip | Xoá khớp bị che (v=0) | Đổi sang v=1 và ước lượng vị trí hông |
| train_13.jpg | 3 | whole person | Thiếu người ở xa | Bổ sung thêm skeleton cho người đi bộ lề trái |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: Nhầm lẫn giữa cờ `v = 0` (Outside) và `v = 1` (Occluded) ở các khớp hông/chân bị che khuất.
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**? Chủ yếu là lỗi **guideline chưa rõ** về quy ước ước lượng xương chậu/hông khi mặc trang phục rộng hoặc bị vật cản che khuất.
