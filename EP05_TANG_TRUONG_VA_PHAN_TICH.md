# EP05 — TĂNG TRƯỞNG VÀ PHÂN TÍCH

## 1. Thông tin Epic

| Thuộc tính | Nội dung |
|---|---|
| Epic ID | EP05 |
| Tên Epic | Tăng trưởng và phân tích |
| Mục tiêu | Khuyến khích đóng góp tích cực, mở rộng khả năng lan truyền và cung cấp dữ liệu để đội nội dung vận hành cộng đồng hiệu quả |
| Đối tượng | Người dùng tích cực; Admin vận hành; đội nội dung/marketing; quản lý sản phẩm |
| Giá trị kinh doanh | Tăng tương tác bền vững, thu hút lượt quay lại và tối ưu hoạt động nội dung dựa trên dữ liệu |
| Phạm vi | Huy hiệu, chia sẻ, dashboard và AI đề xuất nội dung vận hành |

## 2. Kết quả mong đợi

- Người dùng có đóng góp tích cực được ghi nhận rõ ràng.
- Nội dung thảo luận có thể điều hướng người xem quay lại MyTV.
- Đội vận hành biết phim/tập/chủ đề nào đang tạo tương tác.
- AI hỗ trợ tìm ứng viên nổi bật và chủ đề nhưng không tự thay thế quyết định của Admin.

## 3. Chỉ số gợi ý

- Tỷ lệ người dùng đạt/duy trì huy hiệu và chất lượng đóng góp sau khi nhận huy hiệu.
- Số lượt chia sẻ và tỷ lệ mở deep link quay lại MyTV.
- Tỷ lệ chuyển đổi từ link chia sẻ sang xem phim/bình luận.
- Tỷ lệ đề xuất AI được Admin chấp nhận.
- Mức tăng Like/Reply sau khi Admin sử dụng chủ đề gợi ý.
- Số phim/tập có tương tác cao theo tuần/tháng.

---

## US17 — Huy hiệu người dùng

### User Story

**Là người dùng tích cực**, tôi muốn được ghi nhận bằng huy hiệu phù hợp và thấy huy hiệu bên cạnh tên của mình, để có động lực tiếp tục đóng góp nội dung chất lượng.

### Giá trị

- Ghi nhận đóng góp và tạo động lực cộng đồng.
- Giúp người xem nhận biết thành viên tích cực hoặc có chuyên môn.
- Tạo công cụ vận hành chiến dịch tương tác dài hạn.

### Ưu tiên

**Could**

### Các loại huy hiệu ban đầu

| Huy hiệu | Cơ chế đề xuất |
|---|---|
| Fan tích cực/trung thành | Tự động theo số lượng đóng góp hợp lệ và tiêu chí cấu hình |
| Bình luận nổi bật | Tự động theo lượng Like/chất lượng bình luận và tiêu chí cấu hình |
| Người yêu phim/Chuyên gia phim ảnh | Admin cấp thủ công dựa trên chất lượng và hiểu biết |

### Điều kiện tiên quyết

- Người dùng có tài khoản hợp lệ.
- Dữ liệu bình luận, Like và vi phạm đủ để đánh giá tiêu chí.
- Danh mục huy hiệu được Admin bật.

### Acceptance Criteria — Hiển thị và tự động cấp

1. Huy hiệu đang hiệu lực được hiển thị cạnh tên tài khoản trong bình luận và reply.
2. Hệ thống tự đánh giá điều kiện theo lịch hoặc sự kiện được thiết kế.
3. Khi người dùng đạt tiêu chí tự động, hệ thống cấp đúng huy hiệu và lưu thời điểm/tiêu chí đạt.
4. Một người dùng có thể sở hữu nhiều huy hiệu nếu cấu hình cho phép.
5. Khi có nhiều huy hiệu, giao diện tuân theo số lượng và thứ tự hiển thị được cấu hình.
6. Nội dung bị xóa/ẩn hoặc xác định là spam không được dùng để tăng thành tích nếu chính sách loại trừ.

### Acceptance Criteria — Quản trị huy hiệu

