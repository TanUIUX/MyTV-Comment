# US10 — Report bình luận vi phạm

> Thuộc EP03 — An toàn và kiểm duyệt
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người dùng đã đăng nhập**, tôi muốn báo cáo một bình luận không phù hợp theo lý do cụ thể, để Admin xem xét và bảo vệ môi trường cộng đồng.

### Giá trị

- Bổ sung tín hiệu cộng đồng cho quá trình kiểm duyệt.
- Giúp phát hiện các nội dung AI bỏ sót.
- Tạo cơ chế phản hồi minh bạch cho người dùng.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập và còn quyền tương tác.
- Bình luận/reply đang tồn tại và người dùng có quyền xem.

### Acceptance Criteria

1. Người dùng có thể chọn Report trên một bình luận hoặc reply đang hiển thị.
2. Hệ thống hiển thị các lý do: Spoiler, Spam/quảng cáo, Xúc phạm, Nội dung không phù hợp, Sai thông tin và Khác.
3. Khi chọn “Khác”, hệ thống cho phép nhập mô tả bổ sung theo giới hạn được cấu hình.
4. Người dùng phải chọn ít nhất một lý do hợp lệ trước khi gửi.
5. Sau khi gửi thành công, hệ thống xác nhận đã tiếp nhận Report.
6. Report được liên kết với người báo cáo, bình luận, phiên bản nội dung, lý do và thời gian gửi.
7. Report được chuyển vào CMS để Admin xử lý.
8. Bình luận vẫn hiển thị sau khi nhận Report cho đến khi CMS hoặc cơ chế kiểm duyệt đưa ra quyết định khác.
9. Số lượng Report lớn không tự động ẩn/xóa bình luận theo quyết định hiện tại.
10. Hệ thống ngăn một tài khoản gửi lặp lại cùng một Report cho cùng một bình luận nếu chưa có chính sách cho phép báo cáo lại.
11. Người dùng không nhìn thấy danh tính người đã Report một bình luận.
12. Người chưa đăng nhập chọn Report được chuyển sang luồng đăng nhập.

### Quy tắc nghiệp vụ

- Report là tín hiệu để Admin đánh giá, không phải kết luận vi phạm.
- Không công khai số Report trên giao diện người xem.
- Dữ liệu Report phải được giữ cùng lịch sử xử lý để audit.
- Cần có rate limit để tránh lạm dụng chức năng Report.

### Phụ thuộc

- US14 — Xử lý nội dung trên CMS.
- US16 — Quản lý người dùng vi phạm và audit log.

### Điểm cần PO chốt

- Có thông báo kết quả xử lý cho người Report hay không.
- Thời gian cho phép báo cáo lại sau khi Admin bỏ qua Report.
- Có cho phép Report bình luận của chính mình hay không.

---

