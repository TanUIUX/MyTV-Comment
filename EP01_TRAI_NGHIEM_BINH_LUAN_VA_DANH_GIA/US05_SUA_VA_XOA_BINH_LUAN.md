# US05 — Sửa và xóa bình luận

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là tác giả bình luận**, tôi muốn sửa hoặc xóa nội dung của chính mình, để cập nhật ý kiến hoặc loại bỏ nội dung không còn muốn hiển thị.

### Giá trị

- Trao quyền kiểm soát nội dung cho người dùng.
- Giảm nhu cầu yêu cầu hỗ trợ thủ công.
- Duy trì lịch sử phục vụ kiểm duyệt và audit.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập đúng tài khoản sở hữu bình luận/reply.
- Bình luận chưa bị hệ thống xóa vĩnh viễn.

### Acceptance Criteria

1. Tác giả có thể sửa bình luận hoặc reply của mình bất kỳ lúc nào khi còn quyền bình luận.
2. Người dùng khác không thể sửa hoặc xóa nội dung không thuộc sở hữu của họ.
3. Nội dung sửa được tạo thành phiên bản mới và đi qua kiểm duyệt.
4. Trong thời gian phiên bản mới chờ duyệt, người khác tiếp tục thấy phiên bản cũ đã được duyệt.
5. Tác giả thấy rõ phiên bản mới đang chờ duyệt.
6. Khi phiên bản mới được duyệt, hệ thống thay thế phiên bản công khai nhưng giữ nguyên định danh bình luận, Like và thread hiện có.
7. Khi phiên bản mới bị từ chối, phiên bản công khai gần nhất tiếp tục được hiển thị.
8. Tác giả có thể xóa bình luận gốc hoặc reply của mình sau bước xác nhận.
9. Khi xóa bình luận gốc, toàn bộ reply thuộc thread bị xóa khỏi giao diện công khai.
10. Khi xóa một reply, chỉ reply đó bị xóa; các nội dung khác trong thread không bị ảnh hưởng.
11. Sau khi xóa, số lượng bình luận công khai được cập nhật.
12. Nội dung được xóa mềm và giữ 90 ngày phục vụ audit trước khi xử lý theo chính sách lưu trữ.
13. Nếu tác giả xóa bình luận khi có phiên bản sửa đang chờ, phiên bản chờ không được phép xuất bản sau đó.

### Quy tắc nghiệp vụ

- Không giới hạn thời gian sửa bình luận.
- Xóa bình luận gốc đồng nghĩa xóa toàn bộ thread khỏi trải nghiệm người dùng.
- Admin vẫn có quyền xử lý bình luận theo chính sách CMS.
- Việc lưu 90 ngày không đồng nghĩa Admin được phép phục hồi công khai nếu chưa có quy trình được phê duyệt.

### Phụ thuộc

- US11 — AI kiểm duyệt theo hai chế độ.
- US16 — Audit log và chính sách lưu trữ.

### Điểm cần PO chốt

- Có hiển thị nhãn “Đã chỉnh sửa” sau khi phiên bản mới được duyệt hay không.
- Người dùng có được xem lịch sử phiên bản của chính mình hay không.

---

