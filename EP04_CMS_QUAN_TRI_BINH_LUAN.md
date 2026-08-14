# EP04 — CMS QUẢN TRỊ BÌNH LUẬN

## 1. Thông tin Epic

| Thuộc tính | Nội dung |
|---|---|
| Epic ID | EP04 |
| Tên Epic | CMS quản trị bình luận |
| Mục tiêu | Cung cấp cho Admin một không gian làm việc tập trung để tra cứu, kiểm duyệt, cấu hình và xử lý người dùng vi phạm |
| Đối tượng | Admin kiểm duyệt; Admin vận hành; quản lý có quyền xem báo cáo/audit |
| Giá trị kinh doanh | Giảm thời gian xử lý, đảm bảo thực thi chính sách nhất quán và minh bạch hóa hoạt động quản trị |
| Phạm vi | Danh sách/tìm kiếm/lọc, thao tác nội dung, Report/Flag/Spoiler, ghim, cấu hình theo phim, xử lý tài khoản và audit log |

## 2. Kết quả mong đợi

- Admin tìm được đúng bình luận cần xử lý trong thời gian ngắn.
- Admin thấy đủ ngữ cảnh trước khi ra quyết định.
- Các hành động kiểm duyệt và xử lý tài khoản được phân quyền, xác nhận và lưu dấu vết.
- Cấu hình mở/đóng và cơ chế kiểm duyệt được áp dụng chính xác theo phim.
- CMS hỗ trợ vận hành hàng ngày mà không cần can thiệp trực tiếp vào dữ liệu.

## 3. Chỉ số gợi ý

- Thời gian trung bình từ lúc bình luận vào hàng chờ đến khi được xử lý.
- Thời gian trung bình xử lý Report.
- Số bình luận xử lý trên mỗi Admin/ca vận hành.
- Tỷ lệ quyết định bị thay đổi hoặc hoàn tác.
- Tỷ lệ thao tác CMS có audit đầy đủ.
- Số lỗi cấu hình theo phim.

---

## US13 — Tra cứu bình luận trên CMS

### User Story

**Là Admin**, tôi muốn xem, tìm kiếm và lọc toàn bộ bình luận theo nhiều tiêu chí, để nhanh chóng xác định nội dung cần kiểm duyệt hoặc điều tra.

### Giá trị

- Giảm thời gian tìm kiếm thủ công.
- Hỗ trợ xử lý hàng chờ và sự cố theo đúng ngữ cảnh.
- Tạo một nguồn dữ liệu quản trị thống nhất.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Admin đã đăng nhập CMS và có quyền phù hợp.
- Dữ liệu bình luận và metadata được đồng bộ với CMS.

### Acceptance Criteria

1. Admin xem được danh sách bình luận và reply trên toàn bộ phạm vi được phân quyền.
2. Mỗi bản ghi hiển thị tối thiểu: nội dung, tác giả, phim/series, tập, thời gian đăng, trạng thái, số Like, số Reply và số Report.
3. Admin tìm kiếm được theo từ khóa nội dung.
4. Admin tìm kiếm được theo tài khoản/nickname hoặc định danh hợp lệ được phép xem.
5. Admin lọc được theo phim, tập và khoảng thời gian.
6. Admin lọc được theo trạng thái: Hiển thị, Chờ duyệt, Bị Report, Có Spoiler, Spam/nhãn tương ứng, Ẩn, Từ chối và Đã xử lý theo mô hình dữ liệu thực tế.
7. Admin có thể kết hợp nhiều bộ lọc và xóa điều kiện lọc để quay về danh sách mặc định.
8. Kết quả có phân trang hoặc tải thêm và không lặp/bỏ sót bản ghi.
9. Admin mở được màn hình chi tiết để xem thread, phiên bản nội dung, kết quả AI, Report/Flag và lịch sử xử lý liên quan.
10. Khi một bình luận gốc có reply, Admin xem được toàn bộ ngữ cảnh thread trước khi xử lý.
11. Dữ liệu nhạy cảm chỉ hiển thị cho vai trò được cấp quyền; CMS không mặc định công khai đầy đủ thông tin thuê bao.
12. Trạng thái mới được phản ánh trong danh sách theo độ trễ vận hành được thống nhất.

### Quy tắc nghiệp vụ

