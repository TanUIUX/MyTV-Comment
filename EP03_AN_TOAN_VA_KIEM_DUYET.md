# EP03 — AN TOÀN VÀ KIỂM DUYỆT

## 1. Thông tin Epic

| Thuộc tính | Nội dung |
|---|---|
| Epic ID | EP03 |
| Tên Epic | An toàn và kiểm duyệt |
| Mục tiêu | Hạn chế nội dung vi phạm bằng Report, AI và quy trình kiểm duyệt linh hoạt mà vẫn duy trì trải nghiệm thảo luận phù hợp |
| Đối tượng | Người dùng đã đăng nhập; Admin; hệ thống AI moderation |
| Giá trị kinh doanh | Giảm rủi ro pháp lý/thương hiệu, bảo vệ người dùng và tối ưu khối lượng vận hành |
| Phạm vi | Report, AI tiền kiểm, hai chế độ duyệt, trạng thái bình luận và cấu hình đóng/mở theo phim |

## 2. Kết quả mong đợi

- Người dùng có kênh báo cáo nội dung không phù hợp.
- Tất cả bình luận, reply và phiên bản sửa đi qua kiểm duyệt.
- Phim thông thường có thể thảo luận gần thời gian thực.
- Nội dung nhạy cảm có thể áp dụng cơ chế Admin duyệt trước.
- Admin kiểm soát được việc mở/đóng và cơ chế kiểm duyệt theo từng phim.
- Mọi chuyển đổi trạng thái quan trọng được lưu dấu vết.

## 3. Chỉ số gợi ý

- Tỷ lệ nội dung an toàn được tự động hiển thị.
- Tỷ lệ nội dung AI chuyển hàng chờ và tỷ lệ Admin xác nhận vi phạm.
- Tỷ lệ lọt vi phạm sau khi đã hiển thị.
- Thời gian trung bình xử lý hàng chờ và Report.
- Số Report hợp lệ/không hợp lệ.
- Tỷ lệ lỗi hoặc timeout của AI moderation.

---

## US10 — Report bình luận vi phạm

### User Story

**Là người dùng đã đăng nhập**, tôi muốn báo cáo một bình luận không phù hợp theo lý do cụ thể, để Admin xem xét và bảo vệ môi trường cộng đồng.

### Giá trị

- Bổ sung tín hiệu cộng đồng cho quá trình kiểm duyệt.
- Giúp phát hiện các nội dung AI bỏ sót.
- Tạo cơ chế phản hồi minh bạch cho người dùng.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Người dùng đã đăng nhập và còn quyền tương tác.
- Bình luận/reply đang tồn tại và người dùng có quyền xem.

### Acceptance Criteria

1. Người dùng có thể chọn Report trên một bình luận hoặc reply đang hiển thị.
2. Hệ thống hiển thị các lý do: Spoiler, Spam/quảng cáo, Xúc phạm, Nội dung không phù hợp, Sai thông tin và Khác.
3. Khi chọn “Khác”, hệ thống cho phép nhập mô tả bổ sung theo giới hạn được cấu hình.
4. Người dùng phải chọn ít nhất một lý do hợp lệ trước khi gửi.
5. Sau khi gửi thành công, hệ thống xác nhận đã tiếp nhận Report.
6. Report được liên kết với người báo cáo, bình luận, phiên bản nội dung, lý do và thời gian gửi.
7. Report được chuyển vào CMS để Admin xử lý.
8. Bình luận vẫn hiển thị sau khi nhận Report cho đến khi CMS hoặc cơ chế kiểm duyệt đưa ra quyết định khác.
9. Số lượng Report lớn không tự động ẩn/xóa bình luận theo quyết định hiện tại.
10. Hệ thống ngăn một tài khoản gửi lặp lại cùng một Report cho cùng một bình luận nếu chưa có chính sách cho phép báo cáo lại.
11. Người dùng không nhìn thấy danh tính người đã Report một bình luận.
12. Người chưa đăng nhập chọn Report được chuyển sang luồng đăng nhập.

### Quy tắc nghiệp vụ

- Report là tín hiệu để Admin đánh giá, không phải kết luận vi phạm.
- Không công khai số Report trên giao diện người xem.
- Dữ liệu Report phải được giữ cùng lịch sử xử lý để audit.
- Cần có rate limit để tránh lạm dụng chức năng Report.

### Phụ thuộc

- US14 — Xử lý nội dung trên CMS.
- US16 — Quản lý người dùng vi phạm và audit log.

### Điểm cần PO chốt

- Có thông báo kết quả xử lý cho người Report hay không.
- Thời gian cho phép báo cáo lại sau khi Admin bỏ qua Report.
- Có cho phép Report bình luận của chính mình hay không.

---

## US11 — AI kiểm duyệt theo hai chế độ

### User Story