1. Admin có thể tạo/cập nhật tên, icon, mô tả và điều kiện của huy hiệu.
2. Admin có thể bật/tắt từng loại theo từng giai đoạn.
3. Admin có thể cấp thủ công huy hiệu Chuyên gia cho tài khoản phù hợp.
4. Admin có thể thu hồi huy hiệu khi tài khoản vi phạm nghiêm trọng hoặc được cấp sai.
5. Tắt một loại huy hiệu phải có hành vi rõ ràng: ngừng cấp mới và ẩn/giữ huy hiệu cũ theo cấu hình.
6. Mọi thao tác cấp thủ công, cấu hình và thu hồi được lưu audit.

### Quy tắc nghiệp vụ

- Không nên dùng số lượng bình luận thuần túy làm tiêu chí duy nhất vì có thể khuyến khích spam.
- Tiêu chí nên chỉ tính nội dung hợp lệ, đã hiển thị và chưa bị xử lý vi phạm.
- Report chưa được xác minh không tự động làm mất huy hiệu.
- Thu hồi huy hiệu không xóa bình luận của người dùng.

### Phụ thuộc

- US07 — Like/Unlike.
- US16 — Lịch sử vi phạm và audit.
- US19 — Dữ liệu thống kê.

### Điểm cần PO chốt

- Ngưỡng ban đầu cho từng huy hiệu.
- Thời gian đánh giá: toàn thời gian hay theo chu kỳ.
- Số huy hiệu tối đa hiển thị cạnh tên.
- Người dùng có nhận thông báo khi được cấp/thu hồi hay không.

---

## US18 — Chia sẻ bình luận và cảnh phim

### User Story

**Là người dùng đã đăng nhập**, tôi muốn chia sẻ một bình luận hoặc cảnh phim đang thảo luận ra mạng xã hội với liên kết quay lại MyTV, để giới thiệu nội dung và mời người khác tham gia.

### Giá trị

- Tạo nguồn truy cập quay lại MyTV từ bên ngoài.
- Tăng khả năng lan truyền của phim và thảo luận cộng đồng.
- Kết nối hoạt động bình luận với chiến dịch mạng xã hội.

### Ưu tiên

**Could**

### Điều kiện tiên quyết

- Bình luận đang Hiển thị và được phép chia sẻ.
- Người dùng có quyền xem/chia sẻ nội dung nguồn.
- Nền tảng đích hoặc share sheet được hỗ trợ.

### Acceptance Criteria

1. Người dùng có thể chọn Chia sẻ trên một bình luận đang hiển thị.
2. Người dùng có thể chọn chia sẻ nội dung bình luận hoặc cảnh phim gắn với bình luận nếu cảnh đó đủ quyền.
3. Nội dung chia sẻ có liên kết quay lại đúng phim/tập trên MyTV.
4. Khi có thể, deep link mở đúng ngữ cảnh bình luận/thread; nếu không, mở trang phim/tập phù hợp.
5. Hệ thống hỗ trợ các kênh mục tiêu như Facebook, TikTok, Zalo hoặc share sheet của thiết bị theo khả năng tích hợp được xác minh.
6. Nội dung Spoiler không được hiển thị trực tiếp trong preview chia sẻ nếu chưa có cơ chế che phù hợp.
7. Bình luận bị ẩn/xóa sau khi chia sẻ không còn được hiển thị qua deep link; người nhận vẫn có thể được chuyển tới phim nếu còn quyền.
8. Cảnh phim chỉ được chia sẻ khi chính sách bản quyền/DRM cho phép.
9. Link chia sẻ có metadata phù hợp và không làm lộ thông tin cá nhân nhạy cảm.
10. Hệ thống ghi nhận sự kiện chia sẻ, kênh đích và lượt mở link để phục vụ phân tích trong phạm vi cho phép.

### Quy tắc nghiệp vụ

- Mọi nội dung chia sẻ phải gắn đường dẫn quay về MyTV.
- Không chia sẻ nội dung đang Chờ duyệt, Từ chối, Ẩn hoặc Xóa.
- Không mặc định coi mọi cảnh phim đều được phép chia sẻ ra ngoài.
- Việc tích hợp trực tiếp từng mạng xã hội phụ thuộc API/chính sách nền tảng; share sheet là phương án fallback.

### Phụ thuộc

