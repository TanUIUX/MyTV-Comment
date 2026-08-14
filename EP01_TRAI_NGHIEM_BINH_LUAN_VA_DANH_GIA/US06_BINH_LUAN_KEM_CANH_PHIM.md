# US06 — Bình luận kèm cảnh phim

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người dùng đang xem phim**, tôi muốn đính kèm một frame hoặc đoạn video ngắn từ phim vào bình luận, để diễn đạt rõ cảnh mình đang thảo luận mà không tải media cá nhân lên.

### Giá trị

- Làm cuộc thảo luận trực quan và gắn chặt với nội dung MyTV.
- Hạn chế rủi ro từ việc người dùng tải file cá nhân.
- Tạo nội dung có khả năng chia sẻ và lan truyền.

### Ưu tiên

**Could**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập và đang phát nội dung được phép sử dụng tính năng.
- Player cung cấp content ID, episode ID và timestamp hiện tại.
- Quyền khai thác nội dung cho phép tạo lại frame/clip.

### Acceptance Criteria

1. Người dùng có thể chọn lấy một frame tại thời điểm đang xem.
2. Người dùng có thể chọn đoạn video ngắn với thời lượng tối đa 5 giây theo giới hạn hệ thống.
3. Hệ thống không hiển thị chức năng tải ảnh/video từ thiết bị cá nhân.
4. Bình luận chỉ lưu content ID, episode ID, timestamp, loại media và thông tin thời lượng cần thiết; không tạo file media riêng theo yêu cầu sản phẩm.
5. Khi người khác xem bình luận, hệ thống dựng lại frame/clip từ nội dung và timestamp tương ứng.
6. Media chỉ hiển thị cho người dùng có quyền xem nội dung nguồn.
7. Khi nội dung nguồn không còn quyền phát, không tồn tại hoặc timestamp không hợp lệ, hệ thống hiển thị trạng thái thay thế phù hợp và không làm hỏng bình luận văn bản.
8. Người dùng vẫn có thể đánh dấu bình luận chứa cảnh phim là Spoiler.
9. Frame/clip phải đi qua quy tắc kiểm duyệt và bản quyền áp dụng cho bình luận.
10. Hành vi phải hoạt động nhất quán trên các thiết bị được hỗ trợ; thiết bị không hỗ trợ cần có phương án fallback.

### Quy tắc nghiệp vụ

- Không cho phép media cá nhân.
- Clip không dài quá 5 giây.
- Không cho phép lấy timestamp ngoài nội dung/tập đang bình luận.
- Việc chia sẻ cảnh phim ra ngoài MyTV thuộc US18 và cần kiểm tra quyền riêng.

### Phụ thuộc

- Player MyTV và dịch vụ nội dung.
- DRM/quản lý quyền nội dung.
- US04 — Đăng bình luận.
- US18 — Chia sẻ bình luận và cảnh phim.

### Rủi ro và điểm cần PO chốt

- Khả năng dựng media theo timestamp trên từng nền tảng.
- Hành vi khi nội dung thay source, bị cắt dựng hoặc hết hạn quyền phát.
- Danh sách nội dung không được phép tạo frame/clip.
- Có cần watermark MyTV hay không.

---

