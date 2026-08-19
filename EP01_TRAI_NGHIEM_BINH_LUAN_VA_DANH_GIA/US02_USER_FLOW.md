# User Flow — US02 Số lượng và sắp xếp bình luận theo nội dung hiện tại

> User Story legacy filename: [US02_XEM_BINH_LUAN_THEO_SERIES_TAP_SO_LUONG_VA_SAP_XEP.md](US02_XEM_BINH_LUAN_THEO_SERIES_TAP_SO_LUONG_VA_SAP_XEP.md)

**Platforms:** Phone, Web, SmartTV

## Flow

```mermaid
flowchart TD
 A[Mở tab Bình luận] --> B[Xác định content scope hiện tại]
 B --> C[Phim lẻ = phim; phim bộ = tập hiện tại]
 C --> D[Load public count + rating + pin + root list]
 D --> E[Sort mặc định Nổi bật]
 E --> F[Hiển thị tối đa 3 Pin luôn ở trên cùng]
 F --> G[Load 10 root đầu]
 G --> H{Cuộn gần cuối?}
 H -->|Có| I[Lazy load tối đa 10 root/batch]
 I --> H
 G --> J{Đổi sort?}
 J --> K[Dropdown: Nổi bật / Mới nhất / Nhiều lượt thích]
 K --> G
 L[Phim bộ đổi tập] --> M[Reset sort Nổi bật + reload toàn dữ liệu tập mới]
```

## UX đã chốt

- **Không có scope `Toàn bộ phim/Series` phía người xem.** Phim bộ chỉ có comment của tập hiện tại; phim lẻ ở cấp phim.
- Pin tối đa 3, luôn ở đầu trong mọi sort, có `📌 Đã ghim`, không lặp trong list thường.
- Sort control dạng dropdown `Nổi bật ▼`.
- Root comment lazy load **10/batch**.
- Khi user bấm indicator comment mới: `Nổi bật` và `Nhiều lượt thích` luôn giữ ranking đúng, không ép comment mới lên đầu; `Mới nhất` có thể đưa user tới comment mới đầu tiên.
- SmartTV vẫn được đổi sort bằng remote.

## States

Loading/skeleton · Empty · Loading more · End list · New-content indicator · Sort menu.