- Quyền xem dữ liệu phải giới hạn theo vai trò/phạm vi của Admin.
- Tìm kiếm và lọc không làm thay đổi dữ liệu.
- Danh sách mặc định nên ưu tiên nội dung cần xử lý theo SLA, nhưng quy tắc cụ thể cần PO/vận hành chốt.

### Phụ thuộc

- Mô hình dữ liệu bình luận, trạng thái và phiên bản.
- Hệ thống phân quyền CMS.
- US11, US12 về nhãn AI và trạng thái.

### Điểm cần PO chốt

- Danh sách mặc định ưu tiên Chờ duyệt, Report hay thời gian mới nhất.
- Nhu cầu export dữ liệu và giới hạn dữ liệu được export.
- Phạm vi dữ liệu cá nhân từng vai trò Admin được xem.

---

## US14 — Xử lý nội dung trên CMS

### User Story

**Là Admin kiểm duyệt**, tôi muốn duyệt, từ chối, ẩn, xóa, xử lý Report/Flag và điều chỉnh Spoiler, để thực thi chính sách cộng đồng một cách nhất quán.

### Giá trị

- Cho phép xử lý toàn bộ vòng đời kiểm duyệt tại một nơi.
- Giảm rủi ro nội dung vi phạm tiếp tục hiển thị.
- Giữ lại bằng chứng và lý do cho việc kiểm tra sau này.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Admin có quyền kiểm duyệt tương ứng.
- Bình luận/reply tồn tại và chưa bị xóa vĩnh viễn.

### Acceptance Criteria — Hành động kiểm duyệt

1. Admin có thể Duyệt nội dung đang Chờ duyệt để chuyển sang Hiển thị.
2. Admin có thể Từ chối nội dung đang Chờ duyệt để nội dung không được công khai.
3. Admin có thể Ẩn nội dung đang Hiển thị mà không xóa dữ liệu audit.
4. Admin có thể Xóa mềm bình luận/reply vi phạm sau bước xác nhận.
5. Khi Admin xóa bình luận gốc, toàn bộ thread không còn hiển thị công khai.
6. Mỗi thao tác yêu cầu lý do theo danh mục được cấu hình hoặc ghi chú bổ sung khi cần.
7. CMS kiểm tra lại trạng thái hiện tại trước khi lưu để tránh hai Admin ghi đè quyết định của nhau mà không cảnh báo.

### Acceptance Criteria — Report

1. Admin xem được danh sách Report, người gửi, lý do, thời gian và phiên bản nội dung bị báo cáo.
2. Admin có thể chọn Bỏ qua Report nếu bình luận không vi phạm.
3. Admin có thể Duyệt giữ nguyên, Ẩn hoặc Xóa nội dung tùy kết luận.
4. Hệ thống lưu lịch sử từng Report và kết quả xử lý.
5. Nhiều Report không tự động thay đổi trạng thái hiển thị trước quyết định CMS.

### Acceptance Criteria — Flag

1. Admin có thể gắn cờ nội bộ với các lý do: Spoiler, Spam/quảng cáo, Ngôn từ xúc phạm, Nội dung không phù hợp và Vi phạm khác.
2. Admin có thể cập nhật trạng thái theo dõi và bỏ cờ khi hoàn tất.
3. Flag chỉ phục vụ vận hành nội bộ và không hiển thị cho người dùng.
4. Hệ thống lưu người gắn cờ, thời gian, lý do và người hoàn tất xử lý.

### Acceptance Criteria — Spoiler

1. Admin có thể thêm trạng thái Spoiler cho bình luận chưa được tác giả đánh dấu.
2. Admin có thể bỏ trạng thái Spoiler nếu xác định không cần thiết.
3. Thay đổi được phản ánh trên ứng dụng người xem và lưu audit.
4. Nội dung bị Admin đánh dấu Spoiler được che theo cùng trải nghiệm với Spoiler do người dùng chọn.

### Quy tắc nghiệp vụ

- Admin xử lý theo chính sách cộng đồng, không dựa trên quan điểm cá nhân.
- Ý kiến trái chiều nhưng hợp lệ không bị xóa chỉ vì tiêu cực.
- Hành động phải được phân quyền và lưu audit.
- Xóa tại CMS là xóa mềm trong thời hạn lưu trữ 90 ngày, trừ quy trình xóa vĩnh viễn được phê duyệt riêng.

