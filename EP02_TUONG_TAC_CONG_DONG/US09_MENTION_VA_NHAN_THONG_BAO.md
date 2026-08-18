# US09 — Mention và nhận thông báo

> Thuộc EP02 — Tương tác cộng đồng
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là người dùng đã đăng nhập**, tôi muốn mention tài khoản phù hợp và nhận thông báo khi có người reply/mention mình, để tiếp tục cuộc trò chuyện liên quan.

### Ưu tiên

**Should**

### Acceptance Criteria

1. User nhập `@` để chọn tài khoản hợp lệ; mention được lưu theo account ID, không chỉ chuỗi nickname.
2. Danh sách gợi ý ưu tiên **user đã tham gia thread hiện tại**, sau đó mở rộng sang user đã tham gia **phim/tập hiện tại**; không mặc định tìm toàn bộ user MyTV.
3. Gợi ý mention không làm lộ full phone/PII; dùng nickname hoặc identity hiển thị hợp lệ.
4. Khi reply/mention đã đủ điều kiện Hiển thị, người nhận nhận **push + in-app notification** nếu công tắc Thông báo cộng đồng đang bật và hệ điều hành cho phép push.
5. Nếu cùng sự kiện vừa Reply vừa Mention cùng một user, chỉ tạo một notification phù hợp.
6. Không gửi notification cho hành động của chính user.
7. Nội dung Chờ duyệt không tạo notification cho người khác trước khi được Hiển thị.
8. User có **một công tắc chung trong Cài đặt** để bật/tắt toàn bộ thông báo tương tác cộng đồng; không có switch riêng Reply/Mention.
9. Công tắc cộng đồng chỉ ảnh hưởng Reply, Mention, huy hiệu và thông báo tương tác tương tự. Các thông báo bắt buộc về **Từ chối/Ẩn/Xóa, khóa bình luận/khóa tài khoản, appeal và kết quả Report** không bị tắt bởi setting này.
10. In-app notification cộng đồng được lưu **90 ngày**.
11. Bấm notification mở đúng phim/tập/thread/comment; nếu target không còn hợp lệ, không lộ dữ liệu và dùng fallback phù hợp.
12. Payload push không lộ Spoiler hoặc PII nhạy cảm trên màn hình khóa.

### Quy tắc nghiệp vụ

- Một switch chung cho notification cộng đồng.
- Notification moderation/chế tài là thông báo nghiệp vụ bắt buộc và độc lập với switch cộng đồng.
- Retention in-app: 90 ngày.

### Điểm cần PO chốt

- Không còn blocker PO cho mention/notification trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US09-001 | Suggestion | Thread có U2/U3, phim có U4 | Nhập `@` | Ưu tiên U2/U3 trước, sau đó U4; không mở search toàn hệ thống mặc định. |
| TC-US09-002 | Identity | U2 đổi nickname | Mở mention cũ | Quan hệ vẫn trỏ đúng account ID. |
| TC-US09-003 | Notification | U1 reply/mention U2, switch bật | Publish nội dung | U2 nhận push + in-app nếu push OS cho phép. |
| TC-US09-004 | Preference | U2 tắt switch cộng đồng | Tạo Reply/Mention | Không tạo notification tương tác mới cho U2. |
| TC-US09-005 | Mandatory bypass | U2 tắt switch cộng đồng | Comment U2 bị Từ chối/Ẩn/Xóa hoặc account bị sanction | Notification nghiệp vụ bắt buộc vẫn được gửi theo rule của US14/US16. |
| TC-US09-006 | Dedup | Một reply vừa mention U2 | Publish | Không gửi hai notification trùng. |
| TC-US09-007 | Self | U1 reply/mention chính mình | Publish | Không gửi self-notification. |
| TC-US09-008 | Moderation | Nội dung Chờ duyệt | Theo dõi notification | Không gửi cho người khác trước khi nội dung public. |
| TC-US09-009 | Retention | Có notification >/<90 ngày | Mở notification center | Chỉ dữ liệu còn trong retention theo policy được giữ. |
| TC-US09-010 | Deep link/privacy | Target bị Ẩn/Xóa hoặc mất quyền | Bấm notification cũ | Không lộ nội dung; hiển thị fallback hợp lệ. |
