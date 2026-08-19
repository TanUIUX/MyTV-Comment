# User Flow — US18 Chia sẻ bình luận

> User Story: [US18_CHIA_SE_BINH_LUAN_VA_CANH_PHIM.md](US18_CHIA_SE_BINH_LUAN_VA_CANH_PHIM.md)

**Platforms:** Phone, Web. SmartTV không Share.

```mermaid
flowchart TD
 A[Menu ⋯ → Chia sẻ] --> B{Đã login + target public?}
 B -->|Không| C[Auth/unavailable]
 B -->|Có + Phone| D[Mở OS share sheet ngay]
 D --> E{Mở thành công?}
 E -->|Có| F[Ghi 1 Share event]
 B -->|Có + Web| G[Mở Share dialog]
 G --> H[Facebook / Zalo / Sao chép liên kết]
 F --> I[Recipient mở deep link]
 H --> I
 I --> J[App nếu có, nếu không MyTV Web]
 J --> K[Visibility Resolver → target hoặc fallback]
```

## UX đã chốt

- Share nằm trong menu `⋯`, không phải action ưu tiên cạnh Like/Reply.
- Phone không có preview/confirm trung gian; mở OS sheet ngay.
- Web dùng dialog riêng: **Facebook, Zalo, Sao chép liên kết**; không TikTok ở MVP Web.
- Payload/preview không chứa nguyên văn comment, nickname/phone/Spoiler; CTA `Xem nội dung này trên MyTV`.
- SmartTV không Share.
