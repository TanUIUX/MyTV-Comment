# US04 — Đăng bình luận

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người dùng đã đăng nhập**, tôi muốn đăng bình luận bằng văn bản, emoji và tùy chọn Spoiler, để chia sẻ cảm nhận về series hoặc tập phim đang xem.

### Giá trị

- Tạo nguồn nội dung cộng đồng cốt lõi cho MyTV.
- Cho phép người xem bày tỏ cảm xúc trong ngữ cảnh phim.
- Tạo dữ liệu cho tương tác, kiểm duyệt và vận hành nội dung.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập và còn quyền bình luận.
- Phim đang mở bình luận.
- Người dùng đang ở đúng phạm vi series hoặc tập.

### Acceptance Criteria

1. Người dùng nhập được văn bản và emoji trong ô bình luận.
2. Người dùng có thể đánh dấu “Bình luận có Spoiler” trước khi gửi.
3. Hệ thống liên kết bình luận với đúng series hoặc tập hiện tại.
4. Hệ thống gửi nội dung qua luồng kiểm duyệt tương ứng với cấu hình của phim.
5. Ở chế độ hiển thị ngay, bình luận an toàn xuất hiện sau khi AI chấp nhận.
6. Ở chế độ chờ duyệt, tác giả nhìn thấy bình luận cùng trạng thái chờ; người khác chưa nhìn thấy.
7. Bình luận Spoiler được che và hiển thị cảnh báo “Spoiler — Nhấn để xem” cho người xem khác.
8. Bình luận hiển thị tên theo một trong hai phương án: số thuê bao đã che bốn số cuối hoặc nickname đã được chấp nhận.
9. Không hiển thị đầy đủ số điện thoại hoặc dữ liệu định danh nhạy cảm trong bình luận.
10. Sau khi gửi thành công, nội dung ô nhập được xóa và hệ thống phản hồi trạng thái rõ ràng.
11. Khi gửi lỗi, hệ thống không tạo bản ghi trùng và cho phép người dùng thử lại.
12. Người không có quyền bình luận không thể gửi nội dung qua giao diện hoặc API.

### Quy tắc nghiệp vụ

- Chỉ tài khoản đăng nhập mới được đăng bình luận.
- Nội dung phải tuân thủ chính sách cộng đồng và được kiểm duyệt.
- Không cho phép tải ảnh/video cá nhân trong luồng bình luận.
- Giới hạn ký tự, URL và tần suất đăng cần được cấu hình thay vì cố định trong giao diện.

### Phụ thuộc

- EP03 — An toàn và kiểm duyệt.
- Hệ thống tài khoản/nickname MyTV.

### Điểm cần PO chốt

- Độ dài tối thiểu/tối đa.
- Có cho phép bình luận chỉ chứa emoji hay không.
- Có cho phép URL hay không và danh sách domain được phép.
- Quy tắc tạo, đổi và kiểm duyệt nickname.

---

