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
7. Khi user bấm timestamp, player **seek trực tiếp tới đúng mốc và tiếp tục phát từ đó**.
8. Timestamp không làm thay đổi rule Spoiler/moderation của phần text.
9. Nếu nội dung/timestamp không còn khả dụng, text comment vẫn hiển thị và timestamp được disable/hiển thị trạng thái không khả dụng; không làm hỏng thread.
10. Không cho upload ảnh/video cá nhân trong luồng này.

### Quy tắc nghiệp vụ

- Một comment/reply tối đa một timestamp.
- Không cho timestamp trỏ sang tập khác.
- Không hỗ trợ frame/clip/watermark/DRM media trong scope hiện tại; các nhu cầu đó chuyển Future/Phase sau.
- Deep link chia sẻ comment có timestamp không tự seek player; người nhận mở đúng comment rồi chủ động bấm timestamp theo US18.

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
| TC-US06-006 | Playback | Comment có `18:00` | Bấm timestamp | Player seek 18:00 và tiếp tục phát. |
| TC-US06-007 | Fallback | Source/timestamp không còn khả dụng | Mở comment | Text/thread vẫn hiển thị; timestamp không gây crash và có trạng thái thay thế. |
| TC-US06-008 | No media | Kiểm tra UI/API | Tìm chức năng frame/clip/upload | Không có chức năng tạo/share frame/clip hoặc upload media cá nhân trong MVP. |
| TC-US06-009 | Live scope | Nội dung Live | Kiểm tra timestamp comment | Không cung cấp tính năng timestamp theo US06 trong scope hiện tại. |
