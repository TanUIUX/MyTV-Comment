# US12 — Quản lý trạng thái và phạm vi hiển thị bình luận

> Thuộc EP03 — An toàn và kiểm duyệt
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là Admin vận hành**, tôi muốn hệ thống áp dụng trạng thái và phạm vi hiển thị phù hợp theo cấu hình của từng phim, để kiểm soát chính xác ai được xem bình luận ở từng giai đoạn kiểm duyệt.

### Giá trị

- Ngăn nội dung chưa được duyệt xuất hiện sai đối tượng.
- Cho phép áp dụng cơ chế linh hoạt theo rủi ro từng phim.
- Đảm bảo trải nghiệm nhất quán giữa ứng dụng và CMS.

### Ưu tiên

**Must**

### Các trạng thái nghiệp vụ tối thiểu

| Trạng thái | Hiển thị với tác giả | Hiển thị với cộng đồng | Ý nghĩa |
|---|---:|---:|---|
| Chờ duyệt | Có | Không | Đang chờ AI/Admin quyết định |
| Hiển thị | Có | Có | Được phép công khai |
| Từ chối | Có theo chính sách UX | Không | Không được duyệt |
| Ẩn | Có/Không theo chính sách UX | Không | Admin tạm ẩn khỏi cộng đồng |
| Xóa mềm | Không | Không | Đã xóa, giữ dữ liệu phục vụ audit |

### Acceptance Criteria

1. Bình luận mới ở trạng thái Chờ duyệt chỉ hiển thị với tác giả.
2. Người khác không nhận được bình luận Chờ duyệt qua giao diện, API công khai, thông báo hoặc số đếm công khai.
3. Khi người dùng sửa bình luận đang Hiển thị, phiên bản công khai cũ tiếp tục hiển thị trong lúc phiên bản mới Chờ duyệt.
4. Khi phiên bản mới được duyệt, hệ thống chuyển phiên bản đó thành nội dung công khai hiện hành.
5. Khi phiên bản mới bị từ chối, phiên bản công khai cũ không bị ảnh hưởng.
6. Mỗi phim có thể được cấu hình ở một trong ba mức: Chế độ 1, Chế độ 2 hoặc Đóng bình luận.
7. Khi phim chuyển sang Đóng bình luận, toàn bộ khu vực Bình luận được ẩn với người xem.
8. Khi Đóng bình luận, hệ thống không chấp nhận bình luận, reply, Like, Mention, Report hoặc đánh giá mới qua giao diện/API theo phạm vi cấu hình.
9. Việc đóng bình luận không xóa dữ liệu lịch sử.
10. Khi mở lại, dữ liệu công khai trước đó được hiển thị lại theo cấu hình và quyền hiện hành, trừ nội dung đã bị xử lý.
11. Admin có thể cấu hình thời điểm hoặc khoảng thời gian chuyển sang trạng thái “Chờ duyệt — chỉ tác giả thấy” nếu nghiệp vụ yêu cầu sau X giờ.
12. Thay đổi cấu hình chỉ áp dụng từ thời điểm hiệu lực; cách xử lý bình luận đang trong luồng phải được xác định nhất quán.
13. Tất cả chuyển đổi trạng thái và thay đổi cấu hình được lưu audit.

### Quy tắc nghiệp vụ

- Chế độ mặc định: AI tiền kiểm, nội dung an toàn hiển thị ngay, Admin hậu kiểm.
- Chế độ đặc biệt: AI kiểm tra, Admin duyệt trước khi công khai.
- Mức 3: đóng hoàn toàn khu vực bình luận.
- Không xóa dữ liệu chỉ vì chuyển chế độ hoặc đóng bình luận.

### Phụ thuộc

- US11 — AI kiểm duyệt theo hai chế độ.
- US15 — Cấu hình theo phim trên CMS.
- US16 — Audit log.

### Điểm cần PO chốt

- Khi đóng bình luận có tiếp tục hiển thị điểm đánh giá hay không; quyết định hiện tại “ẩn toàn bộ khu vực” có thể bao gồm cả rating nếu rating nằm trong khu vực này.
- Xử lý nội dung đang Chờ duyệt khi chuyển từ Chế độ 2 sang Chế độ 1.
- Ý nghĩa chính xác của cấu hình “sau X giờ”: tính từ giờ phát hành, giờ phát sóng hay một mốc Admin chọn.
- Có hiển thị lý do từ chối/ẩn cho tác giả hay không.

---

