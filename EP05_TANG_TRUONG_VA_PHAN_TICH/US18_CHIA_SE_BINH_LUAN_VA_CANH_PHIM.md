# US18 — Chia sẻ bình luận và cảnh phim

> Thuộc EP05 — Tăng trưởng và phân tích
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người dùng đã đăng nhập**, tôi muốn chia sẻ một bình luận hoặc cảnh phim đang thảo luận ra mạng xã hội với liên kết quay lại MyTV, để giới thiệu nội dung và mời người khác tham gia.

### Giá trị

- Tạo nguồn truy cập quay lại MyTV từ bên ngoài.
- Tăng khả năng lan truyền của phim và thảo luận cộng đồng.
- Kết nối hoạt động bình luận với chiến dịch mạng xã hội.

### Ưu tiên

**Could**

### Điều kiện tiên quyết

- Bình luận đang Hiển thị và được phép chia sẻ.
- Người dùng có quyền xem/chia sẻ nội dung nguồn.
- Nền tảng đích hoặc share sheet được hỗ trợ.

### Acceptance Criteria

1. Người dùng có thể chọn Chia sẻ trên một bình luận đang hiển thị.
2. Người dùng có thể chọn chia sẻ nội dung bình luận hoặc cảnh phim gắn với bình luận nếu cảnh đó đủ quyền.
3. Nội dung chia sẻ có liên kết quay lại đúng phim/tập trên MyTV.
4. Khi có thể, deep link mở đúng ngữ cảnh bình luận/thread; nếu không, mở trang phim/tập phù hợp.
5. Hệ thống hỗ trợ các kênh mục tiêu như Facebook, TikTok, Zalo hoặc share sheet của thiết bị theo khả năng tích hợp được xác minh.
6. Nội dung Spoiler không được hiển thị trực tiếp trong preview chia sẻ nếu chưa có cơ chế che phù hợp.
7. Bình luận bị ẩn/xóa sau khi chia sẻ không còn được hiển thị qua deep link; người nhận vẫn có thể được chuyển tới phim nếu còn quyền.
8. Cảnh phim chỉ được chia sẻ khi chính sách bản quyền/DRM cho phép.
9. Link chia sẻ có metadata phù hợp và không làm lộ thông tin cá nhân nhạy cảm.
10. Hệ thống ghi nhận sự kiện chia sẻ, kênh đích và lượt mở link để phục vụ phân tích trong phạm vi cho phép.

### Quy tắc nghiệp vụ

- Mọi nội dung chia sẻ phải gắn đường dẫn quay về MyTV.
- Không chia sẻ nội dung đang Chờ duyệt, Từ chối, Ẩn hoặc Xóa.
- Không mặc định coi mọi cảnh phim đều được phép chia sẻ ra ngoài.
- Việc tích hợp trực tiếp từng mạng xã hội phụ thuộc API/chính sách nền tảng; share sheet là phương án fallback.

### Phụ thuộc

- US06 — Bình luận kèm cảnh phim.
- Dịch vụ deep link và attribution.
- DRM/quản lý quyền nội dung.
- US19 — Thống kê.

### Rủi ro và điểm cần PO chốt

- Danh sách kênh MVP và mức tích hợp từng kênh.
- Mẫu nội dung/preview và watermark.
- Quyền chia sẻ theo từng phim/nhà cung cấp.
- Thời gian hiệu lực của deep link và hành vi với người chưa cài ứng dụng.

---