- US06 — Bình luận kèm cảnh phim.
- Dịch vụ deep link và attribution.
- DRM/quản lý quyền nội dung.
- US19 — Thống kê.

### Rủi ro và điểm cần PO chốt

- Danh sách kênh MVP và mức tích hợp từng kênh.
- Mẫu nội dung/preview và watermark.
- Quyền chia sẻ theo từng phim/nhà cung cấp.
- Thời gian hiệu lực của deep link và hành vi với người chưa cài ứng dụng.

---

## US19 — Thống kê hoạt động bình luận

### User Story

**Là quản lý sản phẩm hoặc Admin vận hành**, tôi muốn theo dõi số liệu bình luận và tương tác theo phim, tập và thời gian, để đánh giá hiệu quả và ưu tiên hoạt động nội dung.

### Giá trị

- Đo lường hiệu quả của tính năng và từng nội dung.
- Giúp đội vận hành tập trung vào phim/tập có tiềm năng.
- Cung cấp dữ liệu cho quyết định mở rộng giai đoạn 2.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Các sự kiện bình luận/tương tác được ghi nhận nhất quán.
- Người xem dashboard có quyền phù hợp.
- Định nghĩa chỉ số đã được thống nhất.

### Acceptance Criteria

1. Dashboard hiển thị số bình luận theo phim, series, tập và khoảng thời gian.
2. Dashboard hiển thị số Like, Reply và Report theo cùng các chiều dữ liệu được hỗ trợ.
3. Hệ thống phân biệt số tạo mới, số đang hiển thị và số bị xử lý khi cần phân tích chất lượng.
4. Người dùng dashboard có thể chọn khoảng thời gian và lọc theo phim/tập.
5. Dashboard xác định/xếp hạng các phim và tập có mức độ tương tác cao.
6. Chỉ số được cập nhật theo độ trễ dữ liệu được công bố rõ.
7. Số liệu tổng hợp phải khớp với định nghĩa giữa dashboard và báo cáo xuất ra nếu có.
8. Quyền xem dữ liệu chi tiết và tổng hợp được phân tách; báo cáo không làm lộ thông tin cá nhân không cần thiết.
9. Sự kiện bị retry hoặc gửi trùng không làm tăng sai số liệu.
10. Dashboard hỗ trợ trạng thái không có dữ liệu và lỗi tải dữ liệu rõ ràng.

### Chỉ số tối thiểu

- Bình luận gốc mới.
- Reply mới.
- Người dùng bình luận duy nhất.
- Like/Unlike ròng và tổng thao tác Like nếu cần.
- Report và tỷ lệ Report được xác nhận vi phạm.
- Nội dung bị ẩn/xóa/từ chối.
- Tỷ lệ bình luận được AI tự động cho hiển thị.
- Thời gian xử lý hàng chờ.

### Quy tắc nghiệp vụ

- Cần có data dictionary cho từng chỉ số.
- Không cộng bình luận Chờ duyệt vào chỉ số “bình luận công khai” nếu chưa hiển thị.
- Nội dung xóa mềm vẫn có thể được tính trong chỉ số lịch sử nhưng phải phân biệt trạng thái.
- Dữ liệu cá nhân chỉ được dùng trong phạm vi được phê duyệt.

### Phụ thuộc

- Tracking từ EP01–EP04.
- Hạ tầng dữ liệu/BI của MyTV.

### Điểm cần PO chốt

- Định nghĩa “mức độ tương tác cao” và công thức engagement score.
- Độ trễ dữ liệu: real-time, gần real-time hay theo ngày.
- Nhu cầu export CSV/XLSX hoặc tích hợp công cụ BI hiện có.
- Bộ KPI dùng để quyết định mở rộng sang thể thao/thiếu nhi/giải trí.

---

## US20 — AI hỗ trợ vận hành cộng đồng

### User Story

**Là Admin/biên tập viên nội dung**, tôi muốn AI đề xuất bình luận đáng chú ý và câu hỏi thảo luận phù hợp với phim/tập, để duy trì sức sống cộng đồng mà vẫn kiểm soát được nội dung xuất bản.

### Giá trị

