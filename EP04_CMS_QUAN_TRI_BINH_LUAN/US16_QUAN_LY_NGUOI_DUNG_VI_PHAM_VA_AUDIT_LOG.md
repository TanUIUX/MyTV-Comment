# US16 — Quản lý người dùng vi phạm và audit log

> Thuộc EP04 — CMS quản trị bình luận
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là Admin có thẩm quyền**, tôi muốn xem lịch sử vi phạm, áp dụng chế tài và tra cứu audit log, để xử lý tài khoản nhất quán và có thể kiểm chứng.

### Giá trị

- Hạn chế tái phạm và bảo vệ cộng đồng.
- Tạo bằng chứng minh bạch cho kiểm tra nội bộ/khiếu nại.
- Kiểm soát việc sử dụng quyền Admin.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Admin đã được cấp quyền xử lý tài khoản hoặc xem audit.
- Dữ liệu vi phạm và lịch sử kiểm duyệt được liên kết với tài khoản.

### Acceptance Criteria — Lịch sử và chế tài

1. Admin xem được lịch sử bình luận bị xử lý, Report liên quan, cảnh báo và chế tài trước đó của tài khoản.
2. Admin có thể gửi cảnh báo theo mẫu/lý do được cấu hình.
3. Admin có thể hạn chế quyền bình luận trong một khoảng thời gian xác định.
4. Admin có thể khóa tạm thời hoặc khóa vĩnh viễn theo quyền được cấp và chính sách phê duyệt.
5. CMS yêu cầu lý do, thời hạn và bước xác nhận đối với chế tài ảnh hưởng tài khoản.
6. Khi quyền bình luận bị hạn chế, người dùng không thể đăng, reply, Like, Mention, Report hoặc thực hiện các tương tác bị cấu hình chặn qua giao diện/API.
7. Khi hết hạn hạn chế tạm thời, quyền được khôi phục tự động nếu không có chế tài khác còn hiệu lực.
8. Việc thu hồi huy hiệu khi vi phạm nghiêm trọng có thể được thực hiện qua US17.
9. Các hành động không được làm mất dữ liệu lịch sử cần thiết cho audit.

### Acceptance Criteria — Audit log

1. Hệ thống lưu tối thiểu: Admin thực hiện, thời gian, đối tượng, hành động, lý do, trạng thái trước và trạng thái sau.
2. Audit bao phủ duyệt/từ chối/ẩn/xóa, Report, Flag, Spoiler, ghim, cấu hình phim, cảnh báo và chế tài tài khoản.
3. Admin được phân quyền có thể tìm kiếm/lọc audit theo thời gian, người thao tác, loại hành động và đối tượng.
4. Audit log không thể bị sửa/xóa bởi vai trò vận hành thông thường.
5. Dữ liệu bình luận xóa mềm được giữ 90 ngày theo quyết định hiện tại; audit log tuân theo thời hạn lưu trữ riêng do chính sách dữ liệu quy định.
6. Việc truy cập dữ liệu nhạy cảm trong audit cũng được kiểm soát và ghi nhận khi cần.

### Quy tắc nghiệp vụ

- Chế tài phải tương xứng mức độ vi phạm và theo ma trận chính sách.
- Admin chỉ sử dụng dữ liệu cho mục đích vận hành được phê duyệt.
- Cần phân biệt khóa quyền cộng đồng với khóa toàn bộ tài khoản/thuê bao MyTV.
- Không thu hồi huy hiệu hoặc khóa tài khoản tự động chỉ dựa trên một Report chưa được xác minh.

### Phụ thuộc

- Hệ thống tài khoản và phân quyền MyTV.
- US10 — Report.
- US14 — Xử lý nội dung.
- US17 — Huy hiệu.

### Điểm cần PO chốt

- Ma trận vi phạm → chế tài.
- Phạm vi “khóa tài khoản”: quyền bình luận hay toàn bộ tài khoản MyTV.
- Thời hạn lưu audit theo yêu cầu pháp lý/nội bộ.
- Quy trình khiếu nại và hoàn tác chế tài.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra lịch sử vi phạm, chế tài, thời hạn khôi phục quyền và audit log đầy đủ/bất biến theo role, đồng thời không khóa nhầm toàn bộ thuê bao khi chỉ hạn chế quyền cộng đồng.

### Rủi ro chính

