# User Flow — US08 Trả lời bình luận một cấp

> User Story: [US08_TRA_LOI_BINH_LUAN_MOT_CAP.md](US08_TRA_LOI_BINH_LUAN_MOT_CAP.md)

**Platforms:** Phone, Web; SmartTV chỉ đọc thread.

```mermaid
flowchart TD
 A[Root có reply] --> B[Hiển thị Xem n phản hồi, không preload reply trong root list]
 B --> C{Platform}
 C -->|Phone/Web| D[Expand thread inline]
 C -->|SmartTV| E[Mở trang Thread riêng chỉ đọc]
 D --> F[Load reply cũ → mới]
 F --> G[Cuộn gần cuối → lazy load tối đa 10 reply/batch]
 D --> H[Tap Trả lời]
 H --> I[Phone bottom sheet / Web inline composer]
 I --> J[Reply root hoặc reply khác]
 J --> K[Nếu reply một reply: tự @nickname nhưng lưu depth=1 dưới root]
```

## UX đã chốt

- Root list không hiển thị sẵn reply; chỉ action `Xem {n} phản hồi`.
- Reply sort **cũ → mới**, lazy load tối đa 10/batch.
- Có reply mới khi thread mở: indicator `Có {n} phản hồi mới`; user bấm → tải và cuộn tới reply mới đầu tiên.
- SmartTV mở trang thread riêng, chỉ đọc; không Reply.
