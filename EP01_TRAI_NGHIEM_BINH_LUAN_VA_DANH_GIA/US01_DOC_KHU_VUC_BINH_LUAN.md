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
5. Khi người chưa đăng nhập chọn một thao tác yêu cầu tương tác, hệ thống hiển thị luồng đăng nhập.
6. Sau khi đăng nhập thành công, hệ thống đưa người dùng trở lại đúng phim/tập; nếu khả thi, tiếp tục thao tác mà người dùng vừa chọn.
7. Khi phim bị đóng bình luận, toàn bộ khu vực Bình luận không hiển thị. Quy tắc chi tiết được quản lý tại US12 và US15.

### Quy tắc nghiệp vụ

- Áp dụng nguyên tắc “Mở để đọc — Đăng nhập để tương tác”.
- Chỉ hiển thị dữ liệu mà người xem có quyền xem.
- Việc không đăng nhập không làm thay đổi thứ tự hoặc nội dung bình luận công khai.

### Phụ thuộc

- Dịch vụ nội dung/phim.
- Hệ thống đăng nhập MyTV.
- US12 — Quản lý trạng thái và phạm vi hiển thị bình luận.

### Ngoài phạm vi

- Cá nhân hóa thứ tự bình luận theo từng người xem.
- Bình luận trên nội dung ngoài Phim truyện trong giai đoạn hiện tại.

---

