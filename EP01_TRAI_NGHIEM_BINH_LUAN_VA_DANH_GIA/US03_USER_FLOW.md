# User Flow — US03 Đánh giá 5 sao

> User Story: [US03_DANH_GIA_SERIES_VA_TAP_PHIM.md](US03_DANH_GIA_SERIES_VA_TAP_PHIM.md)

**Platforms:** Phone, Web, SmartTV

## Flow

```mermaid
flowchart TD
 A[Mở tab Bình luận] --> B{total rating > 0?}
 B -->|Có| C[Hiển thị 5 sao + average + total]
 B -->|Không| D[Ẩn toàn bộ khối rating]
 C --> E{User chọn 1–5 sao?}
 E -->|Guest| F[Auth gate]
 F --> G[Login → quay lại đúng content, không auto rating]
 E -->|Đã login| H[Submit ngay]
 G --> H
 H --> I{Server success?}
 I -->|Có| J[Cập nhật rating hiện hành + aggregate]
 I -->|Không| K[Revert rating hợp lệ trước đó + báo lỗi]
 J --> L[Muốn thay đổi → chọn số sao khác]
 M[Player phát content_completed: 90% duration hoặc end event, lần đầu phiên xem] --> M2{Scope đang Đóng bình luận?}
 M2 -->|Có| M3[Không mở prompt]
 M2 -->|Không| N[Mở Post-watch Rating Prompt riêng]
 N --> O{Đã login?}
 O -->|Không| P[Auth gate → quay lại prompt, không auto rating]
 O -->|Có| Q[Chọn 1–5 sao → submit]
 P --> Q
```

## UX/Business đã chốt

- Phim lẻ: rating cấp phim. Phim bộ: rating **chỉ cấp tập hiện tại**; không còn rating Series.
- Một account có 1 rating hiện hành/content scope.
- User **được thay đổi 1–5 sao nhưng không được xóa rating**.
- Chọn sao là submit ngay, không có nút `Gửi đánh giá`.
- Submit lỗi: revert rating cũ, không tự retry.
- SmartTV cho đánh giá bằng remote nếu đã đăng nhập; prompt `O11` có biến thể remote.
- Prompt post-watch **không mở khi scope đang Đóng bình luận**; Khóa bình luận không chặn rating.
- Khi `total = 0`, tab không có khối rating và **không có empty-state** — rating đầu tiên chỉ đến từ prompt post-watch. Rủi ro cold-start đã được ghi nhận trong US03 mục Quy tắc nghiệp vụ.
- A11y: radiogroup, mỗi option label `{n} sao`.