- Giảm thời gian tìm bình luận chất lượng.
- Hỗ trợ tạo câu hỏi gợi mở theo từng nội dung.
- Tăng khả năng xuất hiện thảo luận tích cực và có chiều sâu.

### Ưu tiên

**Could**

### Điều kiện tiên quyết

- AI có quyền truy cập dữ liệu cần thiết theo chính sách.
- Phim/tập có metadata hoặc nội dung đầu vào phù hợp.
- Admin có giao diện xem và quyết định đối với đề xuất.

### Acceptance Criteria — Đề xuất bình luận nổi bật

1. AI có thể tạo danh sách ứng viên từ các bình luận đang Hiển thị.
2. Đề xuất có thể xem xét các tín hiệu như Like, Reply, thời gian, chất lượng nội dung và mức độ liên quan.
3. AI loại trừ nội dung đang Chờ duyệt, bị Report chưa xử lý theo ngưỡng rủi ro, bị Flag nghiêm trọng, Ẩn hoặc Xóa.
4. Admin xem được lý do/tín hiệu chính khiến bình luận được đề xuất trong phạm vi giải pháp hỗ trợ.
5. AI không tự động ghim; Admin quyết định ghim qua US15.
6. Admin có thể bỏ qua đề xuất và hành động đó được ghi nhận để đánh giá chất lượng AI.

### Acceptance Criteria — Đề xuất chủ đề/câu hỏi

1. AI có thể đề xuất câu hỏi gợi mở theo series hoặc tập cụ thể.
2. Câu hỏi không tiết lộ Spoiler ngoài phạm vi được cho phép.
3. Nội dung đề xuất phải đi qua kiểm tra an toàn trước khi Admin sử dụng.
4. Admin có thể chỉnh sửa, chấp nhận hoặc loại bỏ đề xuất.
5. AI không tự đăng nội dung ra cộng đồng trong phạm vi hiện tại.
6. Hệ thống ghi nhận đề xuất được sử dụng và kết quả tương tác để đánh giá hiệu quả.

### Quy tắc nghiệp vụ

- Mô hình human-in-the-loop: AI đề xuất, con người quyết định.
- Không ưu tiên nội dung chỉ vì gây tranh cãi nếu có nguy cơ vi phạm hoặc tạo tương tác tiêu cực.
- Đề xuất phải tuân thủ chính sách nội dung và quyền dữ liệu của MyTV.
- Bình luận nổi bật do Admin ghim có ưu tiên hiển thị theo US02/US15.

### Phụ thuộc

- US11 — AI moderation để loại trừ nội dung rủi ro.
- US15 — Ghim bình luận.
- US19 — Dữ liệu tương tác và đo hiệu quả.

### Rủi ro và điểm cần PO chốt

- Tiêu chí “hay”, “hấp dẫn” và “tương tác tích cực”.
- Nguồn dữ liệu AI được phép sử dụng.
- Có cần biên tập viên duyệt hai lớp trước khi đăng câu hỏi hay không.
- Cơ chế đo chất lượng và ngừng sử dụng đề xuất không phù hợp.

---

## 4. Công việc vận hành liên quan nhưng không phải User Story

- Chuẩn bị 3–5 bình luận khởi tạo cho mỗi phim/tập.
- Lập lịch câu hỏi gợi mở và phân công Admin phụ trách phim.
- Phối hợp với nhóm mạng xã hội để lựa chọn phân cảnh tiềm năng.
- Tổ chức chiến dịch nội bộ khuyến khích tương tác.
- Xây dựng quy trình biên tập, duyệt và phản hồi đối với đề xuất AI.

Các nội dung trên nên được quản lý trong runbook/campaign backlog thay vì phát sinh thêm User Story phát triển nếu chưa cần công cụ mới.

## 5. Điều kiện hoàn thành Epic

- Huy hiệu không khuyến khích spam và có cơ chế cấu hình/thu hồi.
- Nội dung chia sẻ luôn có deep link và tuân thủ bản quyền.
- Dashboard có định nghĩa chỉ số thống nhất và dữ liệu đủ tin cậy.
- AI chỉ đề xuất, không tự ghim hoặc tự đăng trong phạm vi hiện tại.
- Có tracking để đo hiệu quả của huy hiệu, chia sẻ và đề xuất AI.
