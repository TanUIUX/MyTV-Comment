# EP01 — TRẢI NGHIỆM BÌNH LUẬN VÀ ĐÁNH GIÁ

## 1. Thông tin Epic

| Thuộc tính | Nội dung |
|---|---|
| Epic ID | EP01 |
| Tên Epic | Trải nghiệm bình luận và đánh giá |
| Mục tiêu | Cho phép người xem khám phá thảo luận, đánh giá nội dung và tự quản lý bình luận của mình một cách rõ ràng, liên tục trên web/mobile |
| Đối tượng | Người xem chưa đăng nhập; người dùng đã đăng nhập |
| Giá trị kinh doanh | Tăng tương tác với phim, hỗ trợ người xem ra quyết định xem và tạo nền tảng cho cộng đồng MyTV |
| Phạm vi | Phim lẻ, series và từng tập phim trong giai đoạn Phim truyện |

## 2. Kết quả mong đợi

- Người chưa đăng nhập vẫn đọc được các bình luận đang hiển thị.
- Người dùng hiểu rõ mình đang xem hoặc viết bình luận cho series hay tập nào.
- Người dùng có thể đánh giá, đăng, sửa và xóa nội dung của chính mình.
- Số lượng, điểm đánh giá và trạng thái bình luận được cập nhật nhất quán.
- Nội dung có Spoiler và cảnh phim được hiển thị an toàn, đúng ngữ cảnh.

## 3. Chỉ số gợi ý

- Tỷ lệ người xem mở/đọc khu vực bình luận.
- Tỷ lệ người xem đăng nhập sau khi chọn thao tác tương tác.
- Tỷ lệ người xem gửi đánh giá hoặc bình luận.
- Tỷ lệ gửi bình luận thành công.
- Tỷ lệ lỗi khi chuyển ngữ cảnh series/tập.
- Thời gian cập nhật số lượng bình luận và điểm đánh giá.

---

## US01 — Đọc khu vực bình luận

### User Story

**Là người xem**, tôi muốn đọc bình luận về nội dung đang xem mà không cần đăng nhập, để tham khảo ý kiến cộng đồng trước hoặc trong khi xem phim.

### Giá trị

- Giảm rào cản tiếp cận cộng đồng.
- Tăng khả năng người dùng chú ý đến phim và các thảo luận nổi bật.
- Tạo điểm chuyển đổi tự nhiên từ người xem chưa đăng nhập sang người dùng tương tác.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Phim được Admin bật tính năng bình luận.
- Hệ thống có ít nhất trạng thái bình luận được phép hiển thị công khai.

### Acceptance Criteria

1. Khi phim đang mở bình luận, người xem chưa đăng nhập truy cập trang chi tiết/xem phim thì nhìn thấy khu vực Bình luận.
2. Người chưa đăng nhập đọc được toàn bộ bình luận và reply đang ở trạng thái công khai.
3. Khu vực Bình luận hiển thị rõ ràng trên cả web và mobile theo thiết kế đã duyệt.
4. Nội dung bị ẩn, bị xóa hoặc đang chờ duyệt không được hiển thị cho người xem khác.
5. Khi người chưa đăng nhập chọn một thao tác yêu cầu tương tác, hệ thống hiển thị luồng đăng nhập.
6. Sau khi đăng nhập thành công, hệ thống đưa người dùng trở lại đúng phim/tập; nếu khả thi, tiếp tục thao tác mà người dùng vừa chọn.
7. Khi phim bị đóng bình luận, toàn bộ khu vực Bình luận không hiển thị. Quy tắc chi tiết được quản lý tại US12 và US15.

### Quy tắc nghiệp vụ

- Áp dụng nguyên tắc “Mở để đọc — Đăng nhập để tương tác”.
- Chỉ hiển thị dữ liệu mà người xem có quyền xem.
- Việc không đăng nhập không làm thay đổi thứ tự hoặc nội dung bình luận công khai.

### Phụ thuộc

- Dịch vụ nội dung/phim.
- Hệ thống đăng nhập MyTV.
- US12 — Quản lý trạng thái và phạm vi hiển thị bình luận.

### Ngoài phạm vi

- Cá nhân hóa thứ tự bình luận theo từng người xem.
- Bình luận trên nội dung ngoài Phim truyện trong giai đoạn hiện tại.

---

## US02 — Xem bình luận theo series/tập, số lượng và sắp xếp

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

