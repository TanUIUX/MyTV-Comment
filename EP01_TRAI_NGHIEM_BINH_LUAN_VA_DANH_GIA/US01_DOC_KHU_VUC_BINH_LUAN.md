# US01 — Đọc khu vực bình luận

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người xem**, tôi muốn đọc bình luận về nội dung đang xem mà không cần đăng nhập, để tham khảo ý kiến cộng đồng trước hoặc trong khi xem phim.

### Giá trị

- Giảm rào cản tiếp cận cộng đồng.
- Tăng khả năng người dùng chú ý đến phim và các thảo luận nổi bật.
- Tạo điểm chuyển đổi tự nhiên từ người xem chưa đăng nhập sang người dùng tương tác.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Phim được Admin bật tính năng bình luận.
- Hệ thống có ít nhất trạng thái bình luận được phép hiển thị công khai.

### Acceptance Criteria

1. Khi phim đang mở bình luận, người xem chưa đăng nhập truy cập trang chi tiết/xem phim thì nhìn thấy khu vực Bình luận.
2. Người chưa đăng nhập đọc được toàn bộ bình luận và reply đang ở trạng thái công khai.
3. Khu vực Bình luận hiển thị rõ ràng trên cả web và mobile theo thiết kế đã duyệt.
4. Nội dung bị ẩn, bị xóa hoặc đang chờ duyệt không được hiển thị cho người xem khác.
5. Người chưa đăng nhập chỉ được đọc; khi chọn Đăng bình luận, Reply, Like/Unlike, Mention, Report, Rating, Chia sẻ hoặc bất kỳ thao tác tương tác nào khác, hệ thống yêu cầu đăng nhập trước khi tạo dữ liệu tương tác.
6. Sau khi đăng nhập thành công, hệ thống đưa người dùng trở lại **đúng phim/tập/thread/comment** liên quan nhưng **không tự thực hiện action đã chọn trước login**; user phải chủ động thao tác lại sau khi đã xác thực.
7. Khi phim bị đóng bình luận, toàn bộ khu vực Bình luận không hiển thị. Quy tắc chi tiết được quản lý tại US12 và US15.
8. Nếu người dùng mở deep link cũ tới comment/thread trong scope đang **Đóng bình luận**, hệ thống vẫn mở đúng phim/tập nhưng **không hiển thị comment/thread**, hiển thị thông báo **“Khu vực bình luận hiện không khả dụng”**; đây là visibility gate của scope, không đổi target sang trạng thái Ẩn/Xóa. Khi Admin mở lại, deep link cũ hoạt động lại nếu target vẫn hợp lệ.

### Quy tắc nghiệp vụ

- Áp dụng nguyên tắc “Mở để đọc — Đăng nhập để tương tác”.
- Phiên khách không được tạo bình luận, Like, Reply, Mention, Report, Rating, Chia sẻ hoặc bất kỳ bản ghi tương tác cộng đồng nào; việc chỉ đọc không được tính thành một tương tác bình luận.
- Luồng login chỉ giữ **context điều hướng**; không giữ pending action để tự động tạo interaction sau xác thực.
- Chỉ hiển thị dữ liệu mà người xem có quyền xem.
- Việc không đăng nhập không làm thay đổi thứ tự hoặc nội dung bình luận công khai.
- Đóng bình luận là gate theo scope; không đồng nghĩa target deep link bị moderation Ẩn/Xóa.

### Phụ thuộc

- Dịch vụ nội dung/phim.
- Hệ thống đăng nhập MyTV.
- US12 — Quản lý trạng thái và phạm vi hiển thị bình luận.

### Ngoài phạm vi

- Cá nhân hóa thứ tự bình luận theo từng người xem.
- Bình luận trên nội dung ngoài Phim truyện trong giai đoạn hiện tại.

---

## Phân tích kiểm thử

### Mục tiêu

Xác nhận người xem chưa đăng nhập đọc được đúng nội dung công khai, không làm lộ dữ liệu theo trạng thái/quyền, và được đưa qua luồng đăng nhập khi thực hiện tương tác.

### Rủi ro chính

- Lộ bình luận Chờ duyệt, Ẩn hoặc Xóa mềm qua UI, API hoặc cache.
- Phiên khách tạo được dữ liệu tương tác hoặc bị tính nhầm thành tương tác bình luận.
- Mất ngữ cảnh phim/tập/thread sau khi đăng nhập.
- Hệ thống tự thực hiện interaction cũ sau login dù user chưa thao tác lại.
- Khu vực bình luận vẫn xuất hiện khi phim đã Đóng bình luận.
- Deep link cũ làm lộ comment/thread khi scope đang Đóng hoặc làm sai state moderation của target.

### Dữ liệu kiểm thử

Một phim đang Mở bình luận có bình luận gốc, reply, Chờ duyệt, Ẩn và Xóa mềm; một phim Đóng bình luận; tài khoản khách và tài khoản đã đăng nhập.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US01-001 | Functional | Phim đang Mở bình luận | Mở trang chi tiết phim bằng phiên khách | Khu vực Bình luận hiển thị đúng vị trí và có thể mở danh sách. |
| TC-US01-002 | Authorization | Có bình luận công khai, Chờ duyệt, Ẩn và Xóa mềm | Mở danh sách bằng phiên khách; kiểm tra UI và response API công khai | Chỉ bình luận/reply công khai xuất hiện; các trạng thái còn lại không được trả về hoặc hiển thị. |
| TC-US01-003 | Compatibility | Có cùng dữ liệu trên web và mobile | Mở cùng phim trên web và mobile | Khu vực, nội dung và quyền đọc nhất quán; không làm lộ dữ liệu ở một nền tảng. |
| TC-US01-004 | Authentication | Phiên khách; có bình luận công khai | Chọn lần lượt Đăng bình luận, Like, Reply, Mention, Report, Rating và Chia sẻ | Mỗi thao tác yêu cầu đăng nhập; không tạo record/event tương tác cộng đồng trước khi xác thực. |
| TC-US01-005 | Navigation/post-login | Khách đang ở E1/thread T1/comment C1 và chọn Like/Reply | Đăng nhập thành công từ màn hình yêu cầu login | Quay lại đúng E1/T1/C1; Like/Reply **chưa được thực hiện**; user phải bấm lại action sau login. |
| TC-US01-006 | State/Deep link | Phim ở trạng thái Đóng bình luận; có deep link cũ tới C1 | Mở trang chi tiết, gọi API đọc và mở deep link C1 | Toàn bộ khu vực bình luận bị ẩn; deep link mở đúng phim/tập nhưng không hiển thị C1/thread và hiện “Khu vực bình luận hiện không khả dụng”; C1 không bị đổi sang Ẩn/Xóa. |
| TC-US01-007 | Security | Có ID bình luận Chờ duyệt/Ẩn | Gọi trực tiếp API bằng phiên khách với ID nội dung không công khai | API từ chối hoặc trả dữ liệu rỗng theo chuẩn bảo mật; không suy ra nội dung qua mã lỗi/metadata. |
| TC-US01-008 | Reopen deep link | Scope từng Đóng, C1 vẫn hợp lệ | Mở lại scope rồi mở lại deep link cũ | Deep link lại mở đúng C1/thread; không cần phát hành link mới. |

### Điểm cần xác nhận khi chạy test

- Thiết kế breakpoint và vị trí khu vực Bình luận trên từng nền tảng.
