# US08 — Trả lời bình luận một cấp

> Thuộc EP02 — Tương tác cộng đồng  
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md) · [User Flow →](US08_USER_FLOW.md)

### User Story

**Là user đăng nhập**, tôi muốn trả lời comment hoặc một reply để tham gia hội thoại nhưng thread vẫn dễ đọc.

### Quy tắc đã chốt

- Reply depth luôn = 1.
- Nếu tap `Trả lời` trên reply, reply mới vẫn nằm dưới root và composer tự chèn `@nickname` của người được trả lời.
- Root list **không hiển thị sẵn reply**; có `Xem {n} phản hồi`.
- Phone/Web: tap → expand thread inline. SmartTV: mở trang thread riêng **chỉ đọc**.
- Reply trong thread sort **cũ → mới**.
- Khi mở thread tải batch đầu; cuộn gần cuối lazy load tối đa **10 reply/batch**.
- Có reply mới khi thread mở → indicator `Có {n} phản hồi mới`; user bấm thì load và cuộn tới reply mới đầu tiên.
- Phone Reply composer = bottom sheet; Web = inline; SmartTV không Reply.
- Draft reply chỉ giữ trong phiên hiện tại.

## Acceptance Criteria trọng yếu

1. Guest chọn Reply → auth → quay lại target, không auto-reply.
2. Root non-public → reply thread không hiển thị độc lập.
3. Reply submit dùng moderation như Comment.
4. Focus/announcement phải giữ đúng khi expand/lazy load/new reply.
