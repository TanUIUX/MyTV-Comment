# US14 — Xử lý nội dung trên CMS

> Thuộc EP04 — CMS quản trị bình luận
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là Admin kiểm duyệt**, tôi muốn duyệt, từ chối, ẩn, xóa, xử lý Report/Flag và điều chỉnh Spoiler, để thực thi chính sách cộng đồng một cách nhất quán.

### Giá trị

- Cho phép xử lý toàn bộ vòng đời kiểm duyệt tại một nơi.
- Giảm rủi ro nội dung vi phạm tiếp tục hiển thị.
- Giữ lại bằng chứng và lý do cho việc kiểm tra sau này.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Admin có quyền kiểm duyệt tương ứng.
- Bình luận/reply tồn tại và chưa bị xóa vĩnh viễn.

### Acceptance Criteria — Hành động kiểm duyệt

1. Admin có thể Duyệt nội dung đang Chờ duyệt để chuyển sang Hiển thị.
2. Admin có thể Từ chối nội dung đang Chờ duyệt để nội dung không được công khai.
3. Admin có thể Ẩn nội dung đang Hiển thị mà không xóa dữ liệu audit.
4. Admin có thể Xóa mềm bình luận/reply vi phạm sau bước xác nhận.
5. Khi Admin xóa bình luận gốc, toàn bộ thread không còn hiển thị công khai.
6. Mỗi thao tác yêu cầu lý do theo danh mục được cấu hình hoặc ghi chú bổ sung khi cần.
7. CMS kiểm tra lại trạng thái hiện tại trước khi lưu để tránh hai Admin ghi đè quyết định của nhau mà không cảnh báo.

### Acceptance Criteria — Report

1. Admin xem được danh sách Report, người gửi, lý do, thời gian và phiên bản nội dung bị báo cáo.
2. Admin có thể chọn Bỏ qua Report nếu bình luận không vi phạm.
3. Admin có thể Duyệt giữ nguyên, Ẩn hoặc Xóa nội dung tùy kết luận.
4. Hệ thống lưu lịch sử từng Report và kết quả xử lý.
5. Nhiều Report không tự động thay đổi trạng thái hiển thị trước quyết định CMS.

### Acceptance Criteria — Flag

1. Admin có thể gắn cờ nội bộ với các lý do: Spoiler, Spam/quảng cáo, Ngôn từ xúc phạm, Nội dung không phù hợp và Vi phạm khác.
2. Admin có thể cập nhật trạng thái theo dõi và bỏ cờ khi hoàn tất.
3. Flag chỉ phục vụ vận hành nội bộ và không hiển thị cho người dùng.
4. Hệ thống lưu người gắn cờ, thời gian, lý do và người hoàn tất xử lý.

### Acceptance Criteria — Spoiler

1. Admin có thể thêm trạng thái Spoiler cho bình luận chưa được tác giả đánh dấu.
2. Admin có thể bỏ trạng thái Spoiler nếu xác định không cần thiết.
3. Thay đổi được phản ánh trên ứng dụng người xem và lưu audit.
4. Nội dung bị Admin đánh dấu Spoiler được che theo cùng trải nghiệm với Spoiler do người dùng chọn.

### Quy tắc nghiệp vụ

- Admin xử lý theo chính sách cộng đồng, không dựa trên quan điểm cá nhân.
- Ý kiến trái chiều nhưng hợp lệ không bị xóa chỉ vì tiêu cực.
- Hành động phải được phân quyền và lưu audit.
- Xóa tại CMS là xóa mềm trong thời hạn lưu trữ 90 ngày, trừ quy trình xóa vĩnh viễn được phê duyệt riêng.

### Phụ thuộc

- US10 — Report bình luận.
- US11 và US12 — AI và trạng thái.
- US16 — Audit log và xử lý tài khoản.

### Điểm cần PO chốt