- Chế tài áp dụng sai phạm vi/thời hạn hoặc không chặn được API.
- Audit thiếu trạng thái trước/sau, lý do hoặc cho phép chỉnh sửa/xóa.
- Lộ dữ liệu nhạy cảm hoặc tự động khóa chỉ dựa trên Report chưa xác minh.

### Dữ liệu kiểm thử

U1 có lịch sử Report/Flag/chế tài; chế tài cảnh báo, hạn chế 1 giờ, khóa tạm thời/vĩnh viễn; Admin đủ quyền, Admin chỉ đọc và user thường.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US16-001 | History | U1 có nhiều comment, Report, chế tài | Mở hồ sơ vi phạm bằng Admin có quyền | Hiển thị đúng lịch sử, đối tượng, thời gian, kết quả; không trộn tài khoản khác. |
| TC-US16-002 | Warning | Có mẫu cảnh báo hợp lệ | Admin gửi cảnh báo cho U1 | Cảnh báo được lưu/gửi theo thiết kế, có actor/thời gian/lý do và xuất hiện trong history. |
| TC-US16-003 | Restriction | U1 chưa bị hạn chế | Áp dụng hạn chế quyền bình luận trong 1 giờ | U1 bị chặn các hành động được cấu hình: comment, reply, Like, Mention, Report; UI và API đều chặn. |
| TC-US16-004 | Expiry | Hạn chế tạm thời sắp hết/hết hạn | Kiểm tra trước và sau mốc hết hạn | Trước mốc bị chặn; sau mốc quyền khôi phục tự động nếu không còn chế tài khác. |
| TC-US16-005 | Temporary lock | Admin có quyền khóa tạm | Nhập thời hạn, lý do, xác nhận khóa | CMS yêu cầu đủ lý do/thời hạn; khóa đúng phạm vi và hiển thị trạng thái. |
| TC-US16-006 | Permanent lock | Role được phép khóa vĩnh viễn | Thực hiện khóa vĩnh viễn | Có bước xác nhận/ma trận phê duyệt; quyền bị khóa theo policy, lịch sử không mất. |
| TC-US16-007 | Scope | Chính sách chỉ khóa quyền cộng đồng | U1 thử đăng nhập/xem nội dung thuê bao và tương tác | Chỉ các quyền cộng đồng bị chặn; không khóa toàn bộ tài khoản nếu policy không yêu cầu. |
| TC-US16-008 | Authorization | Admin chỉ đọc/user thường | Thử áp dụng chế tài và xem PII | Chỉ role được cấp mới thao tác; dữ liệu nhạy cảm bị masking/deny. |
| TC-US16-009 | Badge integration | U1 có huy hiệu và vi phạm nghiêm trọng đã xác minh | Thu hồi qua luồng được phép | Huy hiệu bị thu hồi theo policy; comment/history không bị xóa ngoài hành động đã chọn. |
| TC-US16-010 | Audit completeness | Thực hiện duyệt/từ chối/ẩn/xóa, Report, Flag, Spoiler, ghim, config, warning, sanction | Tra cứu audit theo đối tượng | Tất cả loại hành động có actor, time, target, action, reason, before/after. |
| TC-US16-011 | Audit immutability | Có audit đã ghi | Dùng role vận hành thường thử sửa/xóa; role có quyền đọc lọc theo thời gian/action | Không sửa/xóa được; tìm kiếm/lọc trả đúng record và quyền. |
| TC-US16-012 | Retention | Comment xóa mềm và audit liên quan | Kiểm tra trong mốc 90 ngày và sau thời hạn policy | Comment xóa mềm phục vụ audit đúng thời hạn; xử lý sau hạn theo policy, không tự lộ ra công khai. |
| TC-US16-013 | Privacy audit | Admin truy cập dữ liệu nhạy cảm | Xem hồ sơ/audit rồi kiểm tra log truy cập | Quyền truy cập được kiểm soát và ghi nhận khi policy yêu cầu; không lộ PII cho role không phù hợp. |
| TC-US16-014 | Safety rule | U1 chỉ có một Report chưa xác minh | Tạo Report rồi kiểm tra chế tài tự động | Không tự khóa/thu hồi huy hiệu chỉ từ Report chưa xác minh. |

### Điểm cần PO chốt

- Ma trận vi phạm → chế tài, phạm vi khóa và thời hạn lưu audit.
- Quy trình khiếu nại, hoàn tác và quyền phê duyệt khóa vĩnh viễn.