### Phụ thuộc

- US10 — Report bình luận.
- US11 và US12 — AI và trạng thái.
- US16 — Audit log và xử lý tài khoản.

### Điểm cần PO chốt

- Danh mục lý do chuẩn cho từng hành động.
- Có hỗ trợ thao tác hàng loạt hay chỉ từng bình luận trong MVP.
- Có thông báo cho tác giả khi nội dung bị từ chối/ẩn/xóa hay không.
- Quy trình hoàn tác một quyết định sai.

---

## US15 — Quản lý bình luận nổi bật và cấu hình theo phim

### User Story

**Là Admin vận hành nội dung**, tôi muốn ghim bình luận nổi bật và cấu hình trạng thái/cơ chế kiểm duyệt theo từng phim, để định hướng thảo luận và áp dụng mức kiểm soát phù hợp.

### Giá trị

- Làm nổi bật nội dung thảo luận có chất lượng.
- Cho phép phản ứng nhanh với nội dung hoặc giai đoạn nhạy cảm.
- Giảm phụ thuộc vào cấu hình kỹ thuật thủ công.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Admin có quyền vận hành nội dung.
- Phim đã tồn tại trong CMS và liên kết đúng với dịch vụ bình luận.

### Acceptance Criteria — Bình luận nổi bật

1. Admin có thể ghim một bình luận đang Hiển thị lên khu vực Nổi bật.
2. Admin có thể bỏ ghim hoặc thay thế bình luận đã ghim.
3. Hệ thống giới hạn số lượng bình luận ghim theo cấu hình; giá trị đề xuất mặc định là ba.
4. Khi đạt giới hạn, CMS yêu cầu Admin bỏ ghim/thay thế trước khi thêm mới hoặc thực hiện theo UX đã duyệt.
5. Bình luận bị ẩn hoặc xóa tự động không còn được ghim công khai.
6. Thứ tự các bình luận ghim được áp dụng nhất quán trên web/mobile.
7. Mọi thao tác ghim/bỏ ghim được lưu audit.

### Acceptance Criteria — Cấu hình theo phim

1. Admin có thể chọn Mở hoặc Đóng khu vực Bình luận cho từng phim.
2. Khi Mở, Admin chọn Chế độ 1 hoặc Chế độ 2 theo định nghĩa tại US11.
3. Khi Đóng, toàn bộ khu vực Bình luận bị ẩn theo US12 và hệ thống ngừng nhận tương tác mới.
4. Admin thấy rõ trạng thái hiện hành và thời điểm hiệu lực của cấu hình.
5. Admin có thể cấu hình mốc thời gian chuyển sang trạng thái “Chờ duyệt — chỉ tác giả thấy” nếu nghiệp vụ sử dụng quy tắc sau X giờ.
6. Hệ thống cảnh báo tác động trước khi Admin thay đổi cấu hình đang áp dụng cho phim có nhiều tương tác.
7. Thay đổi cấu hình được truyền tới ứng dụng và API trong độ trễ được thống nhất.
8. Toàn bộ lịch sử cấu hình được lưu với người thao tác và thời gian.

### Quy tắc nghiệp vụ

- Nổi bật là chế độ sắp xếp mặc định trên ứng dụng.
- AI có thể đề xuất ứng viên nổi bật tại US20, nhưng Admin là người quyết định ghim trong phạm vi hiện tại.
- Đóng bình luận không xóa bình luận cũ.
- Cấu hình áp dụng theo phim; khả năng ghi đè ở cấp tập cần được coi là phạm vi bổ sung nếu PO yêu cầu.

### Phụ thuộc

- US02 — Hiển thị và sắp xếp.
- US11, US12 — Chế độ kiểm duyệt và trạng thái.
- US20 — AI hỗ trợ vận hành cộng đồng.

### Điểm cần PO chốt

- Có cho phép sắp xếp thủ công thứ tự ba bình luận ghim hay theo thời gian ghim.
- Có cần ngày hết hạn ghim.
- Mốc “X giờ” được tính từ sự kiện nào.
- Có cần cấu hình riêng theo tập phim trong tương lai.

---

## US16 — Quản lý người dùng vi phạm và audit log

### User Story

