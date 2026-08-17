# US12 — Quản lý trạng thái và phạm vi hiển thị bình luận

> Thuộc EP03 — An toàn và kiểm duyệt
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là Admin vận hành**, tôi muốn hệ thống áp dụng trạng thái và phạm vi hiển thị phù hợp theo cấu hình của từng phim, để kiểm soát chính xác ai được xem bình luận ở từng giai đoạn kiểm duyệt.

### Giá trị

- Ngăn nội dung chưa được duyệt xuất hiện sai đối tượng.
- Cho phép áp dụng cơ chế linh hoạt theo rủi ro từng phim.
- Đảm bảo trải nghiệm nhất quán giữa ứng dụng và CMS.

### Ưu tiên

**Must**

### Các trạng thái nghiệp vụ tối thiểu

| Trạng thái | Hiển thị với tác giả | Hiển thị với cộng đồng | Ý nghĩa |
|---|---:|---:|---|
| Chờ duyệt | Có | Không | Đang chờ AI/Admin quyết định |
| Hiển thị | Có | Có | Được phép công khai |
| Từ chối | Có theo chính sách UX | Không | Không được duyệt |
| Ẩn | Có/Không theo chính sách UX | Không | Admin tạm ẩn khỏi cộng đồng |
| Xóa mềm | Không | Không | Đã xóa, giữ dữ liệu phục vụ audit |

### Acceptance Criteria

1. Bình luận mới ở trạng thái Chờ duyệt chỉ hiển thị với tác giả.
2. Người khác không nhận được bình luận Chờ duyệt qua giao diện, API công khai, thông báo hoặc số đếm công khai.
3. Khi người dùng sửa bình luận đang Hiển thị, phiên bản công khai cũ tiếp tục hiển thị trong lúc phiên bản mới Chờ duyệt.
4. Khi phiên bản mới được duyệt, hệ thống chuyển phiên bản đó thành nội dung công khai hiện hành.
5. Khi phiên bản mới bị từ chối, phiên bản công khai cũ không bị ảnh hưởng.
6. Mỗi phim có thể được cấu hình ở một trong ba mức: Chế độ 1, Chế độ 2 hoặc Đóng bình luận.
7. Khi phim chuyển sang Đóng bình luận, toàn bộ khu vực Bình luận được ẩn với người xem.
8. Khi Đóng bình luận, hệ thống không chấp nhận bình luận, reply, Like, Mention, Report hoặc đánh giá mới qua giao diện/API theo phạm vi cấu hình.
9. Việc đóng bình luận không xóa dữ liệu lịch sử.
10. Khi mở lại, dữ liệu công khai trước đó được hiển thị lại theo cấu hình và quyền hiện hành, trừ nội dung đã bị xử lý.
11. Admin có thể cấu hình thời điểm hoặc khoảng thời gian chuyển sang trạng thái “Chờ duyệt — chỉ tác giả thấy” nếu nghiệp vụ yêu cầu sau X giờ.
12. Thay đổi cấu hình chỉ áp dụng từ thời điểm hiệu lực; cách xử lý bình luận đang trong luồng phải được xác định nhất quán.
13. Tất cả chuyển đổi trạng thái và thay đổi cấu hình được lưu audit.

### Quy tắc nghiệp vụ

- Chế độ mặc định: AI tiền kiểm, nội dung an toàn hiển thị ngay, Admin hậu kiểm.
- Chế độ đặc biệt: AI kiểm tra, Admin duyệt trước khi công khai.
- Mức 3: đóng hoàn toàn khu vực bình luận.
- Không xóa dữ liệu chỉ vì chuyển chế độ hoặc đóng bình luận.

### Phụ thuộc

- US11 — AI kiểm duyệt theo hai chế độ.
- US15 — Cấu hình theo phim trên CMS.
- US16 — Audit log.

### Điểm cần PO chốt