**Là bộ phận vận hành MyTV**, tôi muốn AI kiểm tra nội dung và hỗ trợ hai chế độ kiểm duyệt, để cân bằng giữa tốc độ hiển thị và mức độ an toàn theo từng loại phim.

### Giá trị

- Giảm khối lượng kiểm duyệt thủ công.
- Cho phép thảo luận gần thời gian thực với nội dung thông thường.
- Tăng mức kiểm soát cho phim hoặc giai đoạn nhạy cảm.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Bộ chính sách kiểm duyệt và taxonomy vi phạm đã được phê duyệt.
- Dịch vụ AI moderation có khả năng trả về kết quả và mức độ rủi ro.
- Phim đã có cấu hình chế độ kiểm duyệt.

### Phạm vi nội dung kiểm tra

- Bình luận gốc.
- Reply.
- Phiên bản bình luận/reply sau chỉnh sửa.
- Nội dung text, emoji, nickname/mention và metadata liên quan trong phạm vi giải pháp hỗ trợ.

### Acceptance Criteria chung

1. Mọi bình luận/reply/phiên bản sửa được gửi qua AI trước khi trở thành nội dung công khai mới.
2. AI kiểm tra tối thiểu các nhóm: từ khóa cấm, tục tĩu, xúc phạm, spam, quảng cáo, nội dung nhạy cảm, nội dung không phù hợp và các nhóm chính sách được cấu hình.
3. AI trả về kết quả phân loại, mức độ rủi ro, lý do/nhãn và thời gian xử lý để CMS có thể tra cứu.
4. Hệ thống áp dụng cùng một phiên bản chính sách cho việc ra quyết định và lưu lại phiên bản đó phục vụ audit.
5. Khi AI timeout hoặc không khả dụng, hệ thống áp dụng phương án fallback an toàn đã được cấu hình; không tự động bỏ qua kiểm duyệt.
6. Hệ thống chống gửi lặp khiến cùng một bình luận được tạo nhiều lần khi AI hoặc mạng retry.

### Acceptance Criteria — Chế độ 1: AI tiền kiểm, hiển thị ngay, Admin hậu kiểm

1. Nội dung được AI xác định an toàn chuyển sang trạng thái Hiển thị.
2. Nội dung nghi ngờ hoặc rủi ro chuyển sang trạng thái Chờ duyệt.
3. Nội dung Chờ duyệt chỉ hiển thị với tác giả theo US12.
4. Admin có thể hậu kiểm cả nội dung đã hiển thị và nội dung chờ duyệt.

### Acceptance Criteria — Chế độ 2: AI tiền lọc, Admin duyệt trước

1. Nội dung sau khi AI kiểm tra không được công khai ngay.
2. Nội dung hợp lệ về mặt kỹ thuật được chuyển vào hàng chờ Admin.
3. Chỉ sau khi Admin duyệt, nội dung mới chuyển sang Hiển thị.
4. Khi Admin từ chối, nội dung không hiển thị với cộng đồng và tác giả nhận được trạng thái phù hợp nếu sản phẩm hỗ trợ thông báo kết quả.

### Quy tắc nghiệp vụ

- Chế độ 1 là mặc định; chế độ 2 áp dụng cho trường hợp đặc biệt.
- AI hỗ trợ ra quyết định nhưng Admin có quyền xử lý theo chính sách.
- Ngưỡng AI và hành động theo từng nhãn phải cấu hình được.
- Quyết định che từ bằng `***`, chặn ngay hoặc chuyển hàng chờ phải được xác định bằng ma trận chính sách; không mặc định áp dụng giống nhau cho mọi loại vi phạm.

### Phụ thuộc

- Dịch vụ AI moderation.
- US12 — Trạng thái và phạm vi hiển thị.
- US14 — Xử lý nội dung trên CMS.
- US16 — Audit log.

### Rủi ro và điểm cần PO chốt

- Taxonomy, ngưỡng rủi ro và ngôn ngữ/ký tự biến thể cần hỗ trợ.
- Fallback khi AI lỗi: tất cả vào hàng chờ hay tạm ngừng nhận bình luận.
- SLA tối đa của AI để không ảnh hưởng trải nghiệm.
- Quy tắc xử lý false positive/false negative và vòng phản hồi để cải thiện mô hình.

---

## US12 — Quản lý trạng thái và phạm vi hiển thị bình luận

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

## 4. Điều kiện hoàn thành Epic

- Report được chuyển đúng vào CMS và không tự động làm ẩn bình luận ngoài chính sách.
- Hai chế độ AI hoạt động đúng và có fallback an toàn.
- Không có nội dung Chờ duyệt bị lộ cho người dùng khác.
- Phiên bản cũ được giữ công khai đúng trong quá trình kiểm duyệt bản sửa.
- Đóng bình luận ẩn đúng toàn bộ khu vực và không xóa dữ liệu.
- Mọi quyết định/trạng thái quan trọng có audit và có thể tra cứu trên CMS.