**Là Admin có thẩm quyền**, tôi muốn xem lịch sử vi phạm, áp dụng chế tài và tra cứu audit log, để xử lý tài khoản nhất quán và có thể kiểm chứng.

### Giá trị

- Hạn chế tái phạm và bảo vệ cộng đồng.
- Tạo bằng chứng minh bạch cho kiểm tra nội bộ/khiếu nại.
- Kiểm soát việc sử dụng quyền Admin.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Admin đã được cấp quyền xử lý tài khoản hoặc xem audit.
- Dữ liệu vi phạm và lịch sử kiểm duyệt được liên kết với tài khoản.

### Acceptance Criteria — Lịch sử và chế tài

1. Admin xem được lịch sử bình luận bị xử lý, Report liên quan, cảnh báo và chế tài trước đó của tài khoản.
2. Admin có thể gửi cảnh báo theo mẫu/lý do được cấu hình.
3. Admin có thể hạn chế quyền bình luận trong một khoảng thời gian xác định.
4. Admin có thể khóa tạm thời hoặc khóa vĩnh viễn theo quyền được cấp và chính sách phê duyệt.
5. CMS yêu cầu lý do, thời hạn và bước xác nhận đối với chế tài ảnh hưởng tài khoản.
6. Khi quyền bình luận bị hạn chế, người dùng không thể đăng, reply, Like, Mention, Report hoặc thực hiện các tương tác bị cấu hình chặn qua giao diện/API.
7. Khi hết hạn hạn chế tạm thời, quyền được khôi phục tự động nếu không có chế tài khác còn hiệu lực.
8. Việc thu hồi huy hiệu khi vi phạm nghiêm trọng có thể được thực hiện qua US17.
9. Các hành động không được làm mất dữ liệu lịch sử cần thiết cho audit.

### Acceptance Criteria — Audit log

1. Hệ thống lưu tối thiểu: Admin thực hiện, thời gian, đối tượng, hành động, lý do, trạng thái trước và trạng thái sau.
2. Audit bao phủ duyệt/từ chối/ẩn/xóa, Report, Flag, Spoiler, ghim, cấu hình phim, cảnh báo và chế tài tài khoản.
3. Admin được phân quyền có thể tìm kiếm/lọc audit theo thời gian, người thao tác, loại hành động và đối tượng.
4. Audit log không thể bị sửa/xóa bởi vai trò vận hành thông thường.
5. Dữ liệu bình luận xóa mềm được giữ 90 ngày theo quyết định hiện tại; audit log tuân theo thời hạn lưu trữ riêng do chính sách dữ liệu quy định.
6. Việc truy cập dữ liệu nhạy cảm trong audit cũng được kiểm soát và ghi nhận khi cần.

### Quy tắc nghiệp vụ

- Chế tài phải tương xứng mức độ vi phạm và theo ma trận chính sách.
- Admin chỉ sử dụng dữ liệu cho mục đích vận hành được phê duyệt.
- Cần phân biệt khóa quyền cộng đồng với khóa toàn bộ tài khoản/thuê bao MyTV.
- Không thu hồi huy hiệu hoặc khóa tài khoản tự động chỉ dựa trên một Report chưa được xác minh.

### Phụ thuộc

- Hệ thống tài khoản và phân quyền MyTV.
- US10 — Report.
- US14 — Xử lý nội dung.
- US17 — Huy hiệu.

### Điểm cần PO chốt

- Ma trận vi phạm → chế tài.
- Phạm vi “khóa tài khoản”: quyền bình luận hay toàn bộ tài khoản MyTV.
- Thời hạn lưu audit theo yêu cầu pháp lý/nội bộ.
- Quy trình khiếu nại và hoàn tác chế tài.

---

## 4. Điều kiện hoàn thành Epic

- CMS hỗ trợ đầy đủ tra cứu, xử lý, cấu hình và quản lý vi phạm theo quyền.
- Admin luôn thấy đủ ngữ cảnh trước khi ra quyết định.
- Không có thao tác quản trị quan trọng thiếu audit.
- Xung đột thao tác đồng thời được phát hiện hoặc ngăn chặn.
- Dữ liệu cá nhân chỉ hiển thị cho vai trò phù hợp.
- Cấu hình trên CMS được phản ánh đúng trên ứng dụng người dùng.
