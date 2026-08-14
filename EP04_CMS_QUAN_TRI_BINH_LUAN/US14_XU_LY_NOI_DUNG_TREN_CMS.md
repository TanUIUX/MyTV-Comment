# US14 — Xử lý nội dung trên CMS

> Thuộc EP04 — CMS quản trị bình luận
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là Admin kiểm duyệt**, tôi muốn duyệt, từ chối, ẩn, xóa, xử lý Report/Flag và điều chỉnh Spoiler, để thực thi chính sách cộng đồng một cách nhất quán.

### Giá trị

- Cho phép xử lý toàn bộ vòng đời kiểm duyệt tại một nơi.
- Giảm rủi ro nội dung vi phạm tiếp tục hiển thị.
- Giữ lại bằng chứng và lý do cho việc kiểm tra sau này.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Admin có quyền kiểm duyệt tương ứng.
- Bình luận/reply tồn tại và chưa bị xóa vĩnh viễn.

### Acceptance Criteria — Hành động kiểm duyệt

1. Admin có thể Duyệt nội dung đang Chờ duyệt để chuyển sang Hiển thị.
2. Admin có thể Từ chối nội dung đang Chờ duyệt để nội dung không được công khai.
3. Admin có thể Ẩn nội dung đang Hiển thị mà không xóa dữ liệu audit.
4. Admin có thể Xóa mềm bình luận/reply vi phạm sau bước xác nhận.
5. Khi Admin xóa bình luận gốc, toàn bộ thread không còn hiển thị công khai.
6. Mỗi thao tác yêu cầu lý do theo danh mục được cấu hình hoặc ghi chú bổ sung khi cần.
7. CMS kiểm tra lại trạng thái hiện tại trước khi lưu để tránh hai Admin ghi đè quyết định của nhau mà không cảnh báo.

### Acceptance Criteria — Report

1. Admin xem được danh sách Report, người gửi, lý do, thời gian và phiên bản nội dung bị báo cáo.
2. Admin có thể chọn Bỏ qua Report nếu bình luận không vi phạm.
3. Admin có thể Duyệt giữ nguyên, Ẩn hoặc Xóa nội dung tùy kết luận.
4. Hệ thống lưu lịch sử từng Report và kết quả xử lý.
5. Nhiều Report không tự động thay đổi trạng thái hiển thị trước quyết định CMS.

### Acceptance Criteria — Flag

1. Admin có thể gắn cờ nội bộ với các lý do: Spoiler, Spam/quảng cáo, Ngôn từ xúc phạm, Nội dung không phù hợp và Vi phạm khác.
2. Admin có thể cập nhật trạng thái theo dõi và bỏ cờ khi hoàn tất.
3. Flag chỉ phục vụ vận hành nội bộ và không hiển thị cho người dùng.
4. Hệ thống lưu người gắn cờ, thời gian, lý do và người hoàn tất xử lý.

### Acceptance Criteria — Spoiler

1. Admin có thể thêm trạng thái Spoiler cho bình luận chưa được tác giả đánh dấu.
2. Admin có thể bỏ trạng thái Spoiler nếu xác định không cần thiết.
3. Thay đổi được phản ánh trên ứng dụng người xem và lưu audit.
4. Nội dung bị Admin đánh dấu Spoiler được che theo cùng trải nghiệm với Spoiler do người dùng chọn.

### Quy tắc nghiệp vụ

- Admin xử lý theo chính sách cộng đồng, không dựa trên quan điểm cá nhân.
- Ý kiến trái chiều nhưng hợp lệ không bị xóa chỉ vì tiêu cực.
- Hành động phải được phân quyền và lưu audit.
- Xóa tại CMS là xóa mềm trong thời hạn lưu trữ 90 ngày, trừ quy trình xóa vĩnh viễn được phê duyệt riêng.

### Phụ thuộc

- US10 — Report bình luận.
- US11 và US12 — AI và trạng thái.
- US16 — Audit log và xử lý tài khoản.

### Điểm cần PO chốt

- Danh mục lý do chuẩn cho từng hành động.
- Có hỗ trợ thao tác hàng loạt hay chỉ từng bình luận trong MVP.
- Có thông báo cho tác giả khi nội dung bị từ chối/ẩn/xóa hay không.
- Quy trình hoàn tác một quyết định sai.

---

