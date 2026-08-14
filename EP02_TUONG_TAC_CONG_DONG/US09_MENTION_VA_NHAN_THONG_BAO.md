# US09 — Mention và nhận thông báo

> Thuộc EP02 — Tương tác cộng đồng
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người dùng đã đăng nhập**, tôi muốn mention một tài khoản hợp lệ và nhận thông báo khi có người reply hoặc mention mình, để tiếp tục cuộc trò chuyện liên quan.

### Giá trị

- Tăng khả năng người dùng quay lại MyTV.
- Giúp cuộc hội thoại có đối tượng rõ ràng.
- Tạo vòng lặp tương tác giữa người xem.

### Ưu tiên

**Should**

### Điều kiện tiên quyết

- Người gửi và người nhận là tài khoản hợp lệ.
- Người gửi có quyền bình luận.
- Hệ thống thông báo MyTV hoạt động.

### Acceptance Criteria

1. Người dùng có thể nhập ký tự `@` trong bình luận hoặc reply để bắt đầu chọn tài khoản hợp lệ.
2. Hệ thống chỉ tạo mention khi người dùng chọn hoặc xác định được một tài khoản hợp lệ trong hệ thống.
3. Giao diện không làm lộ số điện thoại đầy đủ hoặc dữ liệu cá nhân nhạy cảm trong kết quả tìm mention.
4. Mention được lưu theo định danh tài khoản, không chỉ theo chuỗi nickname hiển thị.
5. Khi bình luận/reply chứa mention được phép hiển thị, người được mention nhận thông báo trong ứng dụng.
6. Khi có người reply bình luận của mình, tác giả bình luận nhận thông báo trong ứng dụng.
7. Hai trường hợp reply và mention đều tạo push notification nếu người nhận cho phép push.
8. Nếu cùng một sự kiện vừa là reply vừa mention cùng một người, hệ thống tránh gửi thông báo trùng không cần thiết.
9. Bấm thông báo mở đúng phim, tập và thread/bình luận liên quan.
10. Nếu bình luận đã bị xóa, ẩn hoặc người nhận không còn quyền xem phim, hệ thống hiển thị trạng thái phù hợp thay vì mở nội dung không có quyền.
11. Người dùng không nhận thông báo cho hành động của chính mình.
12. Thông báo chỉ được gửi khi nội dung đủ điều kiện hiển thị; nội dung đang chờ duyệt không thông báo cho người khác.

### Quy tắc nghiệp vụ

- Chỉ mention tài khoản hợp lệ.
- Gửi cả push và thông báo trong ứng dụng cho reply/mention.
- Quyền nhận push phụ thuộc cài đặt hệ điều hành và tùy chọn người dùng.
- Nội dung thông báo không được làm lộ Spoiler hoặc dữ liệu nhạy cảm trên màn hình khóa.

### Phụ thuộc

- Hệ thống tài khoản/nickname MyTV.
- Dịch vụ push notification và thông báo trong ứng dụng.
- Deep link đến phim/tập/comment.
- US11 và US12 về trạng thái kiểm duyệt.

### Điểm cần PO chốt

- Phạm vi tài khoản được gợi ý khi nhập `@`.
- Người dùng có được tắt riêng thông báo reply/mention hay không.
- Thời gian lưu thông báo trong ứng dụng.

---

