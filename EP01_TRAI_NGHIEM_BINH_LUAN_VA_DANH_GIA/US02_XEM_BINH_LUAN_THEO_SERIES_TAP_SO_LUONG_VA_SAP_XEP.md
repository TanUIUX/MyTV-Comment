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
6. Hệ thống cung cấp ba chế độ: **Sắp xếp Nổi bật, Mới nhất, Nhiều lượt thích nhất**; Sắp xếp Nổi bật là mặc định.
7. Ở chế độ Sắp xếp Nổi bật, bình luận được Admin **Ghim** hiển thị trước và có **hard max 3 bình luận ghim** cho mỗi scope. Nhóm ghim (tối đa 3) nằm ngoài quota 10 item của lần tải đầu và bị loại khỏi danh sách xếp theo Sắp xếp Nổi bật để tránh hiển thị trùng; tổng số công khai vẫn đếm comment ghim đúng 1 lần.
8. Sau nhóm ghim, các comment được xếp theo `FeaturedScore = 0.5×ln(1+Like) + 0.3×ln(1+Reply) + 0.2×e^(-AgeHours/72)`.
9. Nếu hai comment có Featured Score bằng nhau, comment mới hơn đứng trước; nếu vẫn bằng nhau dùng `comment_id` làm tie-break ổn định.
10. Ở chế độ Mới nhất, bình luận chính được sắp từ mới đến cũ.
11. Ở chế độ Nhiều lượt thích nhất, bình luận chính được sắp theo **Net Like công khai hiện tại** giảm dần; nếu bằng nhau thì bình luận mới hơn đứng trước.
12. Lần tải đầu lấy **10 comment gốc**; khi cuộn xuống hệ thống lazy load **10 comment gốc/lần** cho tới hết.
13. Lazy load không được lặp hoặc bỏ sót comment và chỉ áp dụng trong scope series/tập hiện tại.
14. Like do account đang **Khóa tài khoản** tạo trước đó vẫn giữ record nhưng **tạm không được tính vào Net Like công khai, Featured Score và ranking** trong thời gian khóa; khi account được mở khóa, Like được tính lại nếu Like record và target vẫn hợp lệ.

### Quy tắc nghiệp vụ

- Reply được tính vào tổng số bình luận nhưng không được trộn thành bình luận gốc trong danh sách.
- Tổng số công khai = số comment/reply có **effective visibility = public tại thời điểm đọc** (xem Effective Visibility Resolver, US12). Bình luận Chờ duyệt, Từ chối, Ẩn hoặc Xóa không được tính vào tổng số công khai và không tham gia Featured Score. Nội dung non-public tạm thời do **Account Lock** (kể cả cascade root) và do **scope Đóng** (visibility gate, KHÔNG phải KPI — xem Quyết định 3, chỉ ảnh hưởng phần hiển thị/đếm cho end-user, không ảnh hưởng KPI dashboard) cũng KHÔNG được tính vào con số hiển thị cho end-user tại thời điểm đọc.
- Like của chính tác giả là Like hợp lệ và được tính vào Featured Score theo US07, trừ khi account tạo Like đang bị Khóa tài khoản theo rule visibility/KPI của US07/US16.
- `Like` trong công thức Featured Score là **Net Like công khai hiện tại**, không phải tổng Like record bất kể trạng thái account của liker.
- Hard max comment ghim là 3, không cho cấu hình vượt mức này.
- `AgeHours` tính từ thời điểm comment được đăng; hệ số decay freshness là 72 giờ.

### Phụ thuộc

- US07 — Like và Unlike bình luận.
- US08 — Trả lời bình luận.
- US15 — Quản lý bình luận ghim và cấu hình theo phim.
- US16 — Quản lý người dùng vi phạm và Account Lock.

### Điểm cần PO chốt

- Không còn điểm PO blocker cho phạm vi sắp xếp/pagination hiện tại.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra phân tách dữ liệu series/tập, tổng số công khai, hard max ghim, Featured Score và lazy load 10 item không lặp/bỏ sót.

### Rủi ro chính

