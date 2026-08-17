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
6. Hệ thống cung cấp ba chế độ: Nổi bật, Mới nhất và Được yêu thích.
7. Nổi bật là chế độ mặc định.
8. Ở chế độ Nổi bật, các bình luận được Admin ghim hiển thị trước; giới hạn mặc định tối đa ba bình luận.
9. Ở chế độ Mới nhất, bình luận chính được sắp xếp từ mới đến cũ.
10. Ở chế độ Được yêu thích, bình luận chính được sắp theo số Like giảm dần; nếu bằng nhau thì bình luận mới hơn đứng trước.
11. Lựa chọn sắp xếp chỉ áp dụng cho phạm vi series/tập hiện tại.
12. Khi danh sách dài, hệ thống hỗ trợ tải thêm hoặc phân trang mà không lặp hoặc bỏ sót bình luận.

### Quy tắc nghiệp vụ

- Reply được tính vào tổng số bình luận nhưng không được trộn thành bình luận gốc trong danh sách.
- Bình luận đang chờ duyệt, bị ẩn hoặc xóa không được tính vào tổng số công khai.
- Công thức xếp hạng phần còn lại của chế độ Nổi bật cần được PO chốt trước refinement.

### Phụ thuộc

- US07 — Like và Unlike bình luận.
- US15 — Quản lý bình luận nổi bật và cấu hình theo phim.

### Điểm cần PO chốt

- Số bình luận tải lần đầu và mỗi lần “Xem thêm”.
- Số reply hiển thị ban đầu.
- Công thức xếp hạng Nổi bật bên dưới các bình luận được ghim.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra cách hệ thống phân tách dữ liệu series/tập, tính tổng số công khai và áp dụng đúng ba chế độ sắp xếp mà không lặp hoặc bỏ sót dữ liệu.

### Rủi ro chính

- Trộn bình luận giữa series và tập khi chuyển ngữ cảnh.
- Tính cả Chờ duyệt/Ẩn/Xóa vào tổng số công khai.
- Sai tie-break khi số Like bằng nhau hoặc phân trang làm lặp bản ghi.

### Dữ liệu kiểm thử

Series S1 có tập E1/E2; tạo bình luận gốc, reply, bình luận ghim, bình luận có số Like bằng nhau và nội dung không công khai ở từng phạm vi.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US02-001 | Functional | S1 có bình luận series và E1/E2 | Mở lần lượt phạm vi series, E1 và E2 | Mỗi phạm vi chỉ trả đúng bình luận được gắn với scope tương ứng. |
| TC-US02-002 | Navigation | Đang xem E1 rồi chuyển sang E2 | Chuyển tập liên tục và tải lại danh sách | Dữ liệu E2 được tải mới; không giữ lại bình luận E1 hoặc dữ liệu cũ từ cache sai scope. |
| TC-US02-003 | Data integrity | Scope có 2 comment gốc và 3 reply; thêm nội dung Chờ duyệt/Ẩn/Xóa | Đối chiếu tổng số trên UI với dữ liệu công khai | Tổng số gồm comment gốc + reply công khai; loại trừ trạng thái không công khai. |
| TC-US02-004 | Functional | Có dữ liệu đủ cho ba thứ tự | Mở danh sách lần đầu, kiểm tra bộ chọn sắp xếp và đổi từng lựa chọn | Nổi bật là mặc định; có đúng Nổi bật, Mới nhất, Được yêu thích. |
| TC-US02-005 | Sorting | Có 4 bình luận ghim và cấu hình giới hạn 3 | Chọn Nổi bật | Tối đa 3 bình luận ghim được ưu tiên; phần còn lại tuân theo công thức đã chốt. |
| TC-US02-006 | Sorting | Có comment ở nhiều thời điểm | Chọn Mới nhất | Comment gốc được sắp từ mới đến cũ; reply không bị trộn thành item gốc. |
| TC-US02-007 | Sorting/Boundary | Có comment A/B cùng số Like, A mới hơn B | Chọn Được yêu thích | Comment có Like cao hơn đứng trước; khi bằng Like, comment mới hơn đứng trước. |
| TC-US02-008 | Scope isolation | Đã chọn E1 và một chế độ sắp xếp | Chuyển trang, refresh hoặc tải thêm | Chế độ và dữ liệu chỉ áp dụng cho scope hiện tại; không ảnh hưởng scope khác ngoài thiết kế. |
| TC-US02-009 | Pagination | Có danh sách lớn hơn kích thước trang, ID ổn định | Tải thêm/phân trang nhiều lần đến cuối danh sách | Không trùng, không bỏ sót và không tạo vòng lặp; tổng số khớp dữ liệu công khai. |

### Điểm cần PO chốt trước khi khóa expected result

- Kích thước trang/số item tải lần đầu.
- Công thức Nổi bật bên dưới các comment ghim và cách làm tròn thời gian nếu có.
