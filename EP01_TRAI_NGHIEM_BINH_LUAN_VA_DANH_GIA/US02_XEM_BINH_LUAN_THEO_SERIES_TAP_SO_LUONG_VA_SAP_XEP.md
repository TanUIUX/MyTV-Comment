# US02 — Xem bình luận theo series/tập, số lượng và sắp xếp

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là người xem**, tôi muốn xem đúng bình luận của series hoặc tập phim và sắp xếp theo nhu cầu, để theo dõi thảo luận phù hợp với nội dung mình đang xem.

### Giá trị

- Tránh lẫn thảo luận giữa các tập.
- Giúp người dùng nhanh chóng tìm được bình luận đáng chú ý hoặc mới nhất.
- Tăng khả năng khám phá các cuộc thảo luận có tương tác cao.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Series/tập phim đã được định danh chính xác trên hệ thống nội dung.
- Khu vực bình luận đang được bật.

### Acceptance Criteria

1. Phim nhiều tập có hai phạm vi độc lập: bình luận chung của series và bình luận của từng tập.
2. Khi người dùng chuyển sang tập khác, hệ thống tải đúng danh sách bình luận của tập mới.
3. Bình luận của một tập không xuất hiện trong phạm vi của tập khác hoặc phạm vi series.
4. Tiêu đề Bình luận hiển thị tổng số bình luận chính và reply thuộc phạm vi hiện tại.
5. Số lượng công khai được cập nhật khi có bình luận/reply mới được hiển thị hoặc khi nội dung bị xóa/ẩn.
6. Hệ thống cung cấp ba chế độ: **Nổi bật, Mới nhất, Được yêu thích**; Nổi bật là mặc định.
7. Ở chế độ Nổi bật, bình luận được Admin ghim hiển thị trước và có **hard max 3 bình luận ghim** cho mỗi scope.
8. Sau nhóm ghim, các comment được xếp theo `FeaturedScore = 0.5×ln(1+Like) + 0.3×ln(1+Reply) + 0.2×e^(-AgeHours/72)`.
9. Nếu hai comment có Featured Score bằng nhau, comment mới hơn đứng trước; nếu vẫn bằng nhau dùng `comment_id` làm tie-break ổn định.
10. Ở chế độ Mới nhất, bình luận chính được sắp từ mới đến cũ.
11. Ở chế độ Được yêu thích, bình luận chính được sắp theo Net Like giảm dần; nếu bằng nhau thì bình luận mới hơn đứng trước.
12. Lần tải đầu lấy **10 comment gốc**; khi cuộn xuống hệ thống lazy load **10 comment gốc/lần** cho tới hết.
13. Lazy load không được lặp hoặc bỏ sót comment và chỉ áp dụng trong scope series/tập hiện tại.

### Quy tắc nghiệp vụ

- Reply được tính vào tổng số bình luận nhưng không được trộn thành bình luận gốc trong danh sách.
- Bình luận Chờ duyệt, Từ chối, Ẩn hoặc Xóa không được tính vào tổng số công khai và không tham gia Featured Score.
- Like của chính tác giả là Like hợp lệ và được tính vào Featured Score theo US07.
- Hard max comment ghim là 3, không cho cấu hình vượt mức này.
- `AgeHours` tính từ thời điểm comment được đăng; hệ số decay freshness là 72 giờ.

### Phụ thuộc

- US07 — Like và Unlike bình luận.
- US08 — Trả lời bình luận.
- US15 — Quản lý bình luận nổi bật và cấu hình theo phim.

### Điểm cần PO chốt

- Không còn điểm PO blocker cho phạm vi sắp xếp/pagination hiện tại.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra phân tách dữ liệu series/tập, tổng số công khai, hard max ghim, Featured Score và lazy load 10 item không lặp/bỏ sót.

### Rủi ro chính

- Trộn bình luận giữa series và tập khi chuyển ngữ cảnh.
- Tính nội dung không công khai vào count/ranking.
- Vượt hard max 3 comment ghim.
- Featured Score/tie-break sai hoặc lazy load tạo bản ghi trùng.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US02-001 | Functional | S1 có comment series và E1/E2 | Mở lần lượt series, E1, E2 | Mỗi scope chỉ trả đúng comment gắn với scope tương ứng. |
| TC-US02-002 | Navigation | Đang xem E1 rồi chuyển E2 | Chuyển tập và refresh | Không giữ comment E1 trong E2 hoặc cache sai scope. |
| TC-US02-003 | Counter | Scope có comment/reply công khai và Chờ duyệt/Ẩn/Xóa | Đối chiếu tổng số | Chỉ comment + reply công khai được tính. |
| TC-US02-004 | Sorting | Có dữ liệu đủ ba chế độ | Mở lần đầu và đổi sort | Nổi bật là mặc định; có đúng ba lựa chọn. |
| TC-US02-005 | Pin boundary | Đã có 3 comment ghim | Thử ghim comment thứ 4 | Không thể có quá 3 comment ghim trong cùng scope. |
| TC-US02-006 | Featured formula | Có comment khác nhau về Like/Reply/AgeHours | Tính score và mở Nổi bật | Thứ tự khớp công thức 50% Like, 30% Reply, 20% freshness với decay 72h. |
| TC-US02-007 | Featured tie | Hai comment cùng score | Mở Nổi bật | Comment mới hơn đứng trước; nếu vẫn bằng thì `comment_id` tạo thứ tự ổn định. |
| TC-US02-008 | Latest | Có comment ở nhiều thời điểm | Chọn Mới nhất | Comment gốc mới → cũ; reply không bị trộn thành item gốc. |
| TC-US02-009 | Most liked | Có Net Like khác nhau và trường hợp bằng nhau | Chọn Được yêu thích | Net Like cao hơn đứng trước; bằng nhau thì comment mới hơn trước. |
| TC-US02-010 | Initial load | Scope có >10 comment | Mở khu vực bình luận | Lần đầu trả đúng 10 comment gốc. |
| TC-US02-011 | Lazy load | Còn >20 comment sau lần đầu | Cuộn liên tục | Mỗi batch thêm tối đa 10 comment; không trùng/bỏ sót và dừng đúng cuối danh sách. |
