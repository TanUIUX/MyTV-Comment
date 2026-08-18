# US04 — Đăng bình luận

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là người dùng đã đăng nhập**, tôi muốn đăng bình luận bằng văn bản, emoji và tùy chọn Spoiler, để chia sẻ cảm nhận về series hoặc tập phim đang xem.

### Ưu tiên

**Must**

### Acceptance Criteria

1. Comment hỗ trợ văn bản, emoji và Spoiler; **emoji-only là nội dung hợp lệ**.
2. Comment tối thiểu **1 ký tự hợp lệ hoặc 1 emoji**, không tính khoảng trắng; tối đa **1000 ký tự**. UI và API dùng cùng giới hạn.
3. URL chỉ hợp lệ khi hostname là `mytv.com.vn` hoặc subdomain thực sự của `mytv.com.vn`; không chấp nhận domain giả như `mytv.com.vn.evil.com`.
4. Mỗi user tối đa **5 comment/reply trong 1 phút**; quota được tính chung giữa comment và reply.
5. Comment được gắn đúng series/tập hiện tại và đi qua moderation theo US11.
6. Ở Chế độ 1, nội dung AI mức Nhẹ/An toàn được Hiển thị; Trung bình vào Chờ duyệt; Nặng bị chặn.
7. Ở Chế độ 2, nội dung không bị chặn vẫn vào Chờ duyệt tới khi Admin duyệt.
8. Spoiler được che với cảnh báo và chỉ mở khi người xem chủ động chọn.
9. Nickname phải **unique không phân biệt hoa/thường**, dài **3–30 ký tự**; cho phép chữ, số, khoảng trắng, `_`, `-`; không cho URL, số điện thoại hoặc ký tự điều khiển.
10. User được đổi nickname không giới hạn số lần; nickname mới/đổi phải qua AI moderation. An toàn dùng ngay; nghi ngờ Chờ duyệt; nặng bị chặn.
11. Trong khi nickname mới Chờ duyệt, tiếp tục dùng nickname hợp lệ cũ. Nếu chưa có nickname hợp lệ, hiển thị số điện thoại đã mask: giữ `0` đầu + 3 số cuối, toàn bộ số giữa thành `*` theo độ dài thực tế, ví dụ `0912345124 → 0******124`.
12. Không hiển thị đầy đủ số điện thoại hoặc PII nhạy cảm trên trải nghiệm người xem.
13. Sau khi gửi thành công, ô nhập được xóa; retry không tạo comment trùng.
14. Người không đăng nhập/không còn quyền bình luận không thể gửi qua UI hoặc API.
15. Không cho upload ảnh/video cá nhân trong luồng comment.

### Quy tắc nghiệp vụ

- Chỉ tài khoản đăng nhập mới được đăng bình luận.
- Giới hạn 1000 ký tự và rate limit 5 nội dung/phút cũng áp dụng Reply tại US08.
- URL được validate theo hostname, không theo chuỗi chứa tên miền.
- Nickname đang Chờ duyệt không được public trước khi có quyết định hợp lệ.
- Khi comment bị Từ chối/Ẩn/Xóa bởi Admin, tác giả được xem lý do trong app và nhận notification theo US14/US16.

### Điểm cần PO chốt

- Không còn blocker PO cho validation comment/nickname trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US04-001 | Functional | U1 đăng nhập | Gửi text/emoji ở đúng scope | Comment được tạo đúng scope và xử lý moderation. |
| TC-US04-002 | Emoji | U1 đăng nhập | Gửi comment chỉ có 1 emoji | Hợp lệ nếu qua moderation. |
| TC-US04-003 | Minimum | Chuẩn bị rỗng, whitespace, 1 ký tự | Gửi | Rỗng/whitespace bị chặn; 1 ký tự hợp lệ được nhận. |
| TC-US04-004 | Boundary | 999/1000/1001 ký tự | Gửi qua UI/API | 999/1000 hợp lệ; 1001 bị chặn nhất quán. |
| TC-US04-005 | URL | Link `mytv.com.vn`, `www.mytv.com.vn`, `support.mytv.com.vn`, `mytv.com.vn.evil.com`, domain khác | Gửi | Chỉ domain MyTV và subdomain thực được phép. |
| TC-US04-006 | Rate limit | U1 gửi comment/reply liên tiếp | Gửi 5 rồi nội dung thứ 6 trong rolling 1 phút | 5 nội dung đầu có thể được nhận; nội dung thứ 6 bị rate-limit và không tạo record. |
| TC-US04-007 | Spoiler | Comment có Spoiler | Gửi và mở bằng U2 | Nội dung bị che; chỉ mở khi U2 chủ động chọn. |
| TC-US04-008 | Moderation | Mode 1 với mức Nhẹ/Trung bình/Nặng | Gửi từng mẫu | Nhẹ Hiển thị, Trung bình Chờ duyệt, Nặng bị chặn. |
| TC-US04-009 | Mode 2 | Nội dung không bị chặn | Gửi | Vào Chờ duyệt; chưa public trước Admin. |
| TC-US04-010 | Nickname uniqueness | Có `MyTVFan` | User khác thử `mytvfan`/`MYTVFAN` | Bị coi là trùng. |
| TC-US04-011 | Nickname format | Nickname 2/3/30/31 ký tự; có URL/phone/control char | Lưu nickname | Chỉ dữ liệu đúng 3–30 và charset/rule hợp lệ được nhận. |
| TC-US04-012 | Nickname moderation | Đổi nickname an toàn/nghi ngờ/nặng | Thực hiện đổi | An toàn dùng ngay; nghi ngờ giữ nickname cũ trong lúc Chờ duyệt; nặng bị chặn. |
| TC-US04-013 | Fallback identity | U1 chưa có nickname hợp lệ, phone `0912345124` | Mở comment | Hiển thị `0******124`, không lộ số đầy đủ. |
| TC-US04-014 | Security/idempotency | Logout hoặc retry request | Gửi qua API | Không bypass auth; retry không tạo trùng. |
| TC-US04-015 | Media restriction | Thử upload ảnh/video cá nhân | UI/API | Không có luồng upload; API từ chối. |
