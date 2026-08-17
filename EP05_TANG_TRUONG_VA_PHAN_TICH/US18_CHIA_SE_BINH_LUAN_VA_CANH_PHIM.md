# US18 — Chia sẻ bình luận và cảnh phim

> Thuộc EP05 — Tăng trưởng và phân tích
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người dùng đã đăng nhập**, tôi muốn chia sẻ một bình luận hoặc cảnh phim đang thảo luận ra mạng xã hội với liên kết quay lại MyTV, để giới thiệu nội dung và mời người khác tham gia.

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

1. Người dùng có thể chọn Chia sẻ trên một bình luận đang hiển thị.
2. Người dùng có thể chọn chia sẻ nội dung bình luận hoặc cảnh phim gắn với bình luận nếu cảnh đó đủ quyền.
3. Nội dung chia sẻ có liên kết quay lại đúng phim/tập trên MyTV.
4. Khi có thể, deep link mở đúng ngữ cảnh bình luận/thread; nếu không, mở trang phim/tập phù hợp.
5. Hệ thống hỗ trợ các kênh mục tiêu như Facebook, TikTok, Zalo hoặc share sheet của thiết bị theo khả năng tích hợp được xác minh.
6. Nội dung Spoiler không được hiển thị trực tiếp trong preview chia sẻ nếu chưa có cơ chế che phù hợp.
7. Bình luận bị ẩn/xóa sau khi chia sẻ không còn được hiển thị qua deep link; người nhận vẫn có thể được chuyển tới phim nếu còn quyền.
8. Cảnh phim chỉ được chia sẻ khi chính sách bản quyền/DRM cho phép.
9. Link chia sẻ có metadata phù hợp và không làm lộ thông tin cá nhân nhạy cảm.
10. Hệ thống ghi nhận sự kiện chia sẻ, kênh đích và lượt mở link để phục vụ phân tích trong phạm vi cho phép.

### Quy tắc nghiệp vụ

- Mọi nội dung chia sẻ phải gắn đường dẫn quay về MyTV.
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
- Mẫu nội dung/preview và watermark.
- Quyền chia sẻ theo từng phim/nhà cung cấp.
- Thời gian hiệu lực của deep link và hành vi với người chưa cài ứng dụng.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra chia sẻ chỉ áp dụng cho nội dung đủ điều kiện, deep link mở đúng ngữ cảnh, preview không lộ Spoiler/PII, media tuân quyền và event attribution ghi nhận đúng.

### Rủi ro chính

- Chia sẻ comment Chờ duyệt/Ẩn/Xóa hoặc cảnh phim hết quyền.
- Deep link mở sai tập/thread hoặc lộ nội dung sau khi comment bị gỡ.
- Preview Spoiler/PII và event share bị ghi trùng.

### Dữ liệu kiểm thử

Comment công khai thường/Spoiler có và không có frame/clip; comment Chờ duyệt/Ẩn/Xóa; người nhận có/không quyền xem; các kênh Facebook/TikTok/Zalo/share sheet.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US18-001 | Functional | Comment công khai không Spoiler | Chọn Chia sẻ và hoàn tất share sheet | Có thể chia sẻ; nội dung/metadata và link quay về MyTV đúng. |
| TC-US18-002 | Media/permission | Comment có frame/clip và viewer có quyền | Chọn chia sẻ text rồi chia sẻ cảnh phim | Cho phép đúng loại; cảnh chỉ xuất hiện khi quyền/DRM cho phép. |
| TC-US18-003 | Deep link | Comment thuộc E1 có thread | Mở link trên app/web đã cài và chưa cài app | Mở đúng phim/tập/thread; nếu không thể mở comment thì fallback đúng trang phim/tập. |
| TC-US18-004 | Channel | Có cấu hình kênh mục tiêu và share sheet | Thử từng kênh được hỗ trợ | Kênh khả dụng mở đúng; kênh lỗi có fallback và không làm mất link. |
| TC-US18-005 | Spoiler/privacy | Comment Spoiler hoặc có PII trong text | Tạo preview/link và xem bằng người nhận | Preview không lộ Spoiler/PII ngoài policy; có cảnh báo/che phù hợp. |
| TC-US18-006 | Invalid state | Comment Chờ duyệt/Từ chối/Ẩn/Xóa | Thử chia sẻ bằng UI và gọi API | Nút bị ẩn/chặn; API từ chối; không tạo share event hợp lệ. |
| TC-US18-007 | Post-share removal | Đã tạo link cho C1, sau đó Admin Ẩn/Xóa C1 | Mở link cũ | Không hiển thị C1 đã gỡ; có thể fallback về phim nếu người nhận còn quyền. |
| TC-US18-008 | Copyright | Cảnh phim hết quyền/DRM không cho phép | Thử share cảnh | Hệ thống chặn cảnh, không phát/đóng gói media; text share xử lý theo policy. |
| TC-US18-009 | Metadata | Comment có nickname/số thuê bao đã che | Kiểm tra og:title, description, image và query link | Metadata không lộ PII, token hoặc dữ liệu nội bộ; deep link không mở quyền vượt cấp. |
| TC-US18-010 | Tracking | Có share thành công và mở link nhiều lần | Kiểm tra event share/channel/open attribution | Ghi nhận đúng kênh/lượt mở; retry không làm nhân đôi sự kiện ngoài định nghĩa. |
| TC-US18-011 | Authorization | User không có quyền xem nội dung nguồn | Thử share comment/cảnh qua API | Bị từ chối hoặc chỉ cho share phần được phép; không lộ media nguồn. |

### Điểm cần PO chốt

- Danh sách kênh MVP, mẫu preview/watermark, thời hạn deep link và hành vi người chưa cài app.
