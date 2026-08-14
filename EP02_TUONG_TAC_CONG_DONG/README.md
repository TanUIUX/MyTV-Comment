# EP02 — TƯƠNG TÁC CỘNG ĐỒNG

## 1. Thông tin Epic

| Thuộc tính | Nội dung |
|---|---|
| Epic ID | EP02 |
| Tên Epic | Tương tác cộng đồng |
| Mục tiêu | Cho phép người dùng phản hồi, thể hiện đồng tình và duy trì cuộc trò chuyện xung quanh phim |
| Đối tượng | Người dùng MyTV đã đăng nhập |
| Giá trị kinh doanh | Tăng chiều sâu tương tác, tần suất quay lại và mức độ gắn kết giữa người xem |
| Phạm vi | Like, Reply một cấp, Mention, push notification và thông báo trong ứng dụng |

## 2. Kết quả mong đợi

- Người dùng tương tác được với bình luận mà không tạo hành vi trùng lặp.
- Hội thoại dễ đọc và không tạo cây reply quá sâu.
- Người được reply/mention nhận được thông báo và quay lại đúng ngữ cảnh.
- Tất cả tương tác tuân thủ quyền tài khoản và trạng thái bình luận.

## 3. Chỉ số gợi ý

- Tỷ lệ bình luận nhận ít nhất một Like hoặc Reply.
- Số Like và Reply trung bình trên mỗi bình luận.
- Tỷ lệ mở thông báo reply/mention.
- Tỷ lệ người dùng quay lại nội dung từ thông báo.

---


## Danh sách User Story

| ID | User Story | File |
|---|---|---|
| US07 | Like và Unlike bình luận | [US07](US07_LIKE_VA_UNLIKE_BINH_LUAN.md) |
| US08 | Trả lời bình luận một cấp | [US08](US08_TRA_LOI_BINH_LUAN_MOT_CAP.md) |
| US09 | Mention và nhận thông báo | [US09](US09_MENTION_VA_NHAN_THONG_BAO.md) |

[← Quay lại backlog tổng](../README.md)

## 4. Điều kiện hoàn thành Epic

- Like, Reply và Mention được kiểm soát quyền ở cả giao diện và API.
- Không phát sinh Like, Reply hoặc thông báo trùng do retry.
- Reply luôn tuân thủ cấu trúc một cấp.
- Push và thông báo trong ứng dụng mở đúng ngữ cảnh và không làm lộ Spoiler.
- Sự kiện tương tác được ghi nhận để phục vụ thống kê.
