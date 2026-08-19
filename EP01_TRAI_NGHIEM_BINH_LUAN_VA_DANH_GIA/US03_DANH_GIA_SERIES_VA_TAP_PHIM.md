# US03 — Đánh giá phim/tập phim

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá  
> Filename giữ tên legacy để không làm gãy liên kết repo.  
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md) · [User Flow →](US03_USER_FLOW.md)

### User Story

**Là người dùng đã đăng nhập**, tôi muốn đánh giá nội dung đang xem bằng thang điểm 5 sao và có thể thay đổi đánh giá, để thể hiện cảm nhận hiện tại của mình với cộng đồng.

### Ưu tiên

**Must**

### Acceptance Criteria

1. Đầu tab Bình luận hiển thị thang điểm 5 sao, điểm trung bình và tổng số lượt đánh giá.
2. Guest xem được aggregate nhưng phải login để đánh giá; login quay lại đúng content và không auto-rating.
3. **Phim lẻ:** rating cấp phim. **Phim bộ:** rating chỉ cấp tập hiện tại; **không có rating cấp Series**.
4. Mỗi account chỉ có một rating hiện hành trên mỗi content scope.
5. User được thay đổi rating 1–5 sao; đổi rating không tăng tổng lượt.
6. **Không hỗ trợ xóa rating** sau khi đã đánh giá.
7. Chọn số sao → submit ngay, không cần nút Gửi đánh giá.
8. Submit/update lỗi → giữ/revert về rating hợp lệ trước đó; không tạo state nửa chừng và không tự retry.
9. Average hiển thị 1 chữ số thập phân và luôn làm tròn lên đến 0.1 gần nhất.
10. Account Lock tạm loại rating khỏi cả average và public total; unlock tính lại nếu record còn. Comment Lock không loại rating.
11. Scope Đóng ẩn rating cùng tab Bình luận content.
12. SmartTV cho user đã login đánh giá 1–5 sao bằng remote và thay đổi rating.

### Quy tắc nghiệp vụ

- Giá trị rating: integer 1–5.
- Một account đóng góp tối đa một current rating/content scope.
- Rating record không bị xóa do Account Lock.
- Không có thao tác DELETE rating ở UX/API product contract MVP.

## Test Cases

| ID | Loại | Bước | Kết quả mong đợi |
|---|---|---|---|
| TC-US03-001 | Guest | Tap sao khi chưa login | Auth gate; login xong không auto-rating. |
| TC-US03-002 | Boundary | Gửi 1★/5★; thử 0/6/thập phân | Chỉ integer 1–5 hợp lệ. |
| TC-US03-003 | Scope | Phim bộ E1 rồi E2 | Rating E1/E2 độc lập; không có Series rating. |
| TC-US03-004 | Update | Có 3★, chọn 5★ | Update current rating; total không tăng. |
| TC-US03-005 | No delete | User đã có rating | UI không có action xóa/toggle-to-empty. |
| TC-US03-006 | Error | Update lỗi | Revert rating cũ + thông báo lỗi; không retry ngầm. |
| TC-US03-007 | Account Lock | Lock/unlock account | Lock loại khỏi aggregate; unlock tính lại nếu record còn. |
| TC-US03-008 | SmartTV | Login và chọn sao bằng remote | Submit rating như Phone/Web. |

### Microcopy

| Trạng thái | Nội dung |
|---|---|
| Guest chạm sao | **Đăng nhập để đánh giá** · Đăng nhập để chấm điểm nội dung này. |
| Submit lỗi | **Không thể cập nhật đánh giá. Vui lòng thử lại.** |
