# US13 — Tra cứu bình luận trên CMS

> Thuộc EP04 — CMS quản trị bình luận
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là Admin**, tôi muốn xem, tìm kiếm và lọc toàn bộ bình luận theo nhiều tiêu chí, để nhanh chóng xác định nội dung cần kiểm duyệt hoặc điều tra.

### Giá trị

- Giảm thời gian tìm kiếm thủ công.
- Hỗ trợ xử lý hàng chờ và sự cố theo đúng ngữ cảnh.
- Tạo một nguồn dữ liệu quản trị thống nhất.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Admin đã đăng nhập CMS và có quyền phù hợp.
- Dữ liệu bình luận và metadata được đồng bộ với CMS.

### Acceptance Criteria

1. Admin xem được danh sách bình luận và reply trên toàn bộ phạm vi được phân quyền.
2. Mỗi bản ghi hiển thị tối thiểu: nội dung, tác giả, phim/series, tập, thời gian đăng, trạng thái, số Like, số Reply và số Report.
3. Admin tìm kiếm được theo từ khóa nội dung.
4. Admin tìm kiếm được theo tài khoản/nickname hoặc định danh hợp lệ được phép xem.
5. Admin lọc được theo phim, tập và khoảng thời gian.
6. Admin lọc được theo trạng thái: Hiển thị, Chờ duyệt, Bị Report, Có Spoiler, Spam/nhãn tương ứng, Ẩn, Từ chối và Đã xử lý theo mô hình dữ liệu thực tế.
7. Admin có thể kết hợp nhiều bộ lọc và xóa điều kiện lọc để quay về danh sách mặc định.
8. Kết quả có phân trang hoặc tải thêm và không lặp/bỏ sót bản ghi.
9. Admin mở được màn hình chi tiết để xem thread, phiên bản nội dung, kết quả AI, Report/Flag và lịch sử xử lý liên quan.
10. Khi một bình luận gốc có reply, Admin xem được toàn bộ ngữ cảnh thread trước khi xử lý.
11. Dữ liệu nhạy cảm chỉ hiển thị cho vai trò được cấp quyền; CMS không mặc định công khai đầy đủ thông tin thuê bao.
12. Trạng thái mới được phản ánh trong danh sách theo độ trễ vận hành được thống nhất.

### Quy tắc nghiệp vụ

- Quyền xem dữ liệu phải giới hạn theo vai trò/phạm vi của Admin.
- Tìm kiếm và lọc không làm thay đổi dữ liệu.
- Danh sách mặc định nên ưu tiên nội dung cần xử lý theo SLA, nhưng quy tắc cụ thể cần PO/vận hành chốt.

### Phụ thuộc

- Mô hình dữ liệu bình luận, trạng thái và phiên bản.
- Hệ thống phân quyền CMS.
- US11, US12 về nhãn AI và trạng thái.

### Điểm cần PO chốt

- Danh sách mặc định ưu tiên Chờ duyệt, Report hay thời gian mới nhất.
- Nhu cầu export dữ liệu và giới hạn dữ liệu được export.
- Phạm vi dữ liệu cá nhân từng vai trò Admin được xem.

---

