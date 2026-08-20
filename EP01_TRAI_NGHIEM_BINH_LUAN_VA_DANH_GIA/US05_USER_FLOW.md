# User Flow — US05 Sửa và xóa bình luận

> User Story: [US05_SUA_VA_XOA_BINH_LUAN.md](US05_SUA_VA_XOA_BINH_LUAN.md)

**Platforms:** Phone, Web

```mermaid
flowchart TD
 A[Menu ⋯ trên content của mình] --> B{Chỉnh sửa / Xóa}
 B -->|Chỉnh sửa| C{Comment Lock hoặc >5 edit/rolling 60 giây/target?}
 C -->|Có| D[Chặn edit]
 C -->|Không| E[Mở editor]
 E --> F[Sửa text/emoji + Spoiler + timestamp]
 F --> G[Submit version mới → moderation]
 G -->|Pending| H[Giữ version public cũ; version mới pending]
 G -->|Approved| I[Replace cùng ID + nhãn Đã chỉnh sửa]
 G -->|Blocked/Rejected| J[Giữ version public gần nhất]
 B -->|Xóa reply| K[Confirm → self-delete reply]
 B -->|Xóa root| L[Confirm cảnh báo cascade reply]
 L --> M[Self-delete root + toàn reply; không Undo public]
```

## UX đã chốt

- Action của chính user nằm trong menu `⋯`.
- Edit được sửa toàn bộ text/emoji, Spoiler, timestamp.
- Pending content vẫn được edit; version pending cũ được thay bằng version mới, không tạo nhiều pending song song.
- Edit limit 5/rolling 60 giây/target.
- Self-delete root khác CMS delete; user không có Undo public.
