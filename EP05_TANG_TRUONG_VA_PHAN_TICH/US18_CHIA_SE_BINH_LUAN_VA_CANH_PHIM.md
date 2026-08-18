# US18 — Chia sẻ bình luận

> Thuộc EP05 — Tăng trưởng và phân tích
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là người dùng đã đăng nhập**, tôi muốn chia sẻ ngữ cảnh thảo luận ra ngoài bằng liên kết quay lại MyTV, để giới thiệu nội dung và mời người khác tham gia.

### Ưu tiên

**Could**

### Phạm vi MVP

- Chia sẻ comment/thread bằng **share sheet của thiết bị**.
- **Không chia sẻ frame/clip** trong phiên bản hiện tại; US06 chỉ hỗ trợ timestamp.

### Acceptance Criteria

1. User đã đăng nhập có thể Share một comment đang Hiển thị.
2. Share payload **không chứa nguyên văn text comment**, nickname/phone của tác giả hoặc Spoiler.
3. CTA chính thức: **“Xem nội dung này trên MyTV”**.
4. Preview gồm **poster/thumbnail phim + tên phim/tập + CTA + logo MyTV**; không chứa text comment/PII.
5. Deep link ưu tiên mở đúng phim/tập và đúng comment/thread khi target còn hợp lệ.
6. Nếu comment có timestamp, deep link **không tự seek player**; người nhận mở đúng comment và chủ động bấm timestamp nếu muốn.
7. Người nhận chưa đăng nhập vẫn được đọc comment public; login chỉ cần khi muốn tương tác.
8. Nếu người nhận chưa cài app, deep link mở **MyTV Web đúng phim/tập/thread** nếu còn hợp lệ; web có thể có CTA cài app.
9. Deep link **không hết hạn theo thời gian**; mỗi lần mở phải kiểm tra lại state/quyền hiện tại.
10. Nếu comment đã Ẩn/Xóa, link cũ mở đúng phim/tập và hiển thị **“Bình luận không còn khả dụng”**; không trả 404 và không lộ nội dung cũ.
11. MVP dùng share sheet; không cần tích hợp SDK/API riêng Facebook/Zalo/TikTok.
12. **Share event được tính khi user bấm Share và OS share sheet mở thành công**. Không cần chờ callback xác nhận user đã gửi sang ứng dụng đích.
13. Ghi nhận kênh đích khi nền tảng cung cấp và lượt mở link phục vụ US19; retry/lỗi kỹ thuật phải dedup để không tăng Share KPI sai.
14. Guest không được thực hiện action Share từ MyTV vì Share là interaction; phải đăng nhập trước khi tạo share event.

### Quy tắc nghiệp vụ

- Mọi share phải có link quay lại MyTV.
- Preview/text share chỉ dùng metadata an toàn của MyTV/phim.
- Share target Chờ duyệt/Từ chối/Ẩn/Xóa bị chặn tại thời điểm tạo share.
- Việc người nhận đọc public comment không yêu cầu login.
- **Mốc ghi nhận Share = share sheet mở thành công**; đóng/cancel share sheet sau đó không hoàn tác Share event đã ghi.
- Không có watermark/media/DRM sharing rule trong MVP vì frame/clip đã loại khỏi scope.

### Điểm cần PO chốt

- Không còn blocker PO cho share MVP hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US18-001 | Functional | C1 public | User login chọn Share | Share sheet mở với CTA + deep link, không có text comment; ghi nhận 1 Share event khi sheet mở thành công. |
| TC-US18-002 | Preview | C1 có Spoiler/PII trong text | Kiểm tra preview | Chỉ poster/tên phim-tập/CTA/logo; không lộ comment/Spoiler/PII. |
| TC-US18-003 | Deep link | C1 thuộc E1/thread T1 | Mở link | Mở đúng E1 và C1/T1 nếu còn hợp lệ. |
| TC-US18-004 | Timestamp | C1 có `18:00` | Mở share link | Mở đúng comment nhưng player không tự seek; user có thể tự bấm timestamp. |
| TC-US18-005 | Guest recipient | Người nhận logout | Mở link | Đọc được C1 public; muốn Like/Reply/etc thì yêu cầu login. |
| TC-US18-006 | Guest sender | Guest đang xem C1 | Chọn Share | Yêu cầu login; share sheet chưa mở và chưa tạo Share event. Sau login phải chủ động bấm Share lại theo US01. |
| TC-US18-007 | No app | Thiết bị chưa cài MyTV | Mở link | Mở MyTV Web đúng context, không mất deep-link target nếu hợp lệ. |
| TC-US18-008 | No expiry | Link cũ lâu ngày, target vẫn valid | Mở | Link vẫn hoạt động sau khi kiểm tra quyền/state hiện tại. |
| TC-US18-009 | Removed target | C1 đã Ẩn/Xóa | Mở link cũ | Mở phim/tập + “Bình luận không còn khả dụng”; không lộ C1. |
| TC-US18-010 | Invalid share state | C1 pending/rejected/hidden/deleted | Thử Share UI/API | Bị chặn; share sheet không mở hợp lệ và không tạo Share event. |
| TC-US18-011 | MVP channel | Có nhiều app share cài trên device | Share | Dùng OS share sheet; chưa cần direct integration SDK/API. |
| TC-US18-012 | Share metric | Share sheet mở thành công rồi user đóng/cancel không gửi | Kiểm tra event | Vẫn ghi nhận đúng 1 Share event vì mốc tính là sheet mở thành công. |
| TC-US18-013 | Dedup | Retry/open sheet lặp do lỗi kỹ thuật cùng request | Kiểm tra event | Không tạo duplicate ngoài định nghĩa dedup; KPI Share không tăng sai. |