- Khi đóng bình luận có tiếp tục hiển thị điểm đánh giá hay không; quyết định hiện tại “ẩn toàn bộ khu vực” có thể bao gồm cả rating nếu rating nằm trong khu vực này.
- Xử lý nội dung đang Chờ duyệt khi chuyển từ Chế độ 2 sang Chế độ 1.
- Ý nghĩa chính xác của cấu hình “sau X giờ”: tính từ giờ phát hành, giờ phát sóng hay một mốc Admin chọn.
- Có hiển thị lý do từ chối/ẩn cho tác giả hay không.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra ma trận hiển thị theo trạng thái, version công khai/chờ duyệt, cấu hình Mở/Chế độ 1/Chế độ 2/Đóng và hiệu lực chuyển trạng thái.

### Rủi ro chính

- Nội dung Chờ duyệt lọt qua API, count, notification hoặc deep link.
- Bản sửa chờ duyệt ghi đè bản cũ.
- Đóng bình luận chỉ khóa UI nhưng vẫn nhận request trực tiếp; mở lại làm mất lịch sử.

### Dữ liệu kiểm thử

Comment công khai C1, bản sửa V2 Chờ duyệt, nội dung Từ chối/Ẩn/Xóa mềm; phim ở Chế độ 1, Chế độ 2, Đóng; cấu hình có mốc X giờ.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US12-001 | Visibility | Comment mới Chờ duyệt; tác giả U1, người xem U2 | U1 và U2 mở UI/API công khai | U1 thấy comment + trạng thái; U2 không thấy qua UI/API/count/notification. |
| TC-US12-002 | Versioning | C1 đang Hiển thị; V2 Chờ duyệt | U2 mở trong lúc chờ, sau đó Admin duyệt V2 | Trước duyệt U2 thấy C1; sau duyệt V2 là bản công khai hiện hành. |
| TC-US12-003 | Rejection | C1 công khai; V2 bị từ chối | Admin từ chối V2 rồi mở lại C1 | C1 không bị ảnh hưởng; V2 không hiển thị cộng đồng theo UX chính sách. |
| TC-US12-004 | Mode config | Phim cấu hình Chế độ 1 | Gửi nội dung an toàn/nghi ngờ | An toàn Hiển thị; nghi ngờ Chờ duyệt đúng US11. |
| TC-US12-005 | Mode config | Phim cấu hình Chế độ 2 | Gửi nội dung an toàn về kỹ thuật | Nội dung vẫn Chờ duyệt cho tới khi Admin duyệt. |
| TC-US12-006 | Closed state/UI | Phim chuyển Đóng | Mở trang chi tiết, app và API đọc | Toàn bộ khu vực bình luận bị ẩn; không trả dữ liệu công khai ngoài chính sách. |
| TC-US12-007 | Closed state/API | Phim Đóng | Gửi comment, reply, Like, Mention, Report, Rating qua UI và API | Tất cả thao tác mới bị chặn; không tạo record hoặc event bất hợp lệ. |
| TC-US12-008 | Reopen/history | Phim Đóng có dữ liệu cũ | Mở lại phim | Dữ liệu công khai đủ điều kiện hiển thị lại; nội dung đã xử lý vẫn bị loại; lịch sử không bị xóa. |
| TC-US12-009 | Schedule | Có cấu hình “sau X giờ” | Kiểm tra trước, đúng và sau thời điểm hiệu lực | Chuyển state đúng mốc, chỉ áp dụng từ thời điểm hiệu lực và có timezone nhất quán. |
| TC-US12-010 | In-flight | Có comment đang AI/Admin xử lý khi đổi chế độ | Đổi Chế độ 1↔2/Đóng và hoàn tất xử lý | Request đang xử lý theo quy tắc đã chốt; không tạo state trung gian mâu thuẫn. |
| TC-US12-011 | Audit | Admin thay đổi state/cấu hình | Tra cứu audit | Ghi actor, thời gian, trước/sau, lý do và cấu hình áp dụng. |
| TC-US12-012 | Authorization | Admin/user khác quyền | Gọi API đổi cấu hình hoặc state | Chỉ vai trò được cấp quyền thực hiện; request trái quyền bị từ chối. |

### Điểm cần PO chốt

- Rating có bị ẩn cùng khu vực khi Đóng hay không.
- Cách xử lý queue đang Chờ duyệt khi đổi chế độ và mốc tính “sau X giờ”.
