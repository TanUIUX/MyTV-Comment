# US04 — Đăng bình luận

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá  
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md) · [User Flow →](US04_USER_FLOW.md)

### User Story

**Là người dùng đã đăng nhập**, tôi muốn đăng bình luận vào phim/tập hiện tại để tham gia thảo luận.

### Quy tắc cốt lõi đã chốt

- Phim lẻ comment cấp phim; phim bộ comment **chỉ cấp tập hiện tại**, không có Series comment scope phía user.
- Phone composer dùng bottom sheet; Web composer inline; SmartTV không tạo comment.
- Comment/Reply 1–1000 grapheme; emoji-only valid; whitespace-only invalid; URL allowlist `mytv.com.vn` + true subdomain.
- Rate limit Comment+Reply 5 nội dung/phút/user.
- Draft được giữ khi đóng composer nhưng **chỉ trong phiên hiện tại**; đóng app/reload/kết thúc phiên thì mất; submit thành công clear.
- Spoiler và timestamp là metadata có thể gắn khi viết.
- Pending hiển thị inline chỉ tác giả với `Đang chờ duyệt`, không tính public count.
- AI Heavy giữ nguyên composer/draft để user sửa.

### Nickname

- Có thể đổi ở Profile/Cài đặt hoặc shortcut `Đổi nickname` trong composer.
- Unique case-insensitive, 3–30 ký tự; quota 1 lần **đổi thành công**/24h/account.
- Global nickname AI policy: Safe/Light → save ngay; Medium/Heavy → block ngay; **không có Pending**.
- AI timeout/5xx/down → không đổi nickname, giữ old/fallback, báo thử lại, không queue, không tiêu quota.

### SmartTV

Không hiển thị nút `Viết bình luận`; hiển thị hướng dẫn + QR để user tiếp tục trên smartphone.

## Acceptance Criteria trọng yếu

1. Guest chọn Viết bình luận → login → quay lại đúng content, không auto-submit.
2. Scope Đóng/Comment Lock chặn composer theo rule tương ứng.
3. Submit valid → moderation Mode1/Mode2; Heavy block, Pending author-only, Public insert list.
4. Network/validation/AI-block giữ draft để sửa.
5. Submit success clear draft.
