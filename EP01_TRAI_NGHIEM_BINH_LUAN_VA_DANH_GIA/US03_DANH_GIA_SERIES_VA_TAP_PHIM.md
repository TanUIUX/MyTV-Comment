# US03 — Đánh giá series và tập phim

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người dùng đã đăng nhập**, tôi muốn đánh giá series hoặc tập phim bằng thang điểm 5 sao và có thể thay đổi đánh giá, để thể hiện cảm nhận của mình với cộng đồng.

### Giá trị

- Cung cấp tín hiệu chất lượng nội dung cho người xem khác.
- Tạo thêm hình thức tương tác nhẹ bên cạnh bình luận.
- Cung cấp dữ liệu phục vụ phân tích và đề xuất nội dung.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Nội dung được phép đánh giá.
- Người dùng đã đăng nhập khi gửi hoặc thay đổi đánh giá.

### Acceptance Criteria

1. Đầu khu vực Bình luận hiển thị thang điểm 5 sao, điểm trung bình và tổng số lượt đánh giá.
2. Người chưa đăng nhập xem được điểm trung bình và tổng lượt đánh giá.
3. Khi người chưa đăng nhập chọn đánh giá, hệ thống yêu cầu đăng nhập.
4. Mỗi tài khoản chỉ có một đánh giá hiện hành trên mỗi series và một đánh giá hiện hành trên mỗi tập.
5. Đánh giá ở cấp series và đánh giá ở cấp tập là hai bản ghi độc lập.
6. Người dùng có thể thay đổi đánh giá đã gửi mà không tạo thêm lượt đánh giá mới.
7. Khi có đánh giá mới hoặc thay đổi đánh giá, điểm trung bình và tổng lượt được tính lại tự động.
8. Sau khi gửi thành công, giao diện hiển thị rõ số sao hiện tại của người dùng.
9. Nếu gửi thất bại, hệ thống giữ trạng thái cũ và thông báo để người dùng thử lại.

### Quy tắc nghiệp vụ

- Giá trị hợp lệ từ 1 đến 5 sao.
- Hệ thống phải chống gửi trùng do người dùng bấm nhiều lần hoặc lỗi mạng.
- Quy tắc làm tròn điểm trung bình cần thống nhất trên các nền tảng.

### Phụ thuộc

- Hệ thống tài khoản MyTV.
- Dịch vụ nội dung định danh series/tập.

### Điểm cần PO chốt

- Số chữ số thập phân của điểm trung bình.
- Có loại trừ đánh giá của tài khoản bị khóa/vi phạm hay không.

---

