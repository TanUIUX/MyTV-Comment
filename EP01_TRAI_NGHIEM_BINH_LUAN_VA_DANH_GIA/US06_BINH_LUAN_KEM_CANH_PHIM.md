# US06 — Bình luận kèm cảnh phim

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người dùng đang xem phim**, tôi muốn đính kèm một frame hoặc đoạn video ngắn từ phim vào bình luận, để diễn đạt rõ cảnh mình đang thảo luận mà không tải media cá nhân lên.

### Giá trị

- Làm cuộc thảo luận trực quan và gắn chặt với nội dung MyTV.
- Hạn chế rủi ro từ việc người dùng tải file cá nhân.
- Tạo nội dung có khả năng chia sẻ và lan truyền.

### Ưu tiên

**Could**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập và đang phát nội dung được phép sử dụng tính năng.
- Player cung cấp content ID, episode ID và timestamp hiện tại.
- Quyền khai thác nội dung cho phép tạo lại frame/clip.

### Acceptance Criteria

1. Người dùng có thể chọn lấy một frame tại thời điểm đang xem.
2. Người dùng có thể chọn đoạn video ngắn với thời lượng tối đa 5 giây theo giới hạn hệ thống.
3. Hệ thống không hiển thị chức năng tải ảnh/video từ thiết bị cá nhân.
4. Bình luận chỉ lưu content ID, episode ID, timestamp, loại media và thông tin thời lượng cần thiết; không tạo file media riêng theo yêu cầu sản phẩm.
5. Khi người khác xem bình luận, hệ thống dựng lại frame/clip từ nội dung và timestamp tương ứng.
6. Media chỉ hiển thị cho người dùng có quyền xem nội dung nguồn.
7. Khi nội dung nguồn không còn quyền phát, không tồn tại hoặc timestamp không hợp lệ, hệ thống hiển thị trạng thái thay thế phù hợp và không làm hỏng bình luận văn bản.
8. Người dùng vẫn có thể đánh dấu bình luận chứa cảnh phim là Spoiler.
9. Frame/clip phải đi qua quy tắc kiểm duyệt và bản quyền áp dụng cho bình luận.
10. Hành vi phải hoạt động nhất quán trên các thiết bị được hỗ trợ; thiết bị không hỗ trợ cần có phương án fallback.

### Quy tắc nghiệp vụ

- Không cho phép media cá nhân.
- Clip không dài quá 5 giây.
- Không cho phép lấy timestamp ngoài nội dung/tập đang bình luận.
- Việc chia sẻ cảnh phim ra ngoài MyTV thuộc US18 và cần kiểm tra quyền riêng.

### Phụ thuộc

- Player MyTV và dịch vụ nội dung.
- DRM/quản lý quyền nội dung.
- US04 — Đăng bình luận.
- US18 — Chia sẻ bình luận và cảnh phim.

### Rủi ro và điểm cần PO chốt

- Khả năng dựng media theo timestamp trên từng nền tảng.
- Hành vi khi nội dung thay source, bị cắt dựng hoặc hết hạn quyền phát.
- Danh sách nội dung không được phép tạo frame/clip.
- Có cần watermark MyTV hay không.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra frame/clip được dựng từ timestamp hợp lệ của nội dung MyTV, không cho tải media cá nhân, tuân thủ quyền phát/kiểm duyệt và không làm hỏng phần text.

### Rủi ro chính

- Timestamp hoặc episode sai làm hiển thị cảnh không liên quan.
- Clip vượt 5 giây, tạo file media riêng hoặc lộ nội dung khi hết quyền.
- Thiết bị không hỗ trợ/DRM lỗi khiến cả comment văn bản không hiển thị.

### Dữ liệu kiểm thử

Nội dung có quyền phát, timestamp đầu/giữa/cuối, clip 0/5/>5 giây, episode khác, nội dung hết hạn, timestamp không hợp lệ và thiết bị có/không hỗ trợ renderer.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US06-001 | Functional | U1 đang xem nội dung có quyền | Chọn lấy frame tại timestamp hiện tại và gửi comment | Comment lưu đúng content ID, episode ID, timestamp và loại media; frame hiển thị đúng cảnh. |
| TC-US06-002 | Boundary | Timestamp ở đầu, giữa và sát cuối tập | Tạo frame/clip tại từng mốc | Renderer lấy đúng vị trí; không lệch tập hoặc vượt phạm vi nội dung. |
| TC-US06-003 | Boundary | Clip dài đúng 5 giây và ngắn hơn 5 giây | Tạo và gửi clip | Clip hợp lệ được chấp nhận, duration lưu đúng và không vượt giới hạn. |
| TC-US06-004 | Negative | Clip >5 giây hoặc duration âm/không hợp lệ | Gửi qua UI và API | Hệ thống chặn request, không tạo media/comment không hợp lệ. |
| TC-US06-005 | Security | Có thử upload ảnh/video từ thiết bị | Kiểm tra UI và gọi endpoint upload trực tiếp | Không có luồng upload media cá nhân; endpoint bị từ chối. |
| TC-US06-006 | Data integrity | Tạo nhiều comment kèm frame/clip | Kiểm tra database/object storage | Chỉ lưu metadata cần thiết; không tạo file media cá nhân riêng ngoài thiết kế. |
| TC-US06-007 | Permission | Viewer không có quyền xem content nguồn | Mở comment chứa frame/clip | Media không phát/lộ; text comment vẫn hiển thị theo quyền comment và có trạng thái thay thế phù hợp. |
| TC-US06-008 | Fallback | Nội dung hết hạn, source không tồn tại hoặc timestamp sai | Mở comment đã lưu | Hiển thị placeholder/lỗi thân thiện; không crash và không làm mất text/thread. |
| TC-US06-009 | Spoiler/moderation | Frame/clip được đánh dấu Spoiler hoặc chứa nội dung rủi ro | Gửi, kiểm duyệt và mở bằng tài khoản khác | Media bị che/đi qua cùng chính sách kiểm duyệt như text; không được bypass bằng thumbnail. |
| TC-US06-010 | Compatibility | Web/mobile và thiết bị không hỗ trợ renderer | Mở cùng comment trên các nền tảng | Hành vi nhất quán; nền tảng không hỗ trợ có fallback theo thiết kế, không mất ngữ cảnh. |

### Điểm cần PO chốt trước khi chạy E2E bản quyền

- Danh sách nội dung không cho phép frame/clip, watermark và fallback cụ thể.
- Cách xử lý khi source video thay đổi sau khi comment đã lưu.
