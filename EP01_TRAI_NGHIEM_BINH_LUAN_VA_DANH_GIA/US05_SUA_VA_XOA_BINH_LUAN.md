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

## Phân tích kiểm thử

### Mục tiêu

Xác nhận quyền sở hữu, vòng đời phiên bản khi sửa, tính toàn vẹn Like/thread, cơ chế xóa thread và lưu xóa mềm 90 ngày.

### Rủi ro chính

- Người khác sửa/xóa được comment không thuộc sở hữu.
- Bản sửa Chờ duyệt ghi đè nhầm bản công khai hoặc làm mất Like/thread.
- Xóa comment gốc nhưng vẫn để lộ reply qua API/cache; bản sửa chờ vẫn được xuất bản sau khi đã xóa.

### Dữ liệu kiểm thử

U1 có comment gốc C1, reply R1/R2, Like và phiên bản công khai; U2 không sở hữu; test cả Chế độ 1 và Chế độ 2, approve/reject, xóa gốc/reply và lỗi đồng thời.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US05-001 | Authorization | U1 sở hữu C1/R1; U2 không sở hữu | U1 sửa/xóa; U2 thử sửa/xóa cùng ID qua UI và API | U1 được phép theo quyền; U2 bị từ chối, không đổi dữ liệu. |
| TC-US05-002 | Functional | C1 đang Hiển thị | U1 sửa nội dung bất kỳ lúc nào | Tạo phiên bản mới, giữ liên kết với cùng comment và đưa qua kiểm duyệt. |
| TC-US05-003 | Versioning | Bản sửa ở trạng thái Chờ duyệt | U2 mở thread trong lúc chờ; U1 mở màn hình của mình | U2 thấy bản cũ; U1 thấy bản mới kèm trạng thái Chờ duyệt. |
| TC-US05-004 | Versioning | Bản sửa Chờ duyệt được Admin duyệt | Admin duyệt phiên bản mới | Bản mới thành công khai; định danh comment, Like và thread được giữ. |
| TC-US05-005 | Negative | Bản sửa Chờ duyệt bị Admin từ chối | Admin từ chối rồi mở lại comment | Bản công khai gần nhất vẫn hiển thị; bản sửa không lộ cho cộng đồng. |
| TC-US05-006 | Delete/root | C1 có R1/R2 và dữ liệu Like | U1 xác nhận xóa C1; kiểm tra UI, API công khai và tổng số | C1 và toàn bộ thread biến mất khỏi công khai; số lượng cập nhật; dữ liệu giữ theo audit. |
| TC-US05-007 | Delete/reply | R1 thuộc U1, C1 còn Hiển thị | U1 xác nhận xóa R1 | Chỉ R1 bị xóa; C1/R2 và ngữ cảnh còn lại không bị ảnh hưởng. |
| TC-US05-008 | Audit/retention | Đã xóa C1/R1 | Tra cứu CMS/audit và kiểm tra mốc lưu 90 ngày | Có bản ghi xóa mềm, người thao tác/thời gian/lý do; không hiển thị công khai trong thời hạn. |
| TC-US05-009 | Race condition | C1 có bản sửa đang Chờ duyệt | U1 xóa C1 trước khi bản sửa được duyệt | Phiên bản Chờ duyệt bị vô hiệu hóa; không được xuất bản sau đó. |
| TC-US05-010 | Confirmation | U1 mở thao tác xóa | Hủy ở hộp thoại xác nhận | Không có thay đổi dữ liệu, thread vẫn hiển thị như trước. |
| TC-US05-011 | Concurrency | Hai request sửa/xóa cùng lúc hoặc phiên đăng nhập cũ | Gửi đồng thời các request | Hệ thống kiểm tra trạng thái/phiên bản, trả lỗi xung đột phù hợp và không tạo trạng thái không nhất quán. |

### Điểm cần PO chốt trước khi nghiệm thu giao diện

- Có hiển thị nhãn “Đã chỉnh sửa” và lịch sử phiên bản cho tác giả hay không.
- Quy trình phục hồi nội bộ sau xóa mềm nếu có yêu cầu audit.
