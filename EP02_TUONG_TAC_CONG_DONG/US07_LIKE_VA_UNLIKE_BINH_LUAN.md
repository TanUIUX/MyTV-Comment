# US07 — Like và Unlike bình luận

> Thuộc EP02 — Tương tác cộng đồng  
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md) · [User Flow →](US07_USER_FLOW.md)

### User Story

**Là user đăng nhập**, tôi muốn Like/Unlike comment/reply để thể hiện phản ứng nhanh.

### Quy tắc đã chốt

- Guest đọc được count nhưng phải login để Like; login xong quay lại target và **không auto-Like**.
- Mỗi target chỉ có state `Đã Like / Chưa Like`; SmartTV chữ “Dislike” nếu có chỉ mang nghĩa Unlike, **không có reaction Dislike riêng**.
- Hiển thị `👍 {count}`.
- Optimistic UI đổi state/count ngay.
- Sync lỗi lần đầu → **retry tự động đúng 1 lần**. Retry vẫn lỗi → dừng retry, không ghi nhận Like, revert state/count về server và báo lỗi.
- Server là source of truth; Like thất bại không tính Net Like/ranking/KPI.
- SmartTV cho Like/Unlike bằng remote nếu đã login.

## Acceptance Criteria trọng yếu

1. Like/Unlike idempotent theo target/account.
2. Target non-public → action unavailable.
3. Account Lock của liker tạm loại Like khỏi public Net Like/ranking nhưng giữ record.
4. A11y: `aria-pressed` và announce reconcile/revert.
