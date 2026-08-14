# US16 — Quản lý người dùng vi phạm và audit log

> Thuộc EP04 — CMS quản trị bình luận
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là Admin có thẩm quyền**, tôi muốn xem lịch sử vi phạm, áp dụng chế tài và tra cứu audit log, để xử lý tài khoản nhất quán và có thể kiểm chứng.

### Giá trị

- Hạn chế tái phạm và bảo vệ cộng đồng.
- Tạo bằng chứng minh bạch cho kiểm tra nội bộ/khiếu nại.
- Kiểm soát việc sử dụng quyền Admin.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Admin đã được cấp quyền xử lý tài khoản hoặc xem audit.
- Dữ liệu vi phạm và lịch sử kiểm duyệt được liên kết với tài khoản.

### Acceptance Criteria — Lịch sử và chế tài

1. Admin xem được lịch sử bình luận bị xử lý, Report liên quan, cảnh báo và chế tài trước đó của tài khoản.
2. Admin có thể gửi cảnh báo theo mẫu/lý do được cấu hình.
3. Admin có thể hạn chế quyền bình luận trong một khoảng thời gian xác định.
4. Admin có thể khóa tạm thời hoặc khóa vĩnh viễn theo quyền được cấp và chính sách phê duyệt.
5. CMS yêu cầu lý do, thời hạn và bước xác nhận đối với chế tài ảnh hưởng tài khoản.
6. Khi quyền bình luận bị hạn chế, người dùng không thể đăng, reply, Like, Mention, Report hoặc thực hiện các tương tác bị cấu hình chặn qua giao diện/API.
7. Khi hết hạn hạn chế tạm thời, quyền được khôi phục tự động nếu không có chế tài khác còn hiệu lực.
8. Việc thu hồi huy hiệu khi vi phạm nghiêm trọng có thể được thực hiện qua US17.
9. Các hành động không được làm mất dữ liệu lịch sử cần thiết cho audit.

### Acceptance Criteria — Audit log

1. Hệ thống lưu tối thiểu: Admin thực hiện, thời gian, đối tượng, hành động, lý do, trạng thái trước và trạng thái sau.
2. Audit bao phủ duyệt/từ chối/ẩn/xóa, Report, Flag, Spoiler, ghim, cấu hình phim, cảnh báo và chế tài tài khoản.
3. Admin được phân quyền có thể tìm kiếm/lọc audit theo thời gian, người thao tác, loại hành động và đối tượng.
4. Audit log không thể bị sửa/xóa bởi vai trò vận hành thông thường.
5. Dữ liệu bình luận xóa mềm được giữ 90 ngày theo quyết định hiện tại; audit log tuân theo thời hạn lưu trữ riêng do chính sách dữ liệu quy định.
6. Việc truy cập dữ liệu nhạy cảm trong audit cũng được kiểm soát và ghi nhận khi cần.

### Quy tắc nghiệp vụ

- Chế tài phải tương xứng mức độ vi phạm và theo ma trận chính sách.
- Admin chỉ sử dụng dữ liệu cho mục đích vận hành được phê duyệt.
- Cần phân biệt khóa quyền cộng đồng với khóa toàn bộ tài khoản/thuê bao MyTV.
- Không thu hồi huy hiệu hoặc khóa tài khoản tự động chỉ dựa trên một Report chưa được xác minh.

### Phụ thuộc

- Hệ thống tài khoản và phân quyền MyTV.
- US10 — Report.
- US14 — Xử lý nội dung.
- US17 — Huy hiệu.

### Điểm cần PO chốt

- Ma trận vi phạm → chế tài.
- Phạm vi “khóa tài khoản”: quyền bình luận hay toàn bộ tài khoản MyTV.
- Thời hạn lưu audit theo yêu cầu pháp lý/nội bộ.
- Quy trình khiếu nại và hoàn tác chế tài.

---

