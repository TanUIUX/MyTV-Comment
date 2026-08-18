# US18 — Chia sẻ bình luận và cảnh phim

> Thuộc EP05 — Tăng trưởng và phân tích
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là người dùng đã đăng nhập**, tôi muốn chia sẻ ngữ cảnh thảo luận/cảnh phim ra mạng xã hội bằng liên kết quay lại MyTV, để giới thiệu nội dung và mời người khác tham gia.

### Giá trị

- Tạo nguồn truy cập quay lại MyTV từ bên ngoài.
- Tăng khả năng lan truyền của phim và thảo luận cộng đồng.
- Kết nối hoạt động bình luận với chiến dịch mạng xã hội.

### Ưu tiên

**Could**

### Điều kiện tiên quyết

- Bình luận đang Hiển thị và được phép chia sẻ.
- Người dùng có quyền xem/chia sẻ nội dung nguồn.
- Nền tảng đích hoặc share sheet được hỗ trợ.

### Acceptance Criteria

1. Người dùng đã đăng nhập có thể chọn Chia sẻ trên một bình luận đang hiển thị.
2. Nội dung text được gửi ra ngoài **không chứa nguyên văn nội dung bình luận**. Phần chia sẻ sử dụng thông điệp/CTA do MyTV cấu hình, ví dụ “Đăng nhập MyTV để xem”, kèm link/deep link quay về MyTV.
3. Nếu bình luận có cảnh phim đủ quyền chia sẻ, người dùng có thể chia sẻ cảnh đó theo chính sách bản quyền/DRM; phần text đi kèm vẫn không sao chép nguyên văn comment.
4. Link chia sẻ quay lại đúng phim/tập trên MyTV và ưu tiên mở đúng ngữ cảnh bình luận/thread khi còn hợp lệ.
5. Nếu người nhận chưa đăng nhập và cần xem nội dung/thảo luận chi tiết, hệ thống thực hiện luồng đăng nhập rồi đưa người dùng trở lại đúng ngữ cảnh nếu khả thi.
6. Hệ thống hỗ trợ các kênh mục tiêu như Facebook, TikTok, Zalo hoặc share sheet của thiết bị theo khả năng tích hợp được xác minh.
7. Nội dung Spoiler không được lộ trong preview chia sẻ; do không đưa nguyên văn comment ra ngoài, preview mặc định chỉ dùng metadata an toàn của MyTV/phim.
8. Bình luận bị ẩn/xóa sau khi chia sẻ không còn được hiển thị qua deep link; người nhận có thể được chuyển tới phim/tập nếu còn quyền.
9. Cảnh phim chỉ được chia sẻ khi chính sách bản quyền/DRM cho phép.
10. Link/metadata chia sẻ không làm lộ số điện thoại, nickname nhạy cảm, nội dung comment, token hoặc dữ liệu nội bộ.
11. Hệ thống ghi nhận sự kiện chia sẻ, kênh đích và lượt mở link để phục vụ phân tích trong phạm vi cho phép.

### Quy tắc nghiệp vụ

- Mọi nội dung chia sẻ phải gắn đường dẫn quay về MyTV.
- Không đưa nguyên văn nội dung bình luận vào text/preview chia sẻ ra mạng xã hội.
- CTA mặc định hướng người nhận quay lại MyTV, ví dụ “Đăng nhập MyTV để xem”.
- Không chia sẻ nội dung đang Chờ duyệt, Từ chối, Ẩn hoặc Xóa.
- Không mặc định coi mọi cảnh phim đều được phép chia sẻ ra ngoài.
- Việc tích hợp trực tiếp từng mạng xã hội phụ thuộc API/chính sách nền tảng; share sheet là phương án fallback.

### Phụ thuộc

- US06 — Bình luận kèm cảnh phim.
- Dịch vụ deep link và attribution.
- DRM/quản lý quyền nội dung.
- US19 — Thống kê.

### Rủi ro và điểm cần PO chốt

