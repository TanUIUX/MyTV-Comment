# US03 — Đánh giá phim/tập phim

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá  
> Filename giữ tên legacy để không làm gãy liên kết repo.  
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md) · [User Flow →](US03_USER_FLOW.md)

### User Story

**Là người dùng đã đăng nhập**, tôi muốn đánh giá nội dung đang xem bằng thang điểm 5 sao và có thể thay đổi đánh giá, để thể hiện cảm nhận hiện tại của mình với cộng đồng.

### Giá trị

- Cho cộng đồng một tín hiệu chất lượng nhanh, gọn ngay đầu khu vực Bình luận.
- Cho phép người dùng cập nhật cảm nhận theo thời gian mà không làm nhiễu tổng lượt đánh giá.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Phim lẻ/tập phim đã được định danh chính xác trên hệ thống nội dung.
- Khu vực Bình luận của content scope hiện tại đang được bật (rating nằm trong khu vực này).

### Acceptance Criteria

1. Đầu tab Bình luận hiển thị thang điểm 5 sao, điểm trung bình và tổng số lượt đánh giá.
2. Guest xem được aggregate nhưng phải login để đánh giá; login quay lại đúng content và không auto-rating.
3. **Phim lẻ:** rating cấp phim. **Phim bộ:** rating chỉ cấp tập hiện tại; **không có rating cấp Series**.
4. Mỗi account chỉ có một rating hiện hành trên mỗi content scope.
5. User được thay đổi rating 1–5 sao; đổi rating không tăng tổng lượt.
6. **Không hỗ trợ xóa rating** sau khi đã đánh giá.
7. Chọn số sao → submit ngay, không cần nút Gửi đánh giá.
8. Submit/update lỗi → giữ/revert về rating hợp lệ trước đó; không tạo state nửa chừng và không tự retry.
9. Average hiển thị 1 chữ số thập phân và luôn làm tròn lên đến 0.1 gần nhất, ví dụ `4.21 → 4.3`, `4.20 → 4.2`.
10. Account Lock tạm loại rating khỏi cả average và public total; unlock tính lại nếu record còn. Comment Lock không loại rating.
11. Scope Đóng ẩn rating cùng tab Bình luận content.
12. SmartTV cho user đã login đánh giá 1–5 sao bằng remote và thay đổi rating.

### Quy tắc nghiệp vụ

- Giá trị rating: integer 1–5.
- Một account đóng góp tối đa một current rating/content scope.
- Rating record không bị xóa do Account Lock; khóa/mở khóa chỉ ảnh hưởng việc rating có được tính vào aggregate công khai hay không.
- Tập rating dùng để tính **average** và **total count công khai** phải nhất quán: rating của account đang Account Lock không tham gia cả hai chỉ số.
- Không có thao tác DELETE rating ở UX/API product contract MVP; con đường duy nhất để thay đổi là chọn lại mức sao 1–5.
- Rating nằm trong khu vực Bình luận và bị ẩn khi phim/tập ở trạng thái Đóng bình luận theo US12.

*Xem thêm: [REQUIREMENTS_A11Y_SECURITY.md](../REQUIREMENTS_A11Y_SECURITY.md) — rating 5 sao dùng `radiogroup`, điều hướng phím mũi tên, nhãn "{n} sao".*

### Phụ thuộc

- US12 — Quản lý trạng thái và phạm vi hiển thị bình luận (scope Đóng ẩn rating).
- US16 — Quản lý người dùng vi phạm và Account Lock.

### Điểm cần PO chốt

- Không còn blocker PO cho rating trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US03-001 | Guest | Content đã có rating; phiên khách | Mở tab Bình luận rồi chạm vào sao | Guest thấy 5 sao + average + total; chạm sao ra auth gate; login xong quay lại đúng content và không auto-rating. |
| TC-US03-002 | Boundary | U1 đã đăng nhập | Gửi 1★ và 5★; thử 0, 6 và giá trị thập phân qua UI/API | Chỉ integer 1–5 hợp lệ; các giá trị còn lại bị từ chối nhất quán ở cả hai bề mặt. |
| TC-US03-003 | Scope | Phim bộ S1 có E1 và E2 | U1 đánh giá E1 rồi E2 | Rating của E1/E2 độc lập theo tập; **không tồn tại rating cấp Series** ở UI và API. |
| TC-US03-004 | Update | U1 đã có 3★ trên content | Chọn lại 5★ | Rating hiện hành đổi thành 5★; tổng lượt không tăng; average được tính lại. |
| TC-US03-005 | No delete | U1 đã có rating trên content | Rà UI (chạm lại đúng sao đang chọn, long-press, menu ngữ cảnh) và rà API contract | Không có action xóa/toggle-to-empty trên UI và không có endpoint DELETE rating; rating chỉ có thể đổi sang mức 1–5 sao. |
| TC-US03-006 | Error | U1 có rating hợp lệ; mock lỗi khi update | Chọn mức sao mới và để request thất bại | Revert về rating hợp lệ trước đó + thông báo lỗi; không tạo state nửa chừng và không tự retry ngầm. |
| TC-US03-007 | Account Lock | U1 có rating; khóa rồi mở khóa toàn bộ account U1 | Đối chiếu average và total công khai ở ba mốc | Khi khóa, rating U1 bị loại khỏi **cả average và total công khai** mà không xóa record; mở khóa tính lại nếu record vẫn còn. |
| TC-US03-008 | SmartTV | U1 đã đăng nhập trên SmartTV | Chọn số sao bằng remote rồi đổi sang mức khác | Submit và update rating hoạt động như Phone/Web. |
| TC-US03-009 | Comment Lock | U1 bị Khóa bình luận nhưng account không bị khóa | Đối chiếu aggregate | Rating của U1 vẫn được tính trong cả average và total count công khai. |
| TC-US03-010 | Rounding | Average thô lần lượt 4.20, 4.21, 4.29, 4.31 | Đối chiếu hiển thị UI và API public | Kết quả lần lượt 4.2, 4.3, 4.3, 4.4 — luôn làm tròn lên đến 0.1 gần nhất, hiển thị đúng 1 chữ số thập phân. |
| TC-US03-011 | Scope Đóng | Content đang ở trạng thái Đóng bình luận, đã có rating trước đó | Mở trang phim/tập; thử gửi rating qua API; sau đó Admin mở lại scope | Khi Đóng: toàn bộ khu vực Bình luận gồm thang sao/average/total bị ẩn và API chặn rating mới. Khi mở lại: rating và aggregate cũ hiển thị lại đúng. |
| TC-US03-012 | Scope phim lẻ | Phim lẻ M1 | U1 đánh giá M1 rồi đổi mức sao | Rating nằm ở cấp phim; U1 chỉ có một rating hiện hành trên M1. |
| TC-US03-013 | Idempotency | U1 gửi/đổi rating với retry và request lỗi giữa chừng | Gửi lặp cùng thao tác nhiều lần | Chỉ tồn tại một rating hiện hành hợp lệ; tổng lượt không bị sai lệch do retry. |

### Microcopy

| Trạng thái | Nội dung hiển thị |
|---|---|
| Guest chạm sao | **Đăng nhập để đánh giá**<br>Đăng nhập để chấm điểm nội dung này.<br>`[Đăng nhập]` |
| Submit/update lỗi | **Không thể cập nhật đánh giá. Vui lòng thử lại.** |
