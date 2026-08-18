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
2. Nội dung bình luận tối đa **500 ký tự**; UI và API cùng áp dụng một giới hạn.
3. Người dùng có thể đánh dấu “Bình luận có Spoiler” trước khi gửi.
4. Hệ thống liên kết bình luận với đúng series hoặc tập hiện tại.
5. Hệ thống gửi nội dung qua luồng kiểm duyệt tương ứng với cấu hình của phim.
6. Ở chế độ hiển thị ngay, bình luận an toàn xuất hiện sau khi AI chấp nhận.
7. Ở chế độ chờ duyệt, tác giả nhìn thấy bình luận cùng trạng thái chờ; người khác chưa nhìn thấy.
8. Bình luận Spoiler được che và hiển thị cảnh báo “Spoiler — Nhấn để xem” cho người xem khác.
9. Bình luận hiển thị tên theo một trong hai phương án: số thuê bao đã che bốn số cuối hoặc nickname đã được chấp nhận.
10. Không hiển thị đầy đủ số điện thoại hoặc dữ liệu định danh nhạy cảm trong bình luận.
11. Sau khi gửi thành công, nội dung ô nhập được xóa và hệ thống phản hồi trạng thái rõ ràng.
12. Khi gửi lỗi, hệ thống không tạo bản ghi trùng và cho phép người dùng thử lại.
13. Người không có quyền bình luận không thể gửi nội dung qua giao diện hoặc API.

### Quy tắc nghiệp vụ

- Chỉ tài khoản đăng nhập mới được đăng bình luận.
- Nội dung tối đa 500 ký tự.
- Nội dung phải tuân thủ chính sách cộng đồng và được kiểm duyệt.
- Không cho phép tải ảnh/video cá nhân trong luồng bình luận.
- Quy tắc URL và tần suất đăng cần được cấu hình/chốt riêng, không làm thay đổi giới hạn 500 ký tự đã thống nhất.

### Phụ thuộc

- EP03 — An toàn và kiểm duyệt.
- Hệ thống tài khoản/nickname MyTV.

### Điểm cần PO chốt

- Có cho phép bình luận chỉ chứa emoji hay không.
- Có cho phép URL hay không và danh sách domain được phép.
- Quy tắc tạo, đổi và kiểm duyệt nickname.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra luồng tạo bình luận đúng scope, đúng danh tính hiển thị, tuân giới hạn 500 ký tự, đi qua kiểm duyệt, xử lý Spoiler và không tạo dữ liệu trùng hoặc lộ thông tin nhạy cảm.

### Rủi ro chính

- Bình luận gắn sai series/tập hoặc hiển thị trước khi được phép.
- UI cho phép quá 500 ký tự trong khi API chặn hoặc ngược lại.
- Lộ số thuê bao đầy đủ/nickname chưa được chấp nhận.
- Retry tạo bản ghi trùng hoặc nội dung cá nhân đi qua API dù UI đã chặn.

### Dữ liệu kiểm thử

Tài khoản có nickname hợp lệ, tài khoản chưa có nickname, phim ở Chế độ 1/Chế độ 2/Đóng; nội dung an toàn, nghi ngờ, Spoiler, emoji-only, URL, 499/500/501 ký tự và request retry.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US04-001 | Functional | U1 đăng nhập, phim Mở | Nhập text và emoji rồi gửi ở scope E1 | Comment được tạo gắn đúng phim/tập; UI phản hồi thành công và xóa nội dung ô nhập. |
| TC-US04-002 | Spoiler | U1 có nội dung có Spoiler | Bật cờ Spoiler, gửi và mở bằng tài khoản khác | Comment công khai bị che, hiển thị cảnh báo; chỉ mở nội dung khi người xem chủ động chọn. |
| TC-US04-003 | Scope | Đang ở series và E1 | Gửi một comment ở mỗi scope | Hai comment có đúng content/episode scope; không xuất hiện chéo. |
| TC-US04-004 | Moderation | Chế độ 1; AI trả an toàn và nghi ngờ | Gửi hai nội dung tương ứng | Nội dung an toàn chuyển Hiển thị theo chính sách; nội dung nghi ngờ Chờ duyệt và chỉ tác giả thấy. |
| TC-US04-005 | Moderation | Chế độ 2 | Gửi nội dung an toàn về kỹ thuật | Comment không công khai trước khi Admin duyệt; tác giả thấy trạng thái Chờ duyệt. |
| TC-US04-006 | Identity/privacy | U1 có số thuê bao và nickname khác nhau | Hiển thị comment trên app/web và kiểm tra response | Chỉ dùng nickname hợp lệ hoặc số đã che 4 số cuối; không có số đầy đủ/PII không cần thiết. |
| TC-US04-007 | Boundary | Chuẩn bị nội dung 499, 500 và 501 ký tự | Gửi lần lượt qua UI và API | 499/500 ký tự được chấp nhận nếu thỏa các rule khác; 501 ký tự bị chặn nhất quán, không tạo comment. |
| TC-US04-008 | Validation | Nội dung rỗng, chỉ khoảng trắng, URL không hợp lệ, emoji-only | Gửi từng dữ liệu | Hệ thống áp dụng rule đã cấu hình/chốt; thông báo lỗi rõ và không tạo comment không hợp lệ. |
| TC-US04-009 | Security | Phiên bị logout/không có quyền bình luận | Gửi trực tiếp request tạo comment | API từ chối; không tạo dữ liệu và không thể bypass bằng cách bỏ qua UI. |
| TC-US04-010 | Idempotency | Request gửi chậm hoặc client retry | Bấm gửi nhiều lần/đẩy lại cùng request | Chỉ tạo một comment; trạng thái cuối nhất quán. |
| TC-US04-011 | Error handling | Mock AI timeout hoặc API lỗi | Gửi comment | Hệ thống dùng fallback an toàn; không tự động công khai nếu chưa có quyết định cho phép; người dùng nhận trạng thái thử lại/chờ. |
| TC-US04-012 | Media restriction | Có nút/endpoint upload file cá nhân | Kiểm tra UI và gọi endpoint upload trực tiếp | UI không cung cấp upload; API từ chối media cá nhân trong luồng comment. |

### Điểm cần PO chốt trước khi chốt bộ dữ liệu biên

- Emoji-only, URL và rate limit.
- Hành vi thông báo cho tác giả khi nội dung bị Từ chối.
