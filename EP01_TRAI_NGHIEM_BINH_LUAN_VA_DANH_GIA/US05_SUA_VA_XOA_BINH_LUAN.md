# US05 — Sửa và xóa bình luận

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là tác giả bình luận**, tôi muốn sửa hoặc xóa nội dung của chính mình, để cập nhật ý kiến hoặc loại bỏ nội dung không còn muốn hiển thị.

### Ưu tiên

**Must**

### Acceptance Criteria

1. Tác giả có thể sửa comment/reply của mình bất kỳ lúc nào khi còn quyền bình luận.
2. Nội dung sửa tạo version mới và đi qua moderation; người khác tiếp tục thấy version công khai cũ trong lúc version mới Chờ duyệt.
3. Khi version mới được duyệt, hệ thống thay thế nội dung công khai nhưng giữ nguyên comment ID, Like và thread; comment hiển thị nhãn **“Đã chỉnh sửa”**.
4. Khi version mới bị Từ chối hoặc bị AI mức Nặng chặn, version công khai gần nhất vẫn tiếp tục Hiển thị.
5. Người dùng cuối chỉ xem version hiện tại + nhãn “Đã chỉnh sửa”; toàn bộ version history chỉ CMS/Audit được xem.
6. Tác giả có thể xóa comment gốc/reply sau bước xác nhận; **user tự xóa không cần nhập reason**.
7. Xóa comment gốc làm toàn bộ thread biến mất khỏi UI/API công khai; xóa reply riêng lẻ chỉ loại reply đó và **không hiển thị placeholder**.
8. Nội dung user tự xóa được soft-delete 90 ngày; user **không tự khôi phục** được.
9. Admin có quyền phù hợp có thể Undo/khôi phục dữ liệu còn trong thời gian lưu trữ theo US14; mọi lần khôi phục ghi audit.
10. Nếu Undo Xóa mềm một **root comment**, hệ thống khôi phục root và toàn bộ reply còn trong retention đã biến mất **chỉ do cascade từ root delete**; mỗi reply quay về state riêng ngay trước cascade. Reply đã bị Ẩn/Từ chối/Xóa riêng trước đó không tự public lại.
11. Nếu user xóa comment trong khi có version sửa Chờ duyệt, version chờ bị vô hiệu hóa và không thể public sau đó.
12. Người khác không thể sửa/xóa nội dung không thuộc sở hữu của họ.

### Quy tắc nghiệp vụ

- Không giới hạn thời gian sửa.
- User delete không yêu cầu reason; audit dùng action/system code để ghi nhận đây là thao tác tác giả tự xóa.
- Version history phục vụ CMS/Audit, không public cho user.
- Soft-delete 90 ngày không đồng nghĩa user có quyền restore.
- Cascade root delete là visibility/lifecycle của thread; Undo root delete không được ghi đè moderation state riêng đã có trước đó của từng reply.

### Điểm cần PO chốt

- Không còn blocker PO cho edit/delete trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US05-001 | Authorization | U1 sở hữu C1; U2 không | U1/U2 thử sửa/xóa | Chỉ U1 được phép. |
| TC-US05-002 | Versioning | C1 Hiển thị | U1 sửa | Tạo version mới; C1 cũ vẫn public trong lúc chờ. |
| TC-US05-003 | Approve | V2 được duyệt | Refresh | V2 public, giữ ID/Like/thread và có nhãn “Đã chỉnh sửa”. |
| TC-US05-004 | Reject/heavy | V2 bị Từ chối hoặc AI mức Nặng | Mở bằng U2 | C1 cũ vẫn public; V2 không thay thế. |
| TC-US05-005 | Version history | C1 đã sửa nhiều lần | U1 mở app và Admin mở CMS | U1 chỉ thấy current + “Đã chỉnh sửa”; CMS/Audit thấy history. |
| TC-US05-006 | Delete root | C1 có R1/R2 | U1 xác nhận xóa không nhập reason | C1 + toàn thread biến mất công khai; không bắt reason. |
| TC-US05-007 | Delete reply | R1 thuộc U1 | Xóa R1 | R1 biến mất hoàn toàn, không placeholder; C1/R2 giữ nguyên. |
| TC-US05-008 | Retention | C1/R1 đã xóa | Tra CMS trong 90 ngày | Record soft-delete còn phục vụ audit, có actor/time/action; reason user nhập không bắt buộc. |
| TC-US05-009 | Restore authorization | C1 soft-delete | U1 thử restore; Admin có quyền Undo | U1 bị chặn; Admin có thể khôi phục theo US14 và có audit. |
| TC-US05-010 | Undo root cascade | C1 trước khi xóa có R1 Hiển thị, R2 Ẩn riêng; root delete làm cả thread biến mất | Admin Undo root delete trong retention | C1 và R1 trở lại state trước cascade; R2 vẫn Ẩn vì có moderation state riêng; không mất audit. |
| TC-US05-011 | Race | C1 có V2 Chờ duyệt | U1 xóa C1 rồi Admin duyệt V2 | V2 không được public sau khi root đã bị xóa. |
