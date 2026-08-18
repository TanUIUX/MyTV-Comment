# US03 — Đánh giá series và tập phim

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là người dùng đã đăng nhập**, tôi muốn đánh giá series hoặc tập phim bằng thang điểm 5 sao và có thể thay đổi/xóa đánh giá, để thể hiện cảm nhận hiện tại của mình với cộng đồng.

### Ưu tiên

**Must**

### Acceptance Criteria

1. Đầu khu vực Bình luận hiển thị thang điểm 5 sao, điểm trung bình và tổng số lượt đánh giá.
2. Người chưa đăng nhập xem được điểm trung bình/tổng lượt nhưng phải đăng nhập để đánh giá.
3. Mỗi tài khoản chỉ có một rating hiện hành trên mỗi series và một rating hiện hành trên mỗi tập; hai scope độc lập.
4. Người dùng có thể đổi rating 1–5 sao; đổi rating không làm tăng tổng lượt đánh giá.
5. Người dùng có thể xóa rating của chính mình; khi xóa, tổng lượt và điểm trung bình được tính lại.
6. Điểm trung bình hiển thị **1 chữ số thập phân và luôn làm tròn lên** đến 0.1 gần nhất, ví dụ `4.21 → 4.3`, `4.20 → 4.2`.
7. Rating của tài khoản chỉ bị loại khỏi điểm trung bình khi tài khoản bị **khóa toàn bộ tài khoản MyTV**; khóa bình luận không loại rating.
8. Khi tài khoản được mở khóa lại, rating cũ tự động được tính lại nếu bản ghi rating vẫn tồn tại.
9. Nếu gửi/đổi/xóa rating thất bại, hệ thống giữ trạng thái hợp lệ trước đó và không tạo dữ liệu nửa chừng.

### Quy tắc nghiệp vụ

- Giá trị hợp lệ: số nguyên 1–5 sao.
- Một tài khoản chỉ đóng góp tối đa một rating hiện hành cho một scope.
- Việc khóa/mở khóa tài khoản ảnh hưởng khả năng tính rating nhưng không tự xóa bản ghi rating.
- Rating nằm trong khu vực Bình luận và bị ẩn khi phim/tập Đóng bình luận theo US12.

### Điểm cần PO chốt

- Không còn blocker PO cho rating trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US03-001 | Functional | Nội dung có rating | Mở bằng phiên khách | Thấy 5 sao, điểm trung bình, tổng lượt; chọn sao thì yêu cầu login. |
| TC-US03-002 | Boundary | U1 đăng nhập | Gửi 1 sao và 5 sao | Hai giá trị biên hợp lệ; 0/6/thập phân bị từ chối. |
| TC-US03-003 | Scope | S1 và E1 | U1 đánh giá cả hai | Tạo hai rating độc lập. |
| TC-US03-004 | Update | U1 có 3★ | Đổi 5★ | Tổng lượt không tăng; điểm trung bình tính lại. |
| TC-US03-005 | Delete | U1 có rating | Xóa rating | Rating U1 bị loại; tổng lượt giảm 1 và điểm trung bình tính lại. |
| TC-US03-006 | Rounding | Average lần lượt 4.20, 4.21, 4.29 | Hiển thị UI/API public | Kết quả lần lượt 4.2, 4.3, 4.3. |
| TC-US03-007 | Comment restriction | U1 bị khóa bình luận nhưng không khóa account | Đối chiếu aggregate | Rating U1 vẫn được tính. |
| TC-US03-008 | Account lock | U1 bị khóa toàn bộ account | Đối chiếu aggregate | Rating U1 bị loại khỏi điểm/tổng công khai mà không xóa record. |
| TC-US03-009 | Unlock | U1 được mở khóa | Recalculate | Rating cũ tự được tính lại nếu còn tồn tại. |
| TC-US03-010 | Idempotency/error | Retry hoặc API lỗi | Gửi/đổi/xóa nhiều lần | Chỉ có một state hợp lệ; lỗi không làm sai tổng lượt. |
