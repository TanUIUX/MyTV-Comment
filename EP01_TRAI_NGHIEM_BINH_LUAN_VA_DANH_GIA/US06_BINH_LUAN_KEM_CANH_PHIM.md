# US06 — Bình luận kèm mốc thời gian

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là người dùng đang xem phim/tập VOD**, tôi muốn gắn một mốc thời gian vào comment/reply, để người khác biết chính xác đoạn nội dung tôi đang thảo luận và có thể nhảy tới đoạn đó.

### Giá trị

- Giữ ngữ cảnh thảo luận gắn với nội dung mà không cần xử lý media.
- Giảm phụ thuộc DRM/bản quyền/renderer so với frame/clip.
- Tạo trải nghiệm thảo luận theo khoảnh khắc đơn giản cho MVP.

### Ưu tiên

**Could**

### Phạm vi MVP

- **Không triển khai tạo/chia sẻ frame hoặc clip trong phiên bản hiện tại.**
- Chỉ hỗ trợ gắn timestamp vào comment/reply của nội dung VOD/timeline cố định.
- Live/đang phát trực tiếp **ngoài scope hiện tại**.

### Acceptance Criteria

1. User có thể bấm **“Gắn thời gian hiện tại”** để lấy timestamp từ player.
2. User có thể chỉnh tay timestamp trước khi gửi theo định dạng được UI hỗ trợ (`mm:ss` hoặc `hh:mm:ss` khi cần).
3. Mỗi comment/reply được gắn tối đa **1 timestamp**.
4. Timestamp phải thuộc phạm vi thời lượng của đúng phim/tập đang bình luận; giá trị âm hoặc vượt duration bị chặn.
5. Comment/reply lưu content ID, episode ID và timestamp cần thiết để tái tạo ngữ cảnh; không lưu/tạo frame/clip.
6. Timestamp hiển thị dạng có thể bấm, ví dụ `18:00`.
7. Khi user bấm timestamp và có entitlement xem nội dung, player **seek trực tiếp tới đúng mốc và tiếp tục phát từ đó**. Nếu chưa có entitlement, hệ thống không seek/không phát và hiển thị CTA xem tập phim để user hoàn tất quyền xem trước.
8. Timestamp không làm thay đổi rule Spoiler/moderation của phần text.
9. Nếu nội dung/timestamp không còn khả dụng, text comment vẫn hiển thị và timestamp được disable/hiển thị trạng thái không khả dụng; không làm hỏng thread.
10. Không cho upload ảnh/video cá nhân trong luồng này.

### Quy tắc nghiệp vụ

- Một comment/reply tối đa một timestamp.
- Không cho timestamp trỏ sang tập khác.
- Không hỗ trợ frame/clip/watermark/DRM media trong scope hiện tại; các nhu cầu đó chuyển Future/Phase sau.
- Deep link chia sẻ comment có timestamp không tự seek player; người nhận mở đúng comment rồi chủ động bấm timestamp theo US18.
- Entitlement là gate bắt buộc trước playback: có quyền xem mới seek/play; chưa có quyền chỉ hiển thị CTA `[Xem ngay]`, không phát nội dung và không làm mất thread.

*Xem thêm: [REQUIREMENTS_A11Y_SECURITY.md](../REQUIREMENTS_A11Y_SECURITY.md) — timestamp phải là `<button>` thật, accessible name "Xem từ phút {m} giây {s}".*

### Điểm cần PO chốt

- Không còn blocker PO cho timestamp MVP hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US06-001 | Functional | U1 đang xem E1 ở 18:00 | Bấm “Gắn thời gian hiện tại”, gửi | Comment lưu timestamp 18:00 đúng E1. |
| TC-US06-002 | Manual edit | Player ở 18:00 | Gắn rồi sửa tay thành 17:55 | Timestamp hợp lệ được lưu 17:55. |
| TC-US06-003 | Boundary | Nội dung duration 45:00 | Gửi 00:00, 45:00, 45:01, số âm | Giá trị trong phạm vi được nhận; vượt/âm bị chặn theo boundary player thực tế. |
| TC-US06-004 | Max one | Comment đã có timestamp | Thử gắn timestamp thứ hai | Không cho lưu hơn 1 timestamp/comment hoặc reply. |
| TC-US06-005 | Scope | Đang comment E1 | Sửa request trỏ timestamp sang E2 | API từ chối mismatch. |
| TC-US06-006 | Playback | Comment có `18:00`; U1 có entitlement | Bấm timestamp | Player seek 18:00 và tiếp tục phát. |
| TC-US06-012 | Entitlement | Comment có `18:00`; U1 chưa có entitlement | Bấm timestamp | Không seek/không phát; hiển thị CTA “Xem tập phim để nhảy tới mốc 18:00” + `[Xem ngay]`; thread/comment vẫn nguyên vẹn. |
| TC-US06-007 | Fallback | Source/timestamp không còn khả dụng | Mở comment | Text/thread vẫn hiển thị; timestamp không gây crash và có trạng thái thay thế. |
| TC-US06-008 | No media | Kiểm tra UI/API | Tìm chức năng frame/clip/upload | Không có chức năng tạo/share frame/clip hoặc upload media cá nhân trong MVP. |
| TC-US06-009 | Live scope | Nội dung Live | Kiểm tra timestamp comment | Không cung cấp tính năng timestamp theo US06 trong scope hiện tại. |
| TC-US06-010 | Spoiler + timestamp cùng lúc | Comment vừa bật Spoiler vừa có timestamp hợp lệ | Gửi và mở bằng U2 | Timestamp không làm thay đổi rule Spoiler/moderation của phần text; text vẫn bị che theo Spoiler cho tới khi U2 chủ động mở, timestamp vẫn hiển thị/hoạt động độc lập với trạng thái che. |
| TC-US06-011 | Timestamp format sai | Chuỗi "abc", "99:99", "12:60" nhập làm timestamp | Gửi qua UI/API | Toàn bộ giá trị định dạng sai bị chặn, không được lưu thành timestamp hợp lệ. |

### Microcopy

| Trạng thái | Nội dung hiển thị |
|---|---|
| Mốc thời gian không còn khả dụng | **Mốc thời gian không còn khả dụng**<br>Nội dung tại mốc này hiện không thể phát.<br>(timestamp hiển thị dạng disable, không bấm được) |
| Bấm timestamp khi chưa có player đang chạy (đã có quyền xem) | Hệ thống khởi tạo player và phát trực tiếp từ mốc `mm:ss` được gắn. |
| Bấm timestamp khi chưa có quyền xem tập | **Xem tập phim để nhảy tới mốc {mm:ss}**<br>Bạn cần xem tập phim này để phát từ mốc được nhắc đến.<br>`[Xem ngay]` |
