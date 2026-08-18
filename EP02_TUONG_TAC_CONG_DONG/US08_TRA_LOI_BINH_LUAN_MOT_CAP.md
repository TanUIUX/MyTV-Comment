# US08 — Trả lời bình luận một cấp

> Thuộc EP02 — Tương tác cộng đồng
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là người dùng đã đăng nhập**, tôi muốn trả lời trực tiếp một bình luận, để tham gia cuộc trò chuyện mà vẫn giữ cấu trúc hội thoại dễ theo dõi.

### Ưu tiên

**Must**

### Acceptance Criteria

1. User có thể Reply một comment gốc đang Hiển thị và cho phép tương tác.
2. Reply luôn được lưu ở **một cấp** dưới root comment; khi trả lời một reply, dữ liệu mới vẫn thuộc root và UI có thể mention người được trả lời để giữ ngữ cảnh.
3. Reply hỗ trợ text, emoji, Spoiler và timestamp theo cùng rule content của comment.
4. Reply tối thiểu **1 ký tự hợp lệ hoặc 1 emoji**, tối đa **1000 ký tự**; emoji-only hợp lệ.
5. URL trong reply chỉ hợp lệ với `mytv.com.vn` hoặc subdomain hợp lệ; áp dụng cùng hostname validation US04.
6. Comment + reply dùng chung rate limit **5 nội dung/1 phút/user**.
7. Reply đi qua cùng moderation theo US11.
8. Reply công khai làm tăng tổng số bình luận; reply Chờ duyệt/Từ chối/Ẩn/Xóa không làm tăng số công khai.
9. Mỗi root comment ban đầu hiển thị tối đa **3 reply**.
10. Khi bấm “Xem thêm phản hồi”, mỗi lần tải **tối đa 10 reply**; nếu phần còn lại <10 thì tải toàn bộ phần còn lại.
11. Xóa một reply riêng lẻ làm reply biến mất hoàn toàn khỏi UI, **không để placeholder**.
12. Root không còn public thì thread không được hiển thị độc lập khỏi root. **Admin Ẩn root** làm toàn thread tạm không public nhưng không đổi moderation state của reply; Undo Ẩn root làm các reply hợp lệ hiện lại theo state riêng.
13. Nếu tác giả root bị **Account Lock**, root và toàn bộ thread tạm không public; reply của user khác **không bị đổi moderation state/không bị coi là vi phạm**. Khi root author được mở khóa, thread hiện lại nếu từng item vẫn hợp lệ.
14. Nếu user **self-delete root**, root + toàn bộ reply bị cascade soft-delete theo US05. Nếu CMS/Admin Xóa mềm root, thread không public và có thể Undo theo rule US14.
15. Guest chọn Reply được chuyển sang login; không tạo dữ liệu trước xác thực.

### Quy tắc nghiệp vụ

- Reply depth = 1.
- Reply không tham gia danh sách root comment khi sort.
- Pagination reply: initial 3, sau đó batch tối đa 10 cho tới hết.
- Tất cả validation text/emoji/URL/rate limit đồng bộ với US04.
- **Visibility cascade** do Admin Hide hoặc Account Lock của root author không tự thay đổi state moderation của reply.
- **Delete cascade** là lifecycle riêng: self-delete root soft-delete toàn thread; CMS/Admin soft-delete root tuân US14.
- Reply không được hiển thị như content độc lập khi root đang non-public.

### Điểm cần PO chốt

- Không còn blocker PO cho Reply trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US08-001 | Functional | U1 login; C1 public | Reply C1 | Reply đúng root/scope và đi moderation. |
| TC-US08-002 | Depth | C1 có R1 | Reply R1 | Reply mới vẫn là cấp 1 dưới C1, không tạo cây sâu. |
| TC-US08-003 | Boundary | 999/1000/1001 ký tự | Gửi reply | 999/1000 hợp lệ; 1001 bị chặn. |
| TC-US08-004 | Emoji/minimum | Emoji-only, whitespace, 1 ký tự | Gửi | Emoji-only/1 ký tự hợp lệ; whitespace bị chặn. |
| TC-US08-005 | URL | MyTV domain/subdomain và domain giả | Gửi | Chỉ hostname MyTV hợp lệ được nhận. |
| TC-US08-006 | Rate limit | U1 đã tạo tổng 5 comment/reply trong phút | Gửi thêm reply | Bị rate-limit, không tạo record thứ 6. |
| TC-US08-007 | Initial display | C1 có >3 reply public | Mở C1 | Ban đầu hiển thị 3 reply. |
| TC-US08-008 | Load more | C1 còn 24 reply sau initial | Bấm Xem thêm nhiều lần | Lần lượt tải tối đa 10/10/phần còn lại; không trùng/bỏ sót. |
| TC-US08-009 | Delete reply | R1 bị user/Admin xóa | Refresh thread | R1 biến mất hoàn toàn, không placeholder; reply khác giữ nguyên. |
| TC-US08-010 | Admin hide root | C1 public có R1/R2 hợp lệ | Admin Ẩn C1 rồi Undo Ẩn | Khi Ẩn: toàn thread không public, R1/R2 giữ state; Undo: các item hợp lệ hiện lại, item có moderation riêng vẫn theo state riêng. |
| TC-US08-011 | Root author Account Lock | U1 sở hữu C1; U2/U3 sở hữu R1/R2 | Account Lock U1 rồi mở khóa | Khi khóa: cả thread không public nhưng R1/R2 không đổi state; mở khóa: thread hiện lại nếu từng item còn hợp lệ. |
| TC-US08-012 | Self-delete root | U1 sở hữu C1 có reply của user khác | U1 self-delete C1 | C1 và toàn bộ reply cascade soft-delete theo US05; không tách reply thành nội dung độc lập. |
| TC-US08-013 | Authentication | Guest | Chọn Reply | Yêu cầu login; không tạo reply trước auth. |