## US03 — Đánh giá series và tập phim

### User Story

**Là người dùng đã đăng nhập**, tôi muốn đánh giá series hoặc tập phim bằng thang điểm 5 sao và có thể thay đổi đánh giá, để thể hiện cảm nhận của mình với cộng đồng.

### Giá trị

- Cung cấp tín hiệu chất lượng nội dung cho người xem khác.
- Tạo thêm hình thức tương tác nhẹ bên cạnh bình luận.
- Cung cấp dữ liệu phục vụ phân tích và đề xuất nội dung.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Nội dung được phép đánh giá.
- Người dùng đã đăng nhập khi gửi hoặc thay đổi đánh giá.

### Acceptance Criteria

1. Đầu khu vực Bình luận hiển thị thang điểm 5 sao, điểm trung bình và tổng số lượt đánh giá.
2. Người chưa đăng nhập xem được điểm trung bình và tổng lượt đánh giá.
3. Khi người chưa đăng nhập chọn đánh giá, hệ thống yêu cầu đăng nhập.
4. Mỗi tài khoản chỉ có một đánh giá hiện hành trên mỗi series và một đánh giá hiện hành trên mỗi tập.
5. Đánh giá ở cấp series và đánh giá ở cấp tập là hai bản ghi độc lập.
6. Người dùng có thể thay đổi đánh giá đã gửi mà không tạo thêm lượt đánh giá mới.
7. Khi có đánh giá mới hoặc thay đổi đánh giá, điểm trung bình và tổng lượt được tính lại tự động.
8. Sau khi gửi thành công, giao diện hiển thị rõ số sao hiện tại của người dùng.
9. Nếu gửi thất bại, hệ thống giữ trạng thái cũ và thông báo để người dùng thử lại.

### Quy tắc nghiệp vụ

- Giá trị hợp lệ từ 1 đến 5 sao.
- Hệ thống phải chống gửi trùng do người dùng bấm nhiều lần hoặc lỗi mạng.
- Quy tắc làm tròn điểm trung bình cần thống nhất trên các nền tảng.

### Phụ thuộc

- Hệ thống tài khoản MyTV.
- Dịch vụ nội dung định danh series/tập.

### Điểm cần PO chốt

- Số chữ số thập phân của điểm trung bình.
- Có loại trừ đánh giá của tài khoản bị khóa/vi phạm hay không.

---

## US04 — Đăng bình luận

### User Story

**Là người dùng đã đăng nhập**, tôi muốn đăng bình luận bằng văn bản, emoji và tùy chọn Spoiler, để chia sẻ cảm nhận về series hoặc tập phim đang xem.

### Giá trị

- Tạo nguồn nội dung cộng đồng cốt lõi cho MyTV.
- Cho phép người xem bày tỏ cảm xúc trong ngữ cảnh phim.
- Tạo dữ liệu cho tương tác, kiểm duyệt và vận hành nội dung.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập và còn quyền bình luận.
- Phim đang mở bình luận.
- Người dùng đang ở đúng phạm vi series hoặc tập.

### Acceptance Criteria

1. Người dùng nhập được văn bản và emoji trong ô bình luận.
2. Người dùng có thể đánh dấu “Bình luận có Spoiler” trước khi gửi.
3. Hệ thống liên kết bình luận với đúng series hoặc tập hiện tại.
4. Hệ thống gửi nội dung qua luồng kiểm duyệt tương ứng với cấu hình của phim.
5. Ở chế độ hiển thị ngay, bình luận an toàn xuất hiện sau khi AI chấp nhận.
6. Ở chế độ chờ duyệt, tác giả nhìn thấy bình luận cùng trạng thái chờ; người khác chưa nhìn thấy.
7. Bình luận Spoiler được che và hiển thị cảnh báo “Spoiler — Nhấn để xem” cho người xem khác.
8. Bình luận hiển thị tên theo một trong hai phương án: số thuê bao đã che bốn số cuối hoặc nickname đã được chấp nhận.
9. Không hiển thị đầy đủ số điện thoại hoặc dữ liệu định danh nhạy cảm trong bình luận.
10. Sau khi gửi thành công, nội dung ô nhập được xóa và hệ thống phản hồi trạng thái rõ ràng.
11. Khi gửi lỗi, hệ thống không tạo bản ghi trùng và cho phép người dùng thử lại.
12. Người không có quyền bình luận không thể gửi nội dung qua giao diện hoặc API.

