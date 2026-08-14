# US15 — Quản lý bình luận nổi bật và cấu hình theo phim

> Thuộc EP04 — CMS quản trị bình luận
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là Admin vận hành nội dung**, tôi muốn ghim bình luận nổi bật và cấu hình trạng thái/cơ chế kiểm duyệt theo từng phim, để định hướng thảo luận và áp dụng mức kiểm soát phù hợp.

### Giá trị

- Làm nổi bật nội dung thảo luận có chất lượng.
- Cho phép phản ứng nhanh với nội dung hoặc giai đoạn nhạy cảm.
- Giảm phụ thuộc vào cấu hình kỹ thuật thủ công.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Admin có quyền vận hành nội dung.
- Phim đã tồn tại trong CMS và liên kết đúng với dịch vụ bình luận.

### Acceptance Criteria — Bình luận nổi bật

1. Admin có thể ghim một bình luận đang Hiển thị lên khu vực Nổi bật.
2. Admin có thể bỏ ghim hoặc thay thế bình luận đã ghim.
3. Hệ thống giới hạn số lượng bình luận ghim theo cấu hình; giá trị đề xuất mặc định là ba.
4. Khi đạt giới hạn, CMS yêu cầu Admin bỏ ghim/thay thế trước khi thêm mới hoặc thực hiện theo UX đã duyệt.
5. Bình luận bị ẩn hoặc xóa tự động không còn được ghim công khai.
6. Thứ tự các bình luận ghim được áp dụng nhất quán trên web/mobile.
7. Mọi thao tác ghim/bỏ ghim được lưu audit.

### Acceptance Criteria — Cấu hình theo phim

1. Admin có thể chọn Mở hoặc Đóng khu vực Bình luận cho từng phim.
2. Khi Mở, Admin chọn Chế độ 1 hoặc Chế độ 2 theo định nghĩa tại US11.
3. Khi Đóng, toàn bộ khu vực Bình luận bị ẩn theo US12 và hệ thống ngừng nhận tương tác mới.
4. Admin thấy rõ trạng thái hiện hành và thời điểm hiệu lực của cấu hình.
5. Admin có thể cấu hình mốc thời gian chuyển sang trạng thái “Chờ duyệt — chỉ tác giả thấy” nếu nghiệp vụ sử dụng quy tắc sau X giờ.
6. Hệ thống cảnh báo tác động trước khi Admin thay đổi cấu hình đang áp dụng cho phim có nhiều tương tác.
7. Thay đổi cấu hình được truyền tới ứng dụng và API trong độ trễ được thống nhất.
8. Toàn bộ lịch sử cấu hình được lưu với người thao tác và thời gian.

### Quy tắc nghiệp vụ

- Nổi bật là chế độ sắp xếp mặc định trên ứng dụng.
- AI có thể đề xuất ứng viên nổi bật tại US20, nhưng Admin là người quyết định ghim trong phạm vi hiện tại.
- Đóng bình luận không xóa bình luận cũ.
- Cấu hình áp dụng theo phim; khả năng ghi đè ở cấp tập cần được coi là phạm vi bổ sung nếu PO yêu cầu.

### Phụ thuộc

- US02 — Hiển thị và sắp xếp.
- US11, US12 — Chế độ kiểm duyệt và trạng thái.
- US20 — AI hỗ trợ vận hành cộng đồng.

### Điểm cần PO chốt

- Có cho phép sắp xếp thủ công thứ tự ba bình luận ghim hay theo thời gian ghim.
- Có cần ngày hết hạn ghim.
- Mốc “X giờ” được tính từ sự kiện nào.
- Có cần cấu hình riêng theo tập phim trong tương lai.

---

