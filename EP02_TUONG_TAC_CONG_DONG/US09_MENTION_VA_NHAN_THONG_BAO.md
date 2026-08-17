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

## Phân tích kiểm thử

### Mục tiêu

Xác nhận mention chỉ trỏ tới tài khoản hợp lệ, thông báo reply/mention được tạo đúng kênh, không trùng/không tự gửi, deep link và quyền xem được kiểm tra.

### Rủi ro chính

- Mention bằng nickname không tồn tại hoặc lưu sai định danh sau khi nickname đổi.
- Gửi trùng push/in-app khi một sự kiện vừa reply vừa mention.
- Deep link làm lộ Spoiler, comment đã xóa hoặc phim người nhận không còn quyền xem.

### Dữ liệu kiểm thử

U1 gửi, U2 nhận; nickname hợp lệ/không hợp lệ; U2 bật/tắt push; comment công khai, Chờ duyệt, đã xóa; sự kiện reply, mention và reply+mention.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US09-001 | Functional | Có U2 là tài khoản hợp lệ | Nhập `@` và chọn U2 từ gợi ý | Mention được tạo với account ID của U2, hiển thị nickname hợp lệ. |
| TC-US09-002 | Negative/privacy | Nhập nickname không tồn tại hoặc ký tự gần giống | Gửi comment không chọn tài khoản hợp lệ; kiểm tra kết quả gợi ý | Không tạo mention giả; không trả số thuê bao đầy đủ/PII trong gợi ý hoặc API. |
| TC-US09-003 | Data integrity | U2 đổi nickname sau khi được mention | Mở comment cũ và kiểm tra notification | Quan hệ vẫn trỏ đúng account ID; hiển thị theo dữ liệu định danh hiện hành được phép. |
| TC-US09-004 | In-app notification | U1 reply comment của U2 | Gửi reply công khai, mở notification center bằng U2 | U2 nhận một thông báo trong ứng dụng với nội dung/ngữ cảnh phù hợp. |
| TC-US09-005 | Push notification | U2 cho phép push | Tạo mention và reply công khai | Có push tương ứng; payload không lộ nội dung Spoiler/PII ngoài chính sách. |
| TC-US09-006 | Preference | U2 tắt push nhưng vẫn bật in-app | Tạo mention/reply | Không gửi push; thông báo trong ứng dụng vẫn được tạo theo thiết kế. |
| TC-US09-007 | Deduplication | Một reply vừa mention U2 | Gửi và kiểm tra notification/event log | Không tạo hai thông báo trùng cho cùng một sự kiện/người nhận. |
| TC-US09-008 | Deep link | Có notification hợp lệ | Bấm notification trên web/mobile | Mở đúng phim, tập và thread/comment liên quan; trạng thái đã đọc được cập nhật nếu có. |
| TC-US09-009 | Access/fallback | Comment bị xóa/ẩn hoặc U2 mất quyền xem phim | Bấm notification cũ | Không lộ nội dung; hiển thị trạng thái thay thế và chuyển tới màn hình hợp lệ nếu chính sách cho phép. |
| TC-US09-010 | Self-notification | U1 mention/reply chính comment của mình | Gửi thao tác và kiểm tra notification U1 | U1 không nhận thông báo cho hành động của chính mình. |
| TC-US09-011 | Moderation | Nội dung mention/reply ở trạng thái Chờ duyệt | Gửi và theo dõi event/notification | Không gửi notification cho người khác trước khi nội dung đủ điều kiện hiển thị. |

### Điểm cần PO chốt

- Phạm vi tài khoản được gợi ý khi nhập `@` và thời gian lưu notification.
- Có cho phép tắt riêng reply/mention hay chỉ tắt push toàn hệ thống.
