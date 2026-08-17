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

## Phân tích kiểm thử

### Mục tiêu

Xác nhận Like/Unlike hoạt động đúng trên comment gốc và reply, chỉ có một trạng thái trên mỗi tài khoản, cập nhật gần thời gian thực và không bị nhân đôi khi retry.

### Rủi ro chính

- Đếm Like sai do double-click, request retry hoặc cập nhật đồng thời.
- Cho Like nội dung đã Ẩn/Xóa hoặc nội dung người dùng không còn quyền xem.
- UI hiển thị trạng thái khác với dữ liệu server.

### Dữ liệu kiểm thử

U1/U2 đã đăng nhập, phiên khách, comment gốc C1, reply R1, comment công khai/Ẩn/Xóa, dữ liệu Like bằng 0/1/nhiều và mạng chậm.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US07-001 | Functional | U1 đăng nhập; C1 và R1 công khai | Like C1 rồi Like R1 | Mỗi đối tượng nhận đúng một Like; số Like và trạng thái nút cập nhật. |
| TC-US07-002 | State transition | U1 đã Like C1 | Chọn Unlike | Like của U1 bị hủy, số Like giảm đúng một và nút trở về trạng thái chưa Like. |
| TC-US07-003 | Idempotency | U1 chưa Like; mạng chậm | Double-click Like hoặc gửi lại cùng request | Chỉ tạo một trạng thái Like; không tăng số đếm nhiều lần. |
| TC-US07-004 | Multi-user | U1/U2 cùng mở C1 | U1 Like/Unlike trong khi U2 refresh | Số Like cuối cùng phản ánh server; UI không ghi đè sai trạng thái của tài khoản khác. |
| TC-US07-005 | Authentication | Phiên khách | Chọn Like trên C1 | Yêu cầu đăng nhập; không tạo Like trước xác thực. |
| TC-US07-006 | Authorization | C1 bị Ẩn/Xóa hoặc U1 mất quyền xem | Thử Like bằng UI và API | Thao tác bị từ chối; số Like công khai không đổi. |
| TC-US07-007 | Error handling | Mock API timeout/500 sau khi bấm Like | Theo dõi UI và tải lại trang | UI hoàn về trạng thái đúng server, hiển thị lỗi và cho phép thử lại. |
| TC-US07-008 | Data integrity | C1 có số Like ban đầu | Like, Unlike, refresh và đối chiếu dữ liệu | Không có bản ghi Like trùng; dữ liệu giữ đúng một trạng thái/tài khoản/comment. |
| TC-US07-009 | Integration | C1 có số Like mới | Mở chế độ Được yêu thích ở US02 sau khi Like | Thứ tự/điểm Like phản ánh thay đổi trong độ trễ đã thống nhất. |

### Điểm cần PO chốt

- Có cho phép tác giả Like comment của chính mình hay không.
- SLA cập nhật số Like và thời điểm dữ liệu được dùng cho sắp xếp.
