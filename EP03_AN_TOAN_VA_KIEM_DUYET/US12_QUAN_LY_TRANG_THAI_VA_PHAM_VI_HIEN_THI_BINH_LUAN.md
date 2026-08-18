# US12 — Quản lý trạng thái và phạm vi hiển thị bình luận

> Thuộc EP03 — An toàn và kiểm duyệt
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là Admin vận hành**, tôi muốn hệ thống áp dụng trạng thái và phạm vi hiển thị theo cấu hình từng phim/tập, để kiểm soát chính xác ai được xem nội dung ở từng giai đoạn moderation.

### Ưu tiên

**Must**

### Trạng thái nghiệp vụ

| Trạng thái | Tác giả | Cộng đồng | Ý nghĩa |
|---|---:|---:|---|
| Chờ duyệt | Có | Không | Chờ Admin quyết định |
| Hiển thị | Có | Có | Public |
| Từ chối | Có + lý do | Không | Không được duyệt |
| Ẩn | Có + lý do | Không | Admin ẩn khỏi cộng đồng |
| Xóa mềm | Không trong public UI | Không | Giữ dữ liệu theo retention |

### Acceptance Criteria

1. Chờ duyệt chỉ hiển thị với tác giả; không xuất hiện ở public API/count/notification cho người khác.
2. Bản sửa Chờ duyệt không ghi đè version public cũ; nếu bị Từ chối/chặn, version cũ vẫn Hiển thị.
3. Phim/tập có ba trạng thái vận hành: Chế độ 1, Chế độ 2, Đóng bình luận.
4. Khi Đóng, **toàn bộ khu vực Bình luận bị ẩn**, gồm danh sách, ô tương tác và rating nằm trong khu vực; UI/API chặn **Comment, Reply, Like, Mention, Report, Rating và Share mới**.
5. Đóng bình luận không xóa dữ liệu lịch sử và không đổi moderation state của comment/reply chỉ vì scope bị Đóng.
6. Khi mở lại, dữ liệu đủ điều kiện được hiển thị lại; nội dung đã Từ chối/Ẩn/Xóa vẫn không public.
7. Khi chuyển **Chế độ 2 → Chế độ 1**, comment đang Chờ duyệt mà **đã có AI result an toàn/Nhẹ trước đó** tự chuyển Hiển thị; các case Trung bình/không đủ điều kiện tiếp tục Chờ duyệt.
8. Khi chuyển **Chế độ 1 → Chế độ 2**, nội dung đã Hiển thị trước effective time vẫn giữ Hiển thị; Chế độ 2 áp dụng cho nội dung mới/version mới từ effective time.
9. Khi chuyển sang **Đóng** trong lúc còn queue, queue được giữ và Admin vẫn xử lý trên CMS; item được Duyệt trong lúc Đóng không public cho tới khi mở lại.
10. Deep link cũ từ Share/Notification tới target trong scope đang Đóng vẫn mở đúng phim/tập nhưng **không hiển thị comment/thread**, hiển thị **“Khu vực bình luận hiện không khả dụng”**; khi mở lại scope, link cũ hoạt động lại nếu target vẫn hợp lệ.
11. Rule “sau X giờ” hỗ trợ ba loại mốc và **Admin chọn loại mốc khi cấu hình**: giờ phát hành MyTV, giờ phát sóng thực tế, hoặc mốc Admin nhập thủ công.
12. Thay đổi mode/schedule chỉ áp dụng từ effective time và có timezone nhất quán.
13. Khi comment bị **Từ chối / Ẩn / Xóa**, tác giả thấy lý do cụ thể trong app và nhận notification nghiệp vụ bắt buộc.
14. Mọi transition/configuration được lưu audit.
15. Trong thời gian scope Đóng, việc user chỉ truy cập trang phim/tập **không tạo active day badge** vì khu vực Bình luận không thể được mở/đọc theo định nghĩa US17.

### Quy tắc nghiệp vụ

- Mode1: Nhẹ Hiển thị, Trung bình queue, Nặng chặn theo US11.
- Mode2: Nhẹ/Trung bình queue, Nặng chặn.
- Đóng là lớp visibility/interaction gate; không hủy moderation data và không đồng nghĩa moderation Ẩn/Xóa.
- Deep link trong scope Đóng dùng fallback theo scope, không dùng thông báo “Bình luận không còn khả dụng” của target bị moderation riêng.
- Notification reason cho Từ chối/Ẩn/Xóa không bị tắt bởi switch thông báo cộng đồng US09.
- Active day chỉ được ghi khi comment area thực sự khả dụng để user đăng nhập mở/đọc.

### Điểm cần PO chốt

- Không còn blocker PO về state transition/schedule trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US12-001 | Visibility | C1 Chờ duyệt | U1/U2 mở UI/API | U1 thấy status; U2 không thấy UI/API/count/notification. |
| TC-US12-002 | Versioning | C1 public, V2 pending | U2 mở trước/sau duyệt V2 | Trước duyệt thấy C1; sau duyệt thấy V2. |
| TC-US12-003 | Mode2→1 safe | Pending có AI Nhẹ/an toàn | Chuyển Mode2 → Mode1 | Item tự Hiển thị từ effective time, không cần Admin duyệt lại. |
| TC-US12-004 | Mode2→1 medium | Pending có AI Trung bình | Chuyển Mode2 → Mode1 | Item tiếp tục Chờ duyệt. |
| TC-US12-005 | Mode1→2 | Có C1 đã public | Chuyển Mode1 → Mode2 | C1 vẫn public; nội dung mới sau effective time vào queue. |
| TC-US12-006 | Close UI/API | Phim/tập Đóng | Mở UI và gọi Comment/Reply/Like/Mention/Report/Rating/Share APIs | Toàn bộ comment area/rating ẩn; mọi interaction mới trong scope bị chặn. |
| TC-US12-007 | Close with queue | Có pending item | Đóng rồi Admin duyệt item | Queue vẫn xử lý; item đã duyệt không public trong lúc Đóng. |
| TC-US12-008 | Reopen | Đang Đóng có data cũ/item đã duyệt | Mở lại | Data đủ điều kiện public lại; state xử lý riêng vẫn được tôn trọng. |
| TC-US12-009 | Closed deep link | Scope Đóng, C1 vẫn hợp lệ | Mở deep link Share/Notification tới C1 | Mở đúng phim/tập + “Khu vực bình luận hiện không khả dụng”; không hiển thị C1 và không đổi state C1. |
| TC-US12-010 | Reopen deep link | Scope mở lại, C1 vẫn hợp lệ | Mở lại deep link cũ | Link lại mở đúng C1/thread, không cần tạo link mới. |
| TC-US12-011 | X-hours | Cấu hình lần lượt 3 loại mốc | Kiểm tra trước/đúng/sau mốc | Transition đúng loại mốc Admin chọn và timezone. |
| TC-US12-012 | Reason notification | Admin Từ chối/Ẩn/Xóa C1 | U1 mở app/notification | U1 thấy lý do cụ thể và nhận notification bắt buộc. |
| TC-US12-013 | Active day closed | U1 đăng nhập; scope đang Đóng | U1 mở trang phim/tập nhiều lần | Không phát sinh active day vì comment area không khả dụng. |
| TC-US12-014 | Audit | Đổi mode/schedule/state | Tra audit | Có actor/time/before-after/effective config. |
