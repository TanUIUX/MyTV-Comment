# US08 — Trả lời bình luận một cấp

> Thuộc EP02 — Tương tác cộng đồng
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người dùng đã đăng nhập**, tôi muốn trả lời trực tiếp một bình luận, để tham gia cuộc trò chuyện mà vẫn giữ cấu trúc hội thoại dễ theo dõi.

### Giá trị

- Tạo thảo luận hai chiều giữa người xem.
- Giúp nội dung trao đổi gắn đúng với bình luận gốc.
- Hạn chế độ phức tạp giao diện bằng mô hình reply một cấp.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập và còn quyền bình luận.
- Bình luận gốc đang hiển thị và cho phép reply.
- Phim đang mở bình luận.

### Acceptance Criteria

1. Người dùng có thể chọn Trả lời trên một bình luận gốc.
2. Reply được liên kết với đúng bình luận gốc, series/tập và nội dung hiện tại.
3. Hệ thống chỉ hỗ trợ một cấp reply bên dưới bình luận gốc.
4. Khi người dùng chọn trả lời một reply, nội dung mới vẫn được lưu ở cấp một dưới bình luận gốc; giao diện có thể tự động mention người được trả lời để giữ ngữ cảnh.
5. Reply hỗ trợ văn bản, emoji và đánh dấu Spoiler theo quy tắc của bình luận.
6. Reply đi qua cùng cơ chế kiểm duyệt với bình luận gốc.
7. Reply công khai mới làm tăng tổng số bình luận của phạm vi hiện tại.
8. Khi số reply vượt giới hạn hiển thị ban đầu, người xem có thể chọn “Xem thêm phản hồi”.
9. Tác giả có thể sửa hoặc xóa reply của mình theo quy tắc tại US05.
10. Nếu bình luận gốc bị xóa, toàn bộ reply trong thread không còn hiển thị.
11. Nếu bình luận gốc bị ẩn hoặc khóa tương tác, hệ thống không cho phép tạo reply mới.
12. Người chưa đăng nhập chọn Trả lời được chuyển sang luồng đăng nhập.

### Quy tắc nghiệp vụ

- Độ sâu tối đa là một cấp.
- Reply được tính vào tổng số bình luận.
- Reply không tham gia danh sách bình luận gốc khi sắp xếp.
- Xóa bình luận gốc làm toàn bộ thread biến mất khỏi trải nghiệm người dùng.

### Phụ thuộc

- US04 — Đăng bình luận.
- US05 — Sửa và xóa bình luận.
- US09 — Mention và nhận thông báo.
- EP03 — An toàn và kiểm duyệt.

### Điểm cần PO chốt

- Số reply hiển thị mặc định trước nút “Xem thêm”.
- Cách hiển thị khi một reply riêng lẻ bị xóa giữa hội thoại.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra reply được gắn đúng thread/scope, luôn giới hạn một cấp, đi qua kiểm duyệt, cập nhật số lượng và xử lý đúng khi comment gốc bị ẩn/xóa.

### Rủi ro chính

- Tạo reply lồng sâu hơn một cấp hoặc gắn sai comment gốc.
- Reply Chờ duyệt làm tăng số công khai/hiển thị sai người.
- Xóa/ẩn comment gốc nhưng vẫn cho reply mới hoặc vẫn lộ thread.

### Dữ liệu kiểm thử

C1 là comment gốc có R1/R2; U1/U2; series/tập khác nhau; phim Mở/Đóng; nội dung text, emoji, Spoiler và kết quả AI khác nhau.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US08-001 | Functional | U1 đăng nhập; C1 công khai | Chọn Trả lời, nhập text và gửi | Reply được lưu dưới C1, đúng series/tập và xuất hiện theo trạng thái kiểm duyệt. |
| TC-US08-002 | Scope | C1 ở E1; đang xem E2 | Thử tạo reply với context E2 hoặc sửa request | Hệ thống từ chối mismatch; reply không bị gắn sang scope khác. |
| TC-US08-003 | Depth | C1 có R1 | Chọn Trả lời trên R1 và gửi | Reply mới vẫn là cấp một dưới C1; không tạo C1→R1→R2. |
| TC-US08-004 | Context | Reply trên R1 | Kiểm tra UI sau khi gửi | Giao diện giữ ngữ cảnh người được trả lời/mention theo thiết kế nhưng cấu trúc dữ liệu vẫn một cấp. |
| TC-US08-005 | Content/moderation | Reply text, emoji và Spoiler; Chế độ 1/2 | Gửi từng loại nội dung | Nội dung hợp lệ được xử lý theo cùng cơ chế comment gốc; Spoiler được che; Chờ duyệt không lộ cộng đồng. |
| TC-US08-006 | Counter | C1 có tổng số hiện tại | Tạo reply công khai rồi refresh/tải thêm | Tổng số tăng đúng một; reply không xuất hiện như comment gốc trong danh sách sắp xếp. |
| TC-US08-007 | Pagination | C1 có nhiều reply hơn giới hạn ban đầu | Chọn Xem thêm phản hồi nhiều lần | Reply không trùng/bỏ sót và vẫn thuộc đúng C1. |
| TC-US08-008 | Delete cascade | C1 có R1/R2 | Xóa/ẩn C1 bằng user/Admin rồi mở thread | Khi xóa, toàn bộ thread không còn công khai; khi ẩn/khóa, không cho reply mới theo chính sách. |
| TC-US08-009 | Ownership | U1 tạo R1; U2 mở R1 | U2 thử sửa/xóa; U1 thử sửa/xóa | U2 bị từ chối; U1 được chuyển theo quy tắc US05. |
| TC-US08-010 | Authentication | Phiên khách | Chọn Trả lời trên C1 | Yêu cầu đăng nhập; không tạo reply trước xác thực. |

### Điểm cần PO chốt

- Số reply hiển thị trước “Xem thêm”.
- Hiển thị placeholder khi một reply bị xóa giữa hội thoại.
