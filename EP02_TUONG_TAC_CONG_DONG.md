# EP02 — TƯƠNG TÁC CỘNG ĐỒNG

## 1. Thông tin Epic

| Thuộc tính | Nội dung |
|---|---|
| Epic ID | EP02 |
| Tên Epic | Tương tác cộng đồng |
| Mục tiêu | Cho phép người dùng phản hồi, thể hiện đồng tình và duy trì cuộc trò chuyện xung quanh phim |
| Đối tượng | Người dùng MyTV đã đăng nhập |
| Giá trị kinh doanh | Tăng chiều sâu tương tác, tần suất quay lại và mức độ gắn kết giữa người xem |
| Phạm vi | Like, Reply một cấp, Mention, push notification và thông báo trong ứng dụng |

## 2. Kết quả mong đợi

- Người dùng tương tác được với bình luận mà không tạo hành vi trùng lặp.
- Hội thoại dễ đọc và không tạo cây reply quá sâu.
- Người được reply/mention nhận được thông báo và quay lại đúng ngữ cảnh.
- Tất cả tương tác tuân thủ quyền tài khoản và trạng thái bình luận.

## 3. Chỉ số gợi ý

- Tỷ lệ bình luận nhận ít nhất một Like hoặc Reply.
- Số Like và Reply trung bình trên mỗi bình luận.
- Tỷ lệ mở thông báo reply/mention.
- Tỷ lệ người dùng quay lại nội dung từ thông báo.

---

## US07 — Like và Unlike bình luận

### User Story

**Là người dùng đã đăng nhập**, tôi muốn Like hoặc Unlike một bình luận, để thể hiện sự đồng tình hoặc yêu thích đối với ý kiến đó.

### Giá trị

- Cung cấp hình thức tương tác nhanh, ít rào cản.
- Tạo tín hiệu cho sắp xếp “Được yêu thích” và bình luận nổi bật.
- Tạo dữ liệu phục vụ huy hiệu và phân tích cộng đồng.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập và còn quyền tương tác.
- Bình luận đang ở trạng thái công khai và cho phép tương tác.

### Acceptance Criteria

1. Người dùng đã đăng nhập có thể Like một bình luận hoặc reply đang hiển thị.
2. Mỗi tài khoản chỉ có một trạng thái Like hiện hành trên mỗi bình luận/reply.
3. Khi đã Like, người dùng có thể Unlike để hủy Like của mình.
4. Nút Like hiển thị đúng trạng thái hiện tại của người dùng.
5. Số Like được cập nhật gần thời gian thực sau thao tác thành công.
6. Hệ thống không tăng/giảm trùng số Like khi người dùng bấm nhiều lần, mạng chậm hoặc gửi lại request.
7. Người chưa đăng nhập chọn Like được chuyển sang luồng đăng nhập.
8. Bình luận bị ẩn, xóa hoặc không còn quyền xem không thể nhận Like mới.
9. Khi thao tác thất bại, giao diện hoàn lại trạng thái đúng và thông báo cho người dùng.
10. Số Like mới được phản ánh trong chế độ sắp xếp Được yêu thích theo độ trễ được thống nhất.

### Quy tắc nghiệp vụ

- Một tài khoản, một Like trên một bình luận/reply.
- Like không tạo thread hoặc nội dung mới.
- Khi bình luận bị xóa mềm, dữ liệu Like được giữ theo chính sách audit nhưng không hiển thị công khai.

### Phụ thuộc

- US02 — Sắp xếp bình luận.
- US17 — Huy hiệu người dùng.
- US19 — Thống kê hoạt động bình luận.

### Điểm cần PO chốt

- Có cho phép tác giả Like bình luận của chính mình hay không.
- Độ trễ tối đa chấp nhận được cho cập nhật số Like và bảng xếp hạng.

---

## US08 — Trả lời bình luận một cấp

### User Story

**Là người dùng đã đăng nhập**, tôi muốn trả lời trực tiếp một bình luận, để tham gia cuộc trò chuyện mà vẫn giữ cấu trúc hội thoại dễ theo dõi.

### Giá trị

- Tạo thảo luận hai chiều giữa người xem.
- Giúp nội dung trao đổi gắn đúng với bình luận gốc.
- Hạn chế độ phức tạp giao diện bằng mô hình reply một cấp.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập và còn quyền bình luận.
- Bình luận gốc đang hiển thị và cho phép reply.
- Phim đang mở bình luận.

### Acceptance Criteria

