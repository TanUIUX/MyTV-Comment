# US08 — Trả lời bình luận một cấp

> Thuộc EP02 — Tương tác cộng đồng
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người dùng đã đăng nhập**, tôi muốn trả lời trực tiếp một bình luận, để tham gia cuộc trò chuyện mà vẫn giữ cấu trúc hội thoại dễ theo dõi.

### Giá trị

- Tạo thảo luận hai chiều giữa người xem.
- Giúp nội dung trao đổi gắn đúng với bình luận gốc.
- Hạn chế độ phức tạp giao diện bằng mô hình reply một cấp.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập và còn quyền bình luận.
- Bình luận gốc đang hiển thị và cho phép reply.
- Phim đang mở bình luận.

### Acceptance Criteria

1. Người dùng có thể chọn Trả lời trên một bình luận gốc.
2. Reply được liên kết với đúng bình luận gốc, series/tập và nội dung hiện tại.
3. Hệ thống chỉ hỗ trợ một cấp reply bên dưới bình luận gốc.
4. Khi người dùng chọn trả lời một reply, nội dung mới vẫn được lưu ở cấp một dưới bình luận gốc; giao diện có thể tự động mention người được trả lời để giữ ngữ cảnh.
5. Reply hỗ trợ văn bản, emoji và đánh dấu Spoiler theo quy tắc của bình luận.
6. Reply đi qua cùng cơ chế kiểm duyệt với bình luận gốc.
7. Reply công khai mới làm tăng tổng số bình luận của phạm vi hiện tại.
8. Khi số reply vượt giới hạn hiển thị ban đầu, người xem có thể chọn “Xem thêm phản hồi”.
9. Tác giả có thể sửa hoặc xóa reply của mình theo quy tắc tại US05.
10. Nếu bình luận gốc bị xóa, toàn bộ reply trong thread không còn hiển thị.
11. Nếu bình luận gốc bị ẩn hoặc khóa tương tác, hệ thống không cho phép tạo reply mới.
12. Người chưa đăng nhập chọn Trả lời được chuyển sang luồng đăng nhập.

### Quy tắc nghiệp vụ

- Độ sâu tối đa là một cấp.
- Reply được tính vào tổng số bình luận.
- Reply không tham gia danh sách bình luận gốc khi sắp xếp.
- Xóa bình luận gốc làm toàn bộ thread biến mất khỏi trải nghiệm người dùng.

### Phụ thuộc

- US04 — Đăng bình luận.
- US05 — Sửa và xóa bình luận.
- US09 — Mention và nhận thông báo.
- EP03 — An toàn và kiểm duyệt.

### Điểm cần PO chốt

- Số reply hiển thị mặc định trước nút “Xem thêm”.
- Cách hiển thị khi một reply riêng lẻ bị xóa giữa hội thoại.

---

