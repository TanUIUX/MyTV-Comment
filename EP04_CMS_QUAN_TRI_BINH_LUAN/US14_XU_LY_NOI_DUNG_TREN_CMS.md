# US14 — Xử lý nội dung trên CMS

> Thuộc EP04 — CMS quản trị bình luận
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là Admin/Moderator kiểm duyệt**, tôi muốn duyệt, từ chối, ẩn, xóa và xử lý Report/Flag/Spoiler, để thực thi chính sách cộng đồng nhất quán và có audit.

### Ưu tiên

**Must**

### Reason taxonomy chuẩn

Các action bắt buộc reason dùng chung taxonomy AI/Report/CMS:
- Spoiler
- Spam/quảng cáo
- Xúc phạm/ngôn từ công kích
- Nội dung không phù hợp
- Sai thông tin
- Vi phạm khác

### Acceptance Criteria — Moderation action

1. Admin có thể Duyệt nội dung Chờ duyệt → Hiển thị.
2. Admin có thể Từ chối nội dung Chờ duyệt, Ẩn nội dung đang Hiển thị và Xóa mềm comment/reply sau xác nhận.
3. **Duyệt không cần reason**.
4. **Chỉ Từ chối / Ẩn / Xóa bắt buộc reason**: Admin phải chọn một reason chuẩn; có ô ghi chú tùy chọn.
5. **Bỏ qua Report / gắn-bỏ Flag / thêm-bỏ Spoiler không bắt buộc reason**.
6. Từ chối/Ẩn/Xóa phải làm tác giả thấy reason trong app và nhận **push + in-app notification**; notification này không bị tắt bởi community notification setting.
7. Xóa root comment làm toàn bộ thread không public; Xóa tại CMS là soft-delete 90 ngày.
8. CMS kiểm tra state hiện tại trước khi lưu để tránh ghi đè im lặng giữa nhiều Admin.

### Acceptance Criteria — Report/Flag/Spoiler

1. Admin xem reporter, reason, description, time và version target.
2. Chỉ có một action giữ nội dung hợp lệ là **“Bỏ qua Report”**: đóng Report và giữ comment ở trạng thái Hiển thị nếu không có moderation action khác.
3. Không dùng action “Duyệt giữ nguyên” riêng để tránh trùng semantics.
4. Nếu kết luận vi phạm, Admin dùng Ẩn/Xóa hoặc action moderation phù hợp.
5. Nhiều Report không tự đổi state trước quyết định CMS.
6. Lịch sử xử lý từng Report được lưu và reporter nhận in-app notification theo US10.
7. Gắn/bỏ **Flag** và thêm/bỏ **Spoiler** phải cập nhật đúng metadata/state hiển thị tương ứng của item và tạo audit; các action này không tự đồng nghĩa với Từ chối/Ẩn/Xóa nếu Admin chưa chọn moderation action đó.

### Acceptance Criteria — Bulk moderation

1. MVP chỉ hỗ trợ bulk cho đúng 4 action: **Duyệt / Từ chối / Ẩn / Xóa mềm**, tối đa **100 comment/reply/lần**.
2. **Bỏ qua Report / Flag / Spoiler không hỗ trợ bulk** trong MVP và phải xử lý từng item.
3. Hệ thống validate quyền/state từng item trước khi xử lý.
4. Bulk dùng **partial success**: item hợp lệ vẫn thành công; item lỗi báo rõ; không rollback item đã thành công.
5. Bulk **Duyệt không cần reason**. Với bulk **Từ chối/Ẩn/Xóa**, Admin chọn **một reason chung cho batch** và có thể **override reason riêng từng item**.
6. Không được báo “thành công toàn bộ” nếu có item thất bại.

### Acceptance Criteria — Undo

1. CMS hỗ trợ Undo **Từ chối / Ẩn / Xóa mềm** khi dữ liệu còn trong thời gian lưu trữ.
2. Undo khôi phục state theo quy tắc đã chốt:
   - Undo **Từ chối** → **Chờ duyệt**.
   - Undo **Ẩn** → **Hiển thị**.
   - Undo **Xóa mềm** → **state ngay trước khi Xóa**.
3. Nếu Undo Xóa mềm một **root comment**, hệ thống khôi phục root và toàn bộ reply còn retention đã biến mất chỉ do cascade root delete; mỗi reply quay về state riêng trước cascade. Reply có moderation state riêng trước đó không bị tự public lại.
4. Mọi Admin/Moderator có quyền moderation tương ứng đều được Undo, không cần là người thực hiện action ban đầu.
5. **Undo không bắt buộc nhập reason**.
6. Undo tạo audit event mới với actor/time/action/before-after; không xóa/sửa audit cũ.

### Acceptance Criteria — SLA moderation