- Danh sách kênh MVP và mức tích hợp từng kênh.
- Mẫu CTA/preview chính thức và watermark cho cảnh phim.
- Quyền chia sẻ theo từng phim/nhà cung cấp.
- Thời gian hiệu lực của deep link và hành vi với người chưa cài ứng dụng.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra chia sẻ chỉ áp dụng cho nội dung đủ điều kiện, không đưa nguyên văn comment ra ngoài, deep link mở đúng ngữ cảnh, preview không lộ Spoiler/PII, media tuân quyền và event attribution ghi nhận đúng.

### Rủi ro chính

- Nội dung comment bị sao chép ra text/preview trái rule.
- Chia sẻ comment Chờ duyệt/Ẩn/Xóa hoặc cảnh phim hết quyền.
- Deep link mở sai tập/thread hoặc lộ nội dung sau khi comment bị gỡ.
- Preview Spoiler/PII và event share bị ghi trùng.

### Dữ liệu kiểm thử

Comment công khai thường/Spoiler có và không có frame/clip; comment Chờ duyệt/Ẩn/Xóa; người nhận có/không đăng nhập, có/không quyền xem; các kênh Facebook/TikTok/Zalo/share sheet.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US18-001 | Functional | Comment công khai không Spoiler | Chọn Chia sẻ và hoàn tất share sheet | Nội dung share có CTA + link MyTV; không chứa nguyên văn comment. |
| TC-US18-002 | Media/permission | Comment có frame/clip và viewer có quyền | Chọn chia sẻ cảnh phim | Cảnh chỉ xuất hiện khi quyền/DRM cho phép; text kèm không sao chép comment. |
| TC-US18-003 | Deep link | Comment thuộc E1 có thread | Mở link trên app/web | Mở đúng phim/tập/thread nếu còn hợp lệ; nếu không thể mở comment thì fallback đúng trang phim/tập. |
| TC-US18-004 | Authentication | Người nhận chưa đăng nhập | Mở link cần xem ngữ cảnh thảo luận | Hiển thị CTA/luồng đăng nhập; sau đăng nhập quay lại đúng phim/tập/thread nếu hệ thống hỗ trợ giữ context. |
| TC-US18-005 | Channel | Có cấu hình kênh mục tiêu và share sheet | Thử từng kênh được hỗ trợ | Kênh khả dụng mở đúng; kênh lỗi có fallback và không làm mất link. |
| TC-US18-006 | Spoiler/privacy | Comment Spoiler hoặc chứa PII trong text | Tạo preview/link và xem bằng người nhận | Preview không chứa comment/Spoiler/PII; chỉ dùng metadata an toàn theo cấu hình MyTV. |
| TC-US18-007 | Invalid state | Comment Chờ duyệt/Từ chối/Ẩn/Xóa | Thử chia sẻ bằng UI và gọi API | Nút bị ẩn/chặn; API từ chối; không tạo share event hợp lệ. |
| TC-US18-008 | Post-share removal | Đã tạo link cho C1, sau đó Admin Ẩn/Xóa C1 | Mở link cũ | Không hiển thị C1 đã gỡ; có thể fallback về phim nếu người nhận còn quyền. |
| TC-US18-009 | Copyright | Cảnh phim hết quyền/DRM không cho phép | Thử share cảnh | Hệ thống chặn cảnh, không phát/đóng gói media; vẫn có thể chia sẻ CTA/link phim nếu policy cho phép. |
| TC-US18-010 | Metadata | Comment có nickname/số thuê bao đã che | Kiểm tra title/description/image và query link | Metadata không chứa text comment, PII, token hoặc dữ liệu nội bộ; deep link không mở quyền vượt cấp. |
| TC-US18-011 | Tracking | Có share thành công và mở link nhiều lần | Kiểm tra event share/channel/open attribution | Ghi nhận đúng kênh/lượt mở; retry không làm nhân đôi sự kiện ngoài định nghĩa. |
| TC-US18-012 | Authorization | User không có quyền xem nội dung nguồn | Thử share comment/cảnh qua API | Bị từ chối hoặc chỉ cho share CTA/link được phép; không lộ media nguồn hay comment. |

### Điểm cần PO chốt

- Danh sách kênh MVP, CTA/preview/watermark, thời hạn deep link và hành vi người chưa cài app.
