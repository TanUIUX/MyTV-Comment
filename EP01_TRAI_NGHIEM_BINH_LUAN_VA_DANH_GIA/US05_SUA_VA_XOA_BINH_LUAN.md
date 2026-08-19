# US05 — Sửa và xóa bình luận

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là tác giả bình luận**, tôi muốn sửa hoặc xóa nội dung của chính mình, để cập nhật ý kiến hoặc loại bỏ nội dung không còn muốn hiển thị.

### Ưu tiên

**Must**

### Acceptance Criteria

1. Tác giả có thể sửa comment/reply của mình bất kỳ lúc nào khi tài khoản còn quyền sửa; user đang **Khóa bình luận** không được tạo edit mới, nhưng vẫn được **tự xóa** nội dung của mình.
2. Nội dung sửa tạo version mới và đi qua moderation; người khác tiếp tục thấy version công khai cũ trong lúc version mới Chờ duyệt.
3. Khi version mới được duyệt, hệ thống thay thế nội dung công khai nhưng giữ nguyên comment ID, Like và thread; comment hiển thị nhãn **“Đã chỉnh sửa”**.
4. Khi version mới bị Từ chối hoặc bị AI mức Nặng chặn, version công khai gần nhất vẫn tiếp tục Hiển thị.
5. Người dùng cuối chỉ xem version hiện tại + nhãn “Đã chỉnh sửa”; toàn bộ version history chỉ CMS/Audit được xem.
6. Tác giả có thể xóa comment gốc/reply sau bước xác nhận; **user tự xóa không cần nhập reason**.
7. User tự xóa **root comment** làm root và **toàn bộ reply trong thread cùng bị soft-delete**, toàn thread biến mất khỏi UI/API công khai; reply cascade theo root chỉ mang cờ `hidden_by_root_cascade = true`, không có đồng hồ retention riêng và không đổi `moderation_state` gốc của reply. User tự xóa một reply riêng lẻ chỉ soft-delete reply đó và **không hiển thị placeholder**.
8. Nội dung user tự xóa được soft-delete **90 ngày** (áp dụng cho root) để audit/compliance; user không tự khôi phục được và **Admin cũng không được Undo để public lại** nội dung do user self-delete.
9. Undo/khôi phục tại US14 chỉ áp dụng cho **Xóa mềm do CMS/Admin**. Nếu Admin Undo một root do CMS/Admin đã Xóa mềm, hệ thống khôi phục root và toàn bộ reply bị ẩn do cascade cùng lúc, với điều kiện **ROOT còn trong 90 ngày retention** (không phải "reply còn retention"); mỗi reply quay về state riêng trước cascade (dựa trên cờ `hidden_by_root_cascade`), không ghi đè moderation state riêng.
10. Nếu user xóa comment trong khi có version sửa Chờ duyệt, version chờ bị vô hiệu hóa và không thể public sau đó.
11. Nếu một version sửa đã được gửi hợp lệ **trước effective time của Khóa bình luận**, moderation queue vẫn xử lý bình thường trong thời gian user bị khóa; nếu được Duyệt thì version mới thay bản cũ và public. Khóa bình luận chỉ chặn edit mới từ effective time.
12. Người khác không thể sửa/xóa nội dung không thuộc sở hữu của họ.
13. Khi user self-delete root làm reply của user khác bị cascade soft-delete (mang cờ `hidden_by_root_cascade = true`), các reply đó được coi là **đã Xóa mềm** cho eligibility của badge/KPI; không giữ contribution badge chỉ vì tác giả reply không phải người xóa root.

### Quy tắc nghiệp vụ

- Không giới hạn thời gian sửa khi user còn quyền sửa.
- Khóa bình luận chặn **Edit mới** nhưng không chặn **self-delete**.
- User delete không yêu cầu reason; audit dùng action/system code để ghi nhận đây là thao tác tác giả tự xóa.
- Version history phục vụ CMS/Audit, không public cho user.
- Soft-delete 90 ngày không đồng nghĩa user hoặc Admin có quyền restore content self-delete ra public.
- **Self-delete root = cascade soft-delete thật toàn thread**; khác với visibility cascade do Account Lock hoặc Admin Hide root.
- Reply cascade theo root **không có retention riêng**, chỉ mang cờ `hidden_by_root_cascade`; retention 90 ngày chỉ áp dụng cho root. Điều kiện Undo dựa trên **ROOT còn retention 90 ngày**, không phải "reply còn retention".
- Undo root delete tại US14 chỉ đảo action **CMS/Admin soft-delete**, không áp dụng self-delete của user.
- Pending edit gửi trước sanction không bị hồi tố hủy chỉ vì Khóa bình luận có hiệu lực sau đó.

### Phụ thuộc

- US11 — AI moderation cho version sửa.
- US14 — Undo moderation do CMS/Admin.
- US16 — Khóa bình luận/Account Lock.
- US17/US19 — Eligibility badge và KPI khi content bị cascade soft-delete.

### Điểm cần PO chốt