1. Người dùng có thể chọn Trả lời trên một bình luận gốc.
2. Reply được liên kết với đúng bình luận gốc, series/tập và nội dung hiện tại.
3. Hệ thống chỉ hỗ trợ một cấp reply bên dưới bình luận gốc.
4. Khi người dùng chọn trả lời một reply, nội dung mới vẫn được lưu ở cấp một dưới bình luận gốc; giao diện có thể tự động mention người được trả lời để giữ ngữ cảnh.
5. Reply hỗ trợ văn bản, emoji và đánh dấu Spoiler theo quy tắc của bình luận.
6. Reply đi qua cùng cơ chế kiểm duyệt với bình luận gốc.
7. Reply công khai mới làm tăng tổng số bình luận của phạm vi hiện tại.
8. Khi số reply vượt giới hạn hiển thị ban đầu, người xem có thể chọn “Xem thêm phản hồi”.
9. Tác giả có thể sửa hoặc xóa reply của mình theo quy tắc tại US05.
10. Nếu bình luận gốc bị xóa, toàn bộ reply trong thread không còn hiển thị.
11. Nếu bình luận gốc bị ẩn hoặc khóa tương tác, hệ thống không cho phép tạo reply mới.
12. Người chưa đăng nhập chọn Trả lời được chuyển sang luồng đăng nhập.

### Quy tắc nghiệp vụ

- Độ sâu tối đa là một cấp.
- Reply được tính vào tổng số bình luận.
- Reply không tham gia danh sách bình luận gốc khi sắp xếp.
- Xóa bình luận gốc làm toàn bộ thread biến mất khỏi trải nghiệm người dùng.

### Phụ thuộc

- US04 — Đăng bình luận.
- US05 — Sửa và xóa bình luận.
- US09 — Mention và nhận thông báo.
- EP03 — An toàn và kiểm duyệt.

### Điểm cần PO chốt

- Số reply hiển thị mặc định trước nút “Xem thêm”.
- Cách hiển thị khi một reply riêng lẻ bị xóa giữa hội thoại.

---

## US09 — Mention và nhận thông báo

### User Story

**Là người dùng đã đăng nhập**, tôi muốn mention một tài khoản hợp lệ và nhận thông báo khi có người reply hoặc mention mình, để tiếp tục cuộc trò chuyện liên quan.

### Giá trị

- Tăng khả năng người dùng quay lại MyTV.
- Giúp cuộc hội thoại có đối tượng rõ ràng.
- Tạo vòng lặp tương tác giữa người xem.

### Ưu tiên

**Should**

### Điều kiện tiên quyết

- Người gửi và người nhận là tài khoản hợp lệ.
- Người gửi có quyền bình luận.
- Hệ thống thông báo MyTV hoạt động.

### Acceptance Criteria

1. Người dùng có thể nhập ký tự `@` trong bình luận hoặc reply để bắt đầu chọn tài khoản hợp lệ.
2. Hệ thống chỉ tạo mention khi người dùng chọn hoặc xác định được một tài khoản hợp lệ trong hệ thống.
3. Giao diện không làm lộ số điện thoại đầy đủ hoặc dữ liệu cá nhân nhạy cảm trong kết quả tìm mention.
4. Mention được lưu theo định danh tài khoản, không chỉ theo chuỗi nickname hiển thị.
5. Khi bình luận/reply chứa mention được phép hiển thị, người được mention nhận thông báo trong ứng dụng.
6. Khi có người reply bình luận của mình, tác giả bình luận nhận thông báo trong ứng dụng.
7. Hai trường hợp reply và mention đều tạo push notification nếu người nhận cho phép push.
8. Nếu cùng một sự kiện vừa là reply vừa mention cùng một người, hệ thống tránh gửi thông báo trùng không cần thiết.
9. Bấm thông báo mở đúng phim, tập và thread/bình luận liên quan.
10. Nếu bình luận đã bị xóa, ẩn hoặc người nhận không còn quyền xem phim, hệ thống hiển thị trạng thái phù hợp thay vì mở nội dung không có quyền.
11. Người dùng không nhận thông báo cho hành động của chính mình.
12. Thông báo chỉ được gửi khi nội dung đủ điều kiện hiển thị; nội dung đang chờ duyệt không thông báo cho người khác.

### Quy tắc nghiệp vụ

- Chỉ mention tài khoản hợp lệ.
- Gửi cả push và thông báo trong ứng dụng cho reply/mention.
- Quyền nhận push phụ thuộc cài đặt hệ điều hành và tùy chọn người dùng.
- Nội dung thông báo không được làm lộ Spoiler hoặc dữ liệu nhạy cảm trên màn hình khóa.

### Phụ thuộc

- Hệ thống tài khoản/nickname MyTV.
- Dịch vụ push notification và thông báo trong ứng dụng.
- Deep link đến phim/tập/comment.
- US11 và US12 về trạng thái kiểm duyệt.

### Điểm cần PO chốt

- Phạm vi tài khoản được gợi ý khi nhập `@`.
- Người dùng có được tắt riêng thông báo reply/mention hay không.
- Thời gian lưu thông báo trong ứng dụng.

---

## 4. Điều kiện hoàn thành Epic

- Like, Reply và Mention được kiểm soát quyền ở cả giao diện và API.
- Không phát sinh Like, Reply hoặc thông báo trùng do retry.
- Reply luôn tuân thủ cấu trúc một cấp.
- Push và thông báo trong ứng dụng mở đúng ngữ cảnh và không làm lộ Spoiler.
- Sự kiện tương tác được ghi nhận để phục vụ thống kê.
