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
12. Xóa root comment làm toàn bộ thread biến mất; Ẩn/khóa root comment ngăn tạo reply mới.
13. Guest chọn Reply được chuyển sang login; không tạo dữ liệu trước xác thực.

### Quy tắc nghiệp vụ

- Reply depth = 1.
- Reply không tham gia danh sách root comment khi sort.
- Pagination reply: initial 3, sau đó batch tối đa 10 cho tới hết.
- Tất cả validation text/emoji/URL/rate limit đồng bộ với US04.

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
| TC-US08-010 | Root lifecycle | C1 bị Xóa hoặc Ẩn/khóa | Mở thread/thử reply | Xóa: thread không public; Ẩn/khóa: không tạo reply mới. |
| TC-US08-011 | Authentication | Guest | Chọn Reply | Yêu cầu login; không tạo reply trước auth. |
