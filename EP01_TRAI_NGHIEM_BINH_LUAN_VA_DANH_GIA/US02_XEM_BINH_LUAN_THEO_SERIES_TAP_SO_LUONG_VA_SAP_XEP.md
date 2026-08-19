# US02 — Xem bình luận theo nội dung hiện tại, số lượng và sắp xếp

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá  
> Filename giữ tên legacy để không làm gãy liên kết repo.  
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md) · [User Flow →](US02_USER_FLOW.md)

### User Story

**Là người xem**, tôi muốn xem đúng bình luận của phim/tập hiện tại và sắp xếp theo nhu cầu, để không lẫn thảo luận giữa các nội dung.

### Acceptance Criteria

1. **Không có phạm vi bình luận `Toàn bộ phim/Series` phía người xem.**
2. Phim lẻ dùng scope phim; phim bộ dùng scope của tập hiện tại.
3. Khi đổi tập, hệ thống reload rating, public comment count, Pin và list của tập mới; sort reset về `Nổi bật`.
4. Tab hiển thị `Bình luận ({public_count})`; count gồm root + reply có effective visibility public.
5. Có 3 sort: `Nổi bật`, `Mới nhất`, `Nhiều lượt thích`; mặc định `Nổi bật`, chọn qua dropdown.
6. Tối đa 3 comment Ghim luôn hiển thị trên cùng **trong mọi sort**, ngoài quota root batch và không trùng trong list thường; hiển thị icon + chữ `Đã ghim`.
7. FeaturedScore của root không ghim: `0.5×ln(1+Like) + 0.3×ln(1+Reply) + 0.2×e^(-AgeHours/72)`; tie → comment mới hơn → `comment_id`.
8. `Mới nhất`: root mới → cũ. `Nhiều lượt thích`: public Net Like giảm dần, tie → mới hơn.
9. Initial load **10 root**; cuộn gần cuối lazy load **10 root/batch**, không trùng/bỏ sót.
10. Comment dài tối đa 3 dòng → `Xem thêm` / `Thu gọn`.
11. Root list không preload Reply; nếu có reply hiển thị `Xem {n} phản hồi` và đi US08.
12. Có comment mới khi đang đọc: không tự chèn; hiện `Có {n} bình luận mới`. Khi refresh, `Nổi bật`/`Nhiều lượt thích` luôn giữ đúng ranking; `Mới nhất` có thể đưa tới comment mới đầu tiên.
13. Rời tab rồi quay lại cùng phiên giữ scroll/sort/thread/expand state và refresh ngầm không reset context.
14. SmartTV cho đọc, đổi sort và lazy load bằng remote.

### Business rules

- Public count dùng Effective Visibility Resolver.
- Like trong FeaturedScore = public Net Like hiện tại.
- Like của account Account Lock giữ record nhưng tạm không tính public ranking; unlock tính lại nếu còn hợp lệ.
- Hard max Pin = 3.

## Test Cases

| ID | Loại | Bước | Expected |
|---|---|---|---|
| TC-US02-001 | Scope | Phim bộ đổi E1→E2 | Chỉ dữ liệu tập hiện tại; không có Series scope. |
| TC-US02-002 | Counter | Có public/pending/hidden | Count chỉ root+reply public. |
| TC-US02-003 | Sort | Mở và đổi 3 sort | Default Nổi bật; dropdown đủ 3 option. |
| TC-US02-004 | Pin | Có 3 pin và đổi sort | 3 pin luôn trên cùng, không lặp list thường. |
| TC-US02-005 | Lazy load | >20 root | 10 initial + tối đa 10/batch, không trùng/bỏ sót. |
| TC-US02-006 | New comments | Đang Nổi bật/Nhiều like | Refresh không phá ranking. |
| TC-US02-007 | Session context | Rời tab rồi quay lại | Giữ context, background refresh. |