### Quy tắc nghiệp vụ

- Chỉ tài khoản đăng nhập mới được đăng bình luận.
- Nội dung phải tuân thủ chính sách cộng đồng và được kiểm duyệt.
- Không cho phép tải ảnh/video cá nhân trong luồng bình luận.
- Giới hạn ký tự, URL và tần suất đăng cần được cấu hình thay vì cố định trong giao diện.

### Phụ thuộc

- EP03 — An toàn và kiểm duyệt.
- Hệ thống tài khoản/nickname MyTV.

### Điểm cần PO chốt

- Độ dài tối thiểu/tối đa.
- Có cho phép bình luận chỉ chứa emoji hay không.
- Có cho phép URL hay không và danh sách domain được phép.
- Quy tắc tạo, đổi và kiểm duyệt nickname.

---

## US05 — Sửa và xóa bình luận

### User Story

**Là tác giả bình luận**, tôi muốn sửa hoặc xóa nội dung của chính mình, để cập nhật ý kiến hoặc loại bỏ nội dung không còn muốn hiển thị.

### Giá trị

- Trao quyền kiểm soát nội dung cho người dùng.
- Giảm nhu cầu yêu cầu hỗ trợ thủ công.
- Duy trì lịch sử phục vụ kiểm duyệt và audit.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập đúng tài khoản sở hữu bình luận/reply.
- Bình luận chưa bị hệ thống xóa vĩnh viễn.

### Acceptance Criteria

1. Tác giả có thể sửa bình luận hoặc reply của mình bất kỳ lúc nào khi còn quyền bình luận.
2. Người dùng khác không thể sửa hoặc xóa nội dung không thuộc sở hữu của họ.
3. Nội dung sửa được tạo thành phiên bản mới và đi qua kiểm duyệt.
4. Trong thời gian phiên bản mới chờ duyệt, người khác tiếp tục thấy phiên bản cũ đã được duyệt.
5. Tác giả thấy rõ phiên bản mới đang chờ duyệt.
6. Khi phiên bản mới được duyệt, hệ thống thay thế phiên bản công khai nhưng giữ nguyên định danh bình luận, Like và thread hiện có.
7. Khi phiên bản mới bị từ chối, phiên bản công khai gần nhất tiếp tục được hiển thị.
8. Tác giả có thể xóa bình luận gốc hoặc reply của mình sau bước xác nhận.
9. Khi xóa bình luận gốc, toàn bộ reply thuộc thread bị xóa khỏi giao diện công khai.
10. Khi xóa một reply, chỉ reply đó bị xóa; các nội dung khác trong thread không bị ảnh hưởng.
11. Sau khi xóa, số lượng bình luận công khai được cập nhật.
12. Nội dung được xóa mềm và giữ 90 ngày phục vụ audit trước khi xử lý theo chính sách lưu trữ.
13. Nếu tác giả xóa bình luận khi có phiên bản sửa đang chờ, phiên bản chờ không được phép xuất bản sau đó.

### Quy tắc nghiệp vụ

- Không giới hạn thời gian sửa bình luận.
- Xóa bình luận gốc đồng nghĩa xóa toàn bộ thread khỏi trải nghiệm người dùng.
- Admin vẫn có quyền xử lý bình luận theo chính sách CMS.
- Việc lưu 90 ngày không đồng nghĩa Admin được phép phục hồi công khai nếu chưa có quy trình được phê duyệt.

### Phụ thuộc

- US11 — AI kiểm duyệt theo hai chế độ.
- US16 — Audit log và chính sách lưu trữ.

### Điểm cần PO chốt

- Có hiển thị nhãn “Đã chỉnh sửa” sau khi phiên bản mới được duyệt hay không.
- Người dùng có được xem lịch sử phiên bản của chính mình hay không.

---

## US06 — Bình luận kèm cảnh phim

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

## 4. Điều kiện hoàn thành Epic

- Sáu User Story đạt acceptance criteria theo phạm vi phát hành đã chọn.
- Web/mobile hiển thị nhất quán theo thiết kế được duyệt.
- Dữ liệu series/tập không bị trộn lẫn.
- Quyền đọc, đăng, sửa và xóa được kiểm tra ở cả giao diện và API.
- Các sự kiện chính có tracking phục vụ US19.
- Các quy tắc chưa được PO chốt đã được chuyển thành quyết định chính thức trước khi story liên quan vào sprint.