- Không còn blocker PO cho edit/delete trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US05-001 | Authorization | U1 sở hữu C1; U2 không | U1/U2 thử sửa/xóa | Chỉ U1 được phép thao tác content của U1. |
| TC-US05-002 | Versioning | C1 Hiển thị | U1 sửa | Tạo version mới; C1 cũ vẫn public trong lúc chờ. |
| TC-US05-003 | Approve | V2 được duyệt | Refresh | V2 public, giữ ID/Like/thread và có nhãn “Đã chỉnh sửa”. |
| TC-US05-004 | Reject/heavy | V2 bị Từ chối hoặc AI mức Nặng | Mở bằng U2 | C1 cũ vẫn public; V2 không thay thế. |
| TC-US05-005 | Version history | C1 đã sửa nhiều lần | U1 mở app và Admin mở CMS | U1 chỉ thấy current + “Đã chỉnh sửa”; CMS/Audit thấy history. |
| TC-US05-006 | Self-delete root cascade | C1 của U1 có R1 của U2 và R2 của U3 | U1 xác nhận xóa root không nhập reason | C1 + R1 + R2 cùng soft-delete, toàn thread biến mất công khai; không bắt reason. |
| TC-US05-007 | Delete reply | R1 thuộc U1 | U1 xóa R1 | R1 soft-delete và biến mất hoàn toàn, không placeholder; C1/reply khác giữ nguyên. |
| TC-US05-008 | Retention | Thread/user content đã self-delete | Tra CMS trong 90 ngày | Record soft-delete còn phục vụ audit/compliance, có actor/time/action; không public. |
| TC-US05-009 | No restore self-delete | C1 do U1 self-delete, còn trong 90 ngày | U1 và Admin thử restore/Undo | U1 bị chặn; Admin không được Undo để public lại self-delete. |
| TC-US05-010 | Admin delete Undo cascade | C1 bị CMS/Admin soft-delete (root còn trong 90 ngày retention); trước đó R1 Hiển thị, R2 Ẩn riêng | Admin Undo root delete trong retention của ROOT | C1/R1 trở lại state trước cascade (cờ `hidden_by_root_cascade` được gỡ); R2 vẫn Ẩn (moderation_state riêng không đổi); rule này không áp dụng self-delete. |
| TC-US05-011 | Race self-delete | C1 có V2 Chờ duyệt | U1 xóa C1 rồi Admin duyệt V2 | V2 không được public sau khi C1 đã self-delete. |
| TC-US05-012 | Comment lock edit/delete | U1 đang Khóa bình luận và có C1 public | Thử Edit C1 rồi Self-delete C1 | Edit mới bị chặn; self-delete vẫn được phép. |
| TC-US05-013 | Pending edit before lock | U1 gửi V2 khi chưa khóa; sau đó bị Khóa bình luận | Admin Duyệt V2 trong thời gian khóa | V2 được Duyệt và public bình thường; sanction không hồi tố edit đã gửi trước effective time. |
| TC-US05-014 | Badge cascade integration | R1 của U2 nằm dưới C1 của U1; R1 từng đóng góp Fan badge | U1 self-delete C1 | R1 cascade soft-delete; R1/Like gắn với R1 không tiếp tục được coi là contribution badge hợp lệ theo US17. |
| TC-US05-015 | Retention boundary | Root C1 bị CMS/Admin soft-delete lần lượt tại mốc D-89, D-90, D-91 kể từ ngày soft-delete; R1 cascade theo root | Admin thử Undo tại từng mốc; đối chiếu audit log | D-89/D-90: Undo hoạt động, root + R1 khôi phục đúng state trước cascade; D-91: Undo bị từ chối vì root đã hết 90 ngày retention. Ở cả 3 mốc, audit log vẫn giữ đủ record tối thiểu 2 năm, độc lập với retention 90 ngày của nội dung. |
| TC-US05-016 | Concurrency edit versions | C1 có V2 đang Chờ duyệt | U1 gửi V3 trong khi V2 còn Chờ duyệt (trước khi Admin quyết định) | Hệ thống không cho 2 version pending tranh nhau; V2 bị vô hiệu hóa/thay thế bởi V3 làm version chờ duyệt hiện hành; không có 2 version cùng chờ song song. |
| TC-US05-017 | Concurrency race edit vs approve | C1 có V2 Chờ duyệt | U1 gửi V3 đúng lúc Admin bấm Duyệt V2 | Chỉ đúng 1 version được public ở thời điểm cuối cùng và có nhãn “Đã chỉnh sửa”; không có 2 version cùng public hoặc xung đột dữ liệu; version history đầy đủ V1→V2→V3 (kể cả version bị vô hiệu hóa do race) vẫn được ghi nhận cho CMS/Audit. |

### Microcopy

| Trạng thái | Nội dung hiển thị |
|---|---|
| Xác nhận xóa root có cascade | **Xóa bình luận này?**<br>{n} phản hồi của người khác trong thảo luận này cũng sẽ bị xóa theo. Thao tác này không thể hoàn tác — kể cả bộ phận hỗ trợ cũng không khôi phục được.<br>`[Xóa cả thảo luận]` `[Hủy]` |
| Nhãn đã chỉnh sửa | **Đã chỉnh sửa · {thời gian tương đối}** |
