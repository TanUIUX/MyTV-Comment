# US07 — Like và Unlike bình luận

> Thuộc EP02 — Tương tác cộng đồng
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người dùng đã đăng nhập**, tôi muốn Like hoặc Unlike một bình luận, để thể hiện sự đồng tình hoặc yêu thích đối với ý kiến đó.

### Giá trị

- Cung cấp hình thức tương tác nhanh, ít rào cản.
- Tạo tín hiệu cho sắp xếp “Được yêu thích” và bình luận nổi bật.
- Tạo dữ liệu phục vụ huy hiệu và phân tích cộng đồng.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập và còn quyền tương tác.
- Bình luận đang ở trạng thái công khai và cho phép tương tác.

### Acceptance Criteria

1. Người dùng đã đăng nhập có thể Like một bình luận hoặc reply đang hiển thị.
2. Mỗi tài khoản chỉ có một trạng thái Like hiện hành trên mỗi bình luận/reply.
3. Khi đã Like, người dùng có thể Unlike để hủy Like của mình.
4. Nút Like hiển thị đúng trạng thái hiện tại của người dùng.
5. Số Like được cập nhật gần thời gian thực sau thao tác thành công.
6. Hệ thống không tăng/giảm trùng số Like khi người dùng bấm nhiều lần, mạng chậm hoặc gửi lại request.
7. Người chưa đăng nhập chọn Like được chuyển sang luồng đăng nhập.
8. Bình luận bị ẩn, xóa hoặc không còn quyền xem không thể nhận Like mới.
9. Khi thao tác thất bại, giao diện hoàn lại trạng thái đúng và thông báo cho người dùng.
10. Số Like mới được phản ánh trong chế độ sắp xếp Được yêu thích theo độ trễ được thống nhất.

### Quy tắc nghiệp vụ

- Một tài khoản, một Like trên một bình luận/reply.
- Like không tạo thread hoặc nội dung mới.
- Khi bình luận bị xóa mềm, dữ liệu Like được giữ theo chính sách audit nhưng không hiển thị công khai.

### Phụ thuộc

- US02 — Sắp xếp bình luận.
- US17 — Huy hiệu người dùng.
- US19 — Thống kê hoạt động bình luận.

### Điểm cần PO chốt

- Có cho phép tác giả Like bình luận của chính mình hay không.
- Độ trễ tối đa chấp nhận được cho cập nhật số Like và bảng xếp hạng.

---