1. Item Chờ duyệt có SLA **24 giờ** kể từ lúc vào moderation queue.
2. CMS hiển thị thời gian chờ, cảnh báo item sắp/quá SLA.
3. Quá 24 giờ vẫn giữ **Chờ duyệt**, đánh dấu **Quá SLA** và ưu tiên lên đầu trong cùng nhóm risk.
4. Không tự Duyệt/Từ chối chỉ vì quá SLA.

### Quy tắc nghiệp vụ

- Duyệt no reason; Từ chối/Ẩn/Xóa = reason chuẩn bắt buộc + note optional.
- Flag/Spoiler/Bỏ qua Report không bắt buộc reason và không bulk trong MVP.
- Bulk = Duyệt/Từ chối/Ẩn/Xóa mềm, max 100, partial success.
- Undo là action sửa sai độc lập: Từ chối→Chờ duyệt, Ẩn→Hiển thị, Xóa mềm→state trước Xóa; mọi Undo phải audit.
- Undo root delete bảo toàn state riêng từng reply và chỉ đảo phần cascade do root delete gây ra.

### Điểm cần PO chốt

- Không còn blocker PO cho moderation CMS trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US14-001 | Approve | C1 Chờ duyệt | Duyệt không nhập reason | C1 Hiển thị; audit before-after; không validation reason. |
| TC-US14-002 | Reason required | Từ chối/Ẩn/Xóa | Bỏ reason rồi xác nhận | Không cho lưu. Chọn taxonomy hợp lệ thì xử lý được; note là optional. |
| TC-US14-003 | No-reason actions | Bỏ qua Report/Flag/Spoiler | Thực hiện không nhập reason | Action được phép và có audit. |
| TC-US14-004 | Flag/Spoiler effect | C1 đang Hiển thị | Gắn rồi bỏ Flag; thêm rồi bỏ Spoiler | Metadata/UI state tương ứng thay đổi đúng action; C1 không tự Từ chối/Ẩn/Xóa nếu chưa có moderation action riêng; audit đầy đủ. |
| TC-US14-005 | Author notification | Admin Từ chối/Ẩn/Xóa C1 | U1 kiểm tra app/push | U1 thấy reason và nhận push + in-app bắt buộc. |
| TC-US14-006 | Delete cascade | C1 có R1/R2 | Xóa mềm C1 | Toàn thread biến mất public; dữ liệu còn retention 90 ngày. |
| TC-US14-007 | Concurrency | A/B cùng mở C1 | A xử lý trước, B lưu state cũ | B nhận conflict; không ghi đè im lặng. |
| TC-US14-008 | Report semantics | C1 có Report nhưng không vi phạm | Chọn “Bỏ qua Report” | Report đóng; C1 tiếp tục Hiển thị; không có “Duyệt giữ nguyên” riêng. |
| TC-US14-009 | Bulk allowed actions | Có item phù hợp cho Duyệt/Từ chối/Ẩn/Xóa và item có Report/Flag/Spoiler | Mở bulk action | Chỉ có 4 action Duyệt/Từ chối/Ẩn/Xóa mềm; Report/Flag/Spoiler không có bulk action. |
| TC-US14-010 | Bulk boundary | 99/100/101 item | Thực hiện bulk | 99/100 được phép; 101 bị chặn/yêu cầu giảm selection. |
| TC-US14-011 | Partial success | 100 item có một số stale/unauthorized | Bulk | Item hợp lệ xử lý thành công; item lỗi báo riêng; không rollback. |
| TC-US14-012 | Bulk reason | Bulk Duyệt và bulk Từ chối/Ẩn/Xóa | Thực hiện từng loại | Duyệt không bắt reason; Từ chối/Ẩn/Xóa bắt reason chung và cho override từng item; lưu đúng reason cuối cùng. |
| TC-US14-013 | Undo reject | C1 bị Từ chối từ Chờ duyệt | Moderator khác có quyền Undo không nhập reason | C1 quay về Chờ duyệt; không bắt reason; audit event mới được tạo. |
| TC-US14-014 | Undo hide | C1 bị Ẩn từ Hiển thị | Undo | C1 quay về Hiển thị nếu không có gate khác; audit event mới. |
| TC-US14-015 | Undo soft delete | C1 bị Xóa mềm khi trước đó ở Hiển thị/Ẩn/Chờ duyệt | Undo từng case | C1 quay đúng state ngay trước Xóa, không mặc định về Hiển thị. |
| TC-US14-016 | Undo root cascade | C1 có R1 Hiển thị và R2 Ẩn riêng trước root delete | Xóa root rồi Undo trong retention | C1/R1 khôi phục theo state trước cascade; R2 vẫn Ẩn; không mất audit cũ. |
| TC-US14-017 | SLA | Item pending 23h/24h/>24h | Mở queue | Hiển thị thời gian/badge; >24h = Quá SLA và ưu tiên trong cùng risk, state vẫn pending. |
| TC-US14-018 | Authorization | Role chỉ đọc | Thử moderation/Undo/bulk | UI/API chặn action không được cấp. |
