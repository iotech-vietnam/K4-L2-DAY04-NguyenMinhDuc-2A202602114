# Báo cáo kiểm chéo bài bạn cùng nhóm

- Người kiểm: Nguyen Minh Duc (2A202602114)
- Người được kiểm: Bạn cùng nhóm (K4-L2)
- Ngày kiểm: 16/09/2026

## 1. Danh sách lỗi chi tiết

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| train_04.jpg | 1 | left_wrist | Chấm lấn sang người bên cạnh (nhầm người) | Dời chấm về phía cẳng tay của người lái hoodie xám |
| train_10.jpg | 1 | left_hip | Xoá khớp bị che (`v=0`) | Đổi sang `v=1` và ước lượng vị trí khớp háng |
| train_13.jpg | 3 | whole person | Thiếu hẳn một người ở xa bên trái | Bổ sung thêm skeleton cho người đi bộ lề trái |

## 2. Nhận xét & Thống nhất Guideline

1. **Lỗi lặp lại nhiều nhất:** Việc sử dụng cờ `v = 0` (Outside) cho các khớp nằm gọn trong khung hình nhưng bị quần áo hoặc vật cản che khuất.
2. **Nguyên nhân:** Do guideline ban đầu của nhóm chưa làm rõ sự khác biệt giữa "khớp ra ngoài khung ảnh" (`v=0`) và "khớp còn trong khung nhưng bị che" (`v=1`, vẫn phải đặt chấm ước lượng).
3. **Thống nhất:** Đã cập nhật mục 2 và 4 trong `GUIDELINE_MINI.md` để nhóm cùng áp dụng chung quy tắc ước lượng giải phẫu.