- Trộn bình luận giữa series và tập khi chuyển ngữ cảnh.
- Tính nội dung không công khai vào count/ranking.
- Tính Like của account đang bị Account Lock vào Net Like/ranking công khai.
- Vượt hard max 3 comment ghim.
- Featured Score/tie-break sai hoặc lazy load tạo bản ghi trùng.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US02-001 | Functional | S1 có comment series và E1/E2 | Mở lần lượt series, E1, E2 | Mỗi scope chỉ trả đúng comment gắn với scope tương ứng. |
| TC-US02-002 | Navigation | Đang xem E1 rồi chuyển E2 | Chuyển tập và refresh | Không giữ comment E1 trong E2 hoặc cache sai scope. |
| TC-US02-003 | Counter | Scope có comment/reply công khai và Chờ duyệt/Ẩn/Xóa | Đối chiếu tổng số | Chỉ comment + reply công khai được tính. |
| TC-US02-004 | Sorting | Có dữ liệu đủ ba chế độ | Mở lần đầu và đổi sort | Sắp xếp Nổi bật là mặc định; có đúng ba lựa chọn (Sắp xếp Nổi bật/Mới nhất/Nhiều lượt thích nhất). |
| TC-US02-005 | Pin boundary | Đã có 3 comment ghim | Thử ghim comment thứ 4 | Không thể có quá 3 comment ghim trong cùng scope. |
| TC-US02-006 | Featured formula | Có comment khác nhau về Like/Reply/AgeHours | Tính score và mở Nổi bật | Thứ tự khớp công thức 50% Like, 30% Reply, 20% freshness với decay 72h. |
| TC-US02-007 | Featured tie | Hai comment cùng score | Mở Nổi bật | Comment mới hơn đứng trước; nếu vẫn bằng thì `comment_id` tạo thứ tự ổn định. |
| TC-US02-008 | Latest | Có comment ở nhiều thời điểm | Chọn Mới nhất | Comment gốc mới → cũ; reply không bị trộn thành item gốc. |
| TC-US02-009 | Most liked | Có Net Like khác nhau và trường hợp bằng nhau | Chọn Nhiều lượt thích nhất | Net Like công khai cao hơn đứng trước; bằng nhau thì comment mới hơn trước. |
| TC-US02-010 | Initial load | Scope có >10 comment | Mở khu vực bình luận | Lần đầu trả đúng 10 comment gốc. |
| TC-US02-011 | Lazy load | Còn >20 comment sau lần đầu | Cuộn liên tục | Mỗi batch thêm tối đa 10 comment; không trùng/bỏ sót và dừng đúng cuối danh sách. |
| TC-US02-012 | Locked liker | U1 đã Like C1; sau đó U1 bị Account Lock | Đối chiếu Net Like/Featured Score/ranking trước khóa, khi khóa và sau mở khóa | Khi khóa, Like U1 tạm bị loại khỏi Net Like công khai/ranking nhưng record không mất; mở khóa tính lại nếu record/target còn hợp lệ. |
| TC-US02-013 | Counter exclusion | Root comment của tác giả đang Account Lock có reply hợp lệ; một scope khác đang Đóng bình luận | Đọc tổng số công khai ở cả hai trường hợp | Comment/reply thuộc root bị Account Lock (kể cả reply cascade theo root) và toàn bộ comment/reply trong scope Đóng không được tính vào tổng số công khai hiển thị cho end-user; KPI/Engagement Score dashboard Admin không bị ảnh hưởng (đối chiếu US19). |
| TC-US02-014 | Featured formula (tính tay) | Bộ dữ liệu (Like, Reply, AgeHours): (0,0,0), (10,5,24), (10,5,72), (100,0,168) | Tính `FeaturedScore = 0.5×ln(1+Like)+0.3×ln(1+Reply)+0.2×e^(-AgeHours/72)` cho từng bộ và đối chiếu giá trị hệ thống trả về | Giá trị số phải khớp: (0,0,0)→0.2; (10,5,24)≈0.5×ln11+0.3×ln6+0.2×e^(-1/3)≈1.1993+0.5375+0.1435≈1.8803; (10,5,72)≈1.1993+0.5375+0.0736≈1.8104; (100,0,168)≈0.5×ln101+0×ln1+0.2×e^(-168/72)≈2.3105+0+0.0193≈2.3298 (sai số cho phép ≤0.001, verify số liệu chứ không chỉ thứ tự). |

### Microcopy

| Trạng thái | Nội dung hiển thị |
|---|---|
| Affordance chọn Sắp xếp Nổi bật | **Sắp xếp Nổi bật**<br>Ưu tiên bình luận được ghim và có tương tác cao, còn mới.<br>`[Chọn]` |
| Affordance chọn Mới nhất | **Mới nhất**<br>Hiển thị bình luận theo thời gian đăng, mới nhất lên đầu.<br>`[Chọn]` |
| Affordance chọn Nhiều lượt thích nhất | **Nhiều lượt thích nhất**<br>Hiển thị bình luận có số lượt thích công khai cao nhất lên đầu.<br>`[Chọn]` |
