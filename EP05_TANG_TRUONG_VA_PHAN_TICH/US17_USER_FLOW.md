# User Flow — US17 Huy hiệu người dùng

> User Story: [US17_HUY_HIEU_NGUOI_DUNG.md](US17_HUY_HIEU_NGUOI_DUNG.md)

```mermaid
flowchart TD
 A[Render nickname trong Comment/Reply] --> B{Có badge hợp lệ?}
 B -->|Không| C[Chỉ nickname]
 B -->|Có| D[Hiển thị 1 badge ưu tiên cao nhất: icon + tên]
 D --> E{Phone/Web tap/click badge?}
 E -->|Có| F[Mở bottom sheet/popover: tên + ý nghĩa + tiêu chí tổng quát]
 G[SmartTV] --> H[Chỉ hiển thị icon + tên, không interaction badge]
```

## UX đã chốt

- Ví dụ hiển thị: `Minh Anh · Fan kỳ cựu`.
- Không chỉ dùng icon/màu; phải có text alternative.
- Phone/Web mở giải thích badge tại chỗ, không rời thread.
- SmartTV chỉ hiển thị, không mở detail badge.
- Chỉ hiển thị 1 badge theo priority rule hiện hành.
