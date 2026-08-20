# User Flow — US04 Đăng bình luận

> User Story: [US04_DANG_BINH_LUAN.md](US04_DANG_BINH_LUAN.md)

**Platforms:** Phone, Web. SmartTV không tạo comment.

## Flow

```mermaid
flowchart TD
 A[Chọn Viết bình luận] --> B{Đã login?}
 B -->|Không| C[Auth gate → login → quay lại, user bấm lại]
 B -->|Có| D{Scope mở + không Comment Lock?}
 D -->|Không| E[Hiển thị trạng thái bị chặn]
 D -->|Có| F[Phone: bottom sheet / Web: composer inline]
 F --> G[Nhập text/emoji + Spoiler + timestamp]
 G --> H[Validate Unicode blocklist + 1–1000 grapheme + URL + rate limit 5 attempt/60s]
 H -->|Lỗi| I[Giữ draft + inline error, không tính attempt]
 H -->|Hợp lệ| J[AI moderation, tính 1 attempt]
 J -->|Heavy| K[Giữ composer + draft, báo lỗi để sửa; ghi blocked_attempt]
 J -->|Timeout/5xx| L2[Tạo record Chờ duyệt fail-safe, không mất nội dung]
 J -->|Pending| L[Đóng/clear sau submit + item inline Đang chờ duyệt chỉ tác giả thấy]
 J -->|Public| M[Đóng/clear sau submit + insert public]
 L2 --> L
```

## UX đã chốt

- Phone mở **bottom sheet**; Web expand composer inline.
- Draft tự lưu khi đóng composer nhưng **chỉ trong phiên hiện tại**; đóng app/reload/kết thúc phiên thì draft mất. Submit thành công clear draft.
- Composer hiển thị content context hiện tại.
- Nickname đổi được ở Profile và có shortcut `Đổi nickname` trong composer.
- Nickname AI timeout/5xx/down: **không đổi nickname**, giữ old/fallback, báo thử lại, không Pending, không vào queue, không tiêu quota đổi nickname.
- **Comment/Reply AI timeout/5xx/down thì ngược lại — fail-safe:** record vẫn được tạo ở `Đang chờ duyệt`, không bao giờ tự public. Đây là ngoại lệ có chủ đích so với nickname vì comment là nội dung user vừa soạn.
- Rate limit đếm **attempt** (5/rolling 60 giây/user, dùng chung Comment+Reply): validation fail không tính, nhưng mọi lần đã gọi AI đều tính — kể cả khi bị chặn ở mức Nặng.
- Pending comment hiển thị inline chỉ tác giả với nhãn `Đang chờ duyệt`, không tính public count.
- SmartTV không có nút Viết bình luận; hiển thị hướng dẫn + QR để dùng smartphone.
