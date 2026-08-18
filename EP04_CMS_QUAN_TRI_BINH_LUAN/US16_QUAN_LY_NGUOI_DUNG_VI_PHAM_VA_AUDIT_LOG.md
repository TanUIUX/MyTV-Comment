# US16 — Quản lý người dùng vi phạm và audit log

> Thuộc EP04 — CMS quản trị bình luận
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là Admin/Moderator có quyền moderation**, tôi muốn xem lịch sử vi phạm, áp dụng chế tài và xử lý khiếu nại có audit, để bảo vệ cộng đồng và xử lý tài khoản nhất quán.

### Ưu tiên

**Must**

### Ma trận chế tài mặc định

- **Nhẹ** → Cảnh báo.
- **Trung bình** → Khóa bình luận tạm thời.
- **Nặng** → Admin quyết định Khóa bình luận hoặc Khóa tài khoản tùy bối cảnh; Khóa tài khoản chỉ dùng khi xác định vi phạm chính sách nghiêm trọng.

### Hai mức khóa riêng

#### 1. Khóa bình luận

- Chỉ là chế tài cộng đồng, không khóa toàn bộ tài khoản MyTV.
- Chặn **Comment + Reply + Mention**.
- User vẫn được **Like, Report, Rating, Share** nếu các quyền khác hợp lệ.
- Chỉ có khóa tạm thời, **không có khóa bình luận vĩnh viễn**.
- Preset thời hạn: **10 phút, 1 giờ, 1 ngày, 3 ngày, 7 ngày, 1 tháng**, hoặc Admin tự nhập thời hạn.
- Hết hạn tự khôi phục quyền nếu không có sanction khác còn hiệu lực.

#### 2. Khóa tài khoản

- Áp dụng cho vi phạm chính sách nghiêm trọng và khóa toàn bộ tài khoản MyTV theo policy tài khoản.
- **Mọi Admin/Moderator có quyền moderation đều có thể thực hiện**, không yêu cầu Super Admin hay bước duyệt thứ hai.
- Có thể khóa tạm thời với cùng preset **10 phút, 1 giờ, 1 ngày, 3 ngày, 7 ngày, 1 tháng hoặc custom**, hoặc **khóa vĩnh viễn**.
- Khi account bị khóa, toàn bộ comment/reply đang Hiển thị của user **tự động Ẩn khỏi cộng đồng** nhưng không bị xóa mềm chỉ vì account lock.
- Khi account được mở khóa, comment/reply bị Ẩn **chỉ vì account lock** tự Hiển thị lại nếu bản thân nội dung vẫn hợp lệ và không có moderation action riêng.
- Rating của account bị loại khỏi aggregate trong thời gian khóa và tự được tính lại khi mở khóa nếu rating record còn tồn tại theo US03.

### Acceptance Criteria — Lịch sử/chế tài

1. CMS hiển thị lịch sử comment bị xử lý, Report, cảnh báo, sanction và appeal của user.
2. Cảnh báo/sanction yêu cầu reason, thời hạn khi có và confirmation trước khi áp dụng.
3. UI/API đều enforce đúng phạm vi Khóa bình luận hoặc Khóa tài khoản.
4. Tác giả nhận reason/trạng thái sanction trong app và notification nghiệp vụ bắt buộc.
5. Không tự khóa user chỉ vì một/nhiều Report chưa được xác minh.

### Acceptance Criteria — Appeal

1. User bị Khóa bình luận hoặc Khóa tài khoản có thể gửi **khiếu nại trong app**.
2. Admin xem appeal và quyết định giữ nguyên hoặc gỡ chế tài; quyết định có audit.
3. SLA xử lý appeal là **48 giờ** kể từ lúc gửi.
4. Quá 48 giờ, sanction vẫn giữ nguyên; appeal được đánh dấu **Quá SLA** và ưu tiên lên đầu queue.
5. Không tự động gỡ sanction chỉ vì appeal quá SLA.

### Acceptance Criteria — Audit log

1. Audit tối thiểu lưu actor, time, target, action, before, after; `reason` bắt buộc/nullable theo rule action tương ứng ở US14/US16.
2. Audit bao phủ moderation, Report/Flag/Spoiler, pin/config, warning/sanction, appeal, Undo và thay đổi AI policy.
3. Audit log không thể sửa/xóa bởi role vận hành thông thường.
4. Audit log được lưu **2 năm**; retention này độc lập với soft-delete comment 90 ngày.
5. Truy cập/export PII tuân rule US13 và ghi log khi cần.

### Quy tắc nghiệp vụ

- Sanction phải tương xứng mức vi phạm.
- Khóa bình luận không chặn Report để user vẫn có thể báo nội dung vi phạm.
- Khóa tài khoản là action nặng, chỉ dùng khi vi phạm nghiêm trọng dù mọi Admin/Moderator có quyền đều có thể thực hiện.
- Appeal là cơ chế review sanction; không làm mất audit cũ.

### Điểm cần PO chốt

- Không còn blocker PO cho sanction/appeal/audit trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US16-001 | History | U1 có moderation/Report/sanction | Mở hồ sơ | Hiển thị đúng timeline, không trộn user khác. |
| TC-US16-002 | Light sanction | Vi phạm Nhẹ | Gửi cảnh báo | Cảnh báo có reason/actor/time và notification. |
| TC-US16-003 | Comment lock permissions | U1 bị Khóa bình luận | Thử Comment/Reply/Mention/Like/Report/Rating/Share | 3 action đầu bị chặn; Like/Report/Rating/Share vẫn được phép nếu hợp lệ. |
| TC-US16-004 | Comment lock durations | Chọn từng preset/custom | Áp dụng | Hỗ trợ 10m/1h/1d/3d/7d/1mo/custom; không có Permanent. |
| TC-US16-005 | Comment lock expiry | Sanction sắp hết | Kiểm tra sau expiry | Quyền tự phục hồi nếu không còn sanction khác. |
| TC-US16-006 | Account lock eligibility | Vi phạm nghiêm trọng | Moderator có quyền khóa | Có thể khóa account không cần Super Admin/two-person approval. |
| TC-US16-007 | Account lock duration | Chọn temp preset/custom/permanent | Áp dụng | Hỗ trợ đúng tất cả lựa chọn và state account. |
| TC-US16-008 | Hide on account lock | U1 có comment/reply public | Khóa account | Toàn bộ comment/reply U1 Ẩn khỏi cộng đồng, không soft-delete. |
| TC-US16-009 | Restore on unlock | Có content Ẩn chỉ do account lock và content Ẩn do moderation riêng | Mở khóa | Content chỉ-Ẩn-do-lock tự public lại; content có moderation action riêng vẫn Ẩn. |
| TC-US16-010 | Rating lock/unlock | U1 có rating | Khóa rồi mở account | Rating bị loại aggregate khi khóa và tự tính lại khi mở. |
| TC-US16-011 | Appeal | U1 bị sanction | Gửi appeal, Admin giữ/gỡ | Appeal có state, quyết định và audit. |
| TC-US16-012 | Appeal SLA | Appeal >48h chưa xử lý | Mở queue | Gắn Quá SLA, ưu tiên lên đầu; sanction vẫn hiệu lực. |
| TC-US16-013 | Audit retention | Có audit cũ | Kiểm tra retention | Audit giữ 2 năm độc lập soft-delete 90 ngày. |
| TC-US16-014 | Audit immutability | Role vận hành thường | Thử sửa/xóa audit | Bị chặn. |
| TC-US16-015 | No auto-sanction | C1 chỉ có Report chưa xác minh | Tạo nhiều Report | Không tự cảnh báo/khóa chỉ từ Report count. |
