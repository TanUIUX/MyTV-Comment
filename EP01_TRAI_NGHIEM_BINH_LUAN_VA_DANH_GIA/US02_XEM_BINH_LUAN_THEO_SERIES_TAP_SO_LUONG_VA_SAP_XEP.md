# US02 — Xem bình luận theo series/tập, số lượng và sắp xếp

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người xem**, tôi muốn xem đúng bình luận của series hoặc tập phim và sắp xếp theo nhu cầu, để theo dõi thảo luận phù hợp với nội dung mình đang xem.

### Giá trị

- Tránh lẫn thảo luận giữa các tập.
- Giúp người dùng nhanh chóng tìm được bình luận đáng chú ý hoặc mới nhất.
- Tăng khả năng khám phá các cuộc thảo luận có tương tác cao.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Series/tập phim đã được định danh chính xác trên hệ thống nội dung.
- Khu vực bình luận đang được bật.

### Acceptance Criteria

1. Phim nhiều tập có hai phạm vi độc lập: bình luận chung của series và bình luận của từng tập.
2. Khi người dùng chuyển sang tập khác, hệ thống tải đúng danh sách bình luận của tập mới.
3. Bình luận của một tập không xuất hiện trong phạm vi của tập khác hoặc phạm vi series.
4. Tiêu đề Bình luận hiển thị tổng số bình luận chính và reply thuộc phạm vi hiện tại.
5. Số lượng công khai được cập nhật khi có bình luận/reply mới được hiển thị hoặc khi nội dung bị xóa/ẩn.
6. Hệ thống cung cấp ba chế độ: Nổi bật, Mới nhất và Được yêu thích.
7. Nổi bật là chế độ mặc định.
8. Ở chế độ Nổi bật, các bình luận được Admin ghim hiển thị trước; giới hạn mặc định tối đa ba bình luận.
9. Ở chế độ Mới nhất, bình luận chính được sắp xếp từ mới đến cũ.
10. Ở chế độ Được yêu thích, bình luận chính được sắp theo số Like giảm dần; nếu bằng nhau thì bình luận mới hơn đứng trước.
11. Lựa chọn sắp xếp chỉ áp dụng cho phạm vi series/tập hiện tại.
12. Khi danh sách dài, hệ thống hỗ trợ tải thêm hoặc phân trang mà không lặp hoặc bỏ sót bình luận.

### Quy tắc nghiệp vụ

- Reply được tính vào tổng số bình luận nhưng không được trộn thành bình luận gốc trong danh sách.
- Bình luận đang chờ duyệt, bị ẩn hoặc xóa không được tính vào tổng số công khai.
- Công thức xếp hạng phần còn lại của chế độ Nổi bật cần được PO chốt trước refinement.

### Phụ thuộc

- US07 — Like và Unlike bình luận.
- US15 — Quản lý bình luận nổi bật và cấu hình theo phim.

### Điểm cần PO chốt

- Số bình luận tải lần đầu và mỗi lần “Xem thêm”.
- Số reply hiển thị ban đầu.
- Công thức xếp hạng Nổi bật bên dưới các bình luận được ghim.

---

