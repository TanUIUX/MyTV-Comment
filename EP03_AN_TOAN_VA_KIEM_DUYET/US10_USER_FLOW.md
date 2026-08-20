# User Flow — US10 Report bình luận

> User Story: [US10_REPORT_BINH_LUAN_VI_PHAM.md](US10_REPORT_BINH_LUAN_VI_PHAM.md)

**Platforms:** Phone, Web. SmartTV không Report.

```mermaid
flowchart TD
 A[Menu ⋯ → Báo cáo] --> B{Đã login + target public + không phải content mình?}
 B -->|Không| C[Auth/action unavailable]
 B -->|Có| D{Cooldown 24h / limit 10 report·h?}
 D -->|Blocked| E["Thông báo cooldown kèm số giờ còn lại / rate-limit"]
 D -->|OK| F[Mở bottom sheet Report]
 F --> G[Chọn 1 trong 6 reason]
 G --> H{Vi phạm khác?}
 H -->|Có| I[Nhập mô tả bắt buộc 1–500]
 H -->|Không| J[Submit]
 I --> J
 J --> K[Success; target vẫn public đến khi CMS xử lý]
```

## UX đã chốt

- Report mở bằng bottom sheet trên Phone/Web.
- Sau report thành công, action `Báo cáo` vẫn hiện; tap lại trong 24h báo đúng microcopy đã chốt tại US10: **“Bạn đã báo cáo bình luận này — Có thể báo cáo lại sau {n} giờ nếu nội dung vẫn còn.”** `{n}` là số giờ còn lại của cooldown 24h, phải hiển thị động để user biết cần chờ bao lâu.
- SmartTV không có Report; dùng Phone/Web.