- Danh mục lý do chuẩn cho từng hành động.
- Có hỗ trợ thao tác hàng loạt hay chỉ từng bình luận trong MVP.
- Có thông báo cho tác giả khi nội dung bị từ chối/ẩn/xóa hay không.
- Quy trình hoàn tác một quyết định sai.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra các hành động kiểm duyệt, Report, Flag và Spoiler được phân quyền, yêu cầu lý do, cập nhật trạng thái đúng, không mất audit và xử lý xung đột đồng thời.

### Rủi ro chính

- Admin thao tác sai state hoặc hai Admin ghi đè nhau.
- Xóa comment gốc không cascade thread; Report/Flag lộ ra người dùng.
- Bỏ cờ/Spoiler không cập nhật app hoặc audit thiếu dữ liệu.

### Dữ liệu kiểm thử

Comment/reply Chờ duyệt, Hiển thị, Report, Flag, Spoiler; C1 có R1/R2; Admin kiểm duyệt đủ quyền, role chỉ đọc, hai Admin thao tác đồng thời.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US14-001 | Moderation | Comment Chờ duyệt | Admin chọn Duyệt, nhập lý do và xác nhận | State chuyển Hiển thị; app cập nhật; audit lưu actor/thời gian/lý do trước-sau. |
| TC-US14-002 | Moderation | Comment Chờ duyệt | Admin chọn Từ chối với lý do hợp lệ | State không công khai; tác giả nhận trạng thái theo UX; audit đầy đủ. |
| TC-US14-003 | Moderation | Comment Hiển thị | Admin chọn Ẩn rồi Xóa mềm | Ẩn không xóa dữ liệu; Xóa yêu cầu xác nhận, loại khỏi công khai và giữ audit. |
| TC-US14-004 | Cascade | C1 có R1/R2 | Admin xóa C1 | C1 và toàn bộ thread biến mất khỏi UI/API công khai; không xóa nhầm thread khác. |
| TC-US14-005 | Reason validation | Thao tác yêu cầu lý do | Bỏ trống lý do hoặc nhập ngoài giới hạn | Không cho lưu; hiển thị validation và không đổi state. |
| TC-US14-006 | Concurrency | Hai Admin cùng mở C1 | Admin A xử lý trước, Admin B lưu state cũ | Admin B nhận cảnh báo xung đột; không ghi đè im lặng quyết định mới. |
| TC-US14-007 | Report queue | C1 có nhiều Report | Mở Report, chọn Bỏ qua/Duyệt/Ẩn/Xóa | Hiển thị người gửi/lý do/version; hành động đúng kết luận và lưu lịch sử từng Report. |
| TC-US14-008 | Report rule | C1 nhận nhiều Report | Tạo thêm Report rồi kiểm tra ngay | State không tự đổi chỉ vì số lượng; Admin là người quyết định. |
| TC-US14-009 | Flag | C1 đang Hiển thị | Gắn từng lý do Flag, cập nhật đang xử lý/hoàn tất, bỏ cờ | Flag chỉ hiển thị nội bộ; trạng thái, người thao tác, thời gian, lý do được lưu. |
| TC-US14-010 | Spoiler | C1 chưa/đã có Spoiler | Admin thêm rồi bỏ Spoiler | App che/bỏ che theo state mới; thay đổi không làm mất text và có audit. |
| TC-US14-011 | Authorization | Role chỉ xem hoặc không có quyền xóa | Gọi UI/API từng hành động | Hành động không được cấp bị ẩn/chặn; không bypass qua API. |
| TC-US14-012 | Data retention | Comment bị Xóa mềm | Kiểm tra CMS/audit sau thao tác | Dữ liệu không hiển thị công khai nhưng còn record phục vụ audit 90 ngày theo policy. |

### Điểm cần PO chốt

- Danh mục lý do, thông báo tác giả và có thao tác hàng loạt trong MVP hay không.
- Cơ chế hoàn tác quyết định sai và quyền được phép hoàn tác.
