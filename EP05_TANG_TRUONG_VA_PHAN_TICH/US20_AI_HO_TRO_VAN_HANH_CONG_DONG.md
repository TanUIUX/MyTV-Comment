# US20 — AI hỗ trợ vận hành cộng đồng

> Thuộc EP05 — Tăng trưởng và phân tích
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


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

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra AI chỉ đề xuất ứng viên/chủ đề đủ điều kiện, giải thích được tín hiệu, loại trừ nội dung rủi ro và luôn để Admin quyết định ghim/đăng.

### Rủi ro chính

- AI đề xuất Chờ duyệt/Ẩn/Xóa hoặc nội dung có Report/Flag rủi ro.
- AI tự động ghim/đăng mà không có human-in-the-loop.
- Câu hỏi chứa Spoiler, thông tin nhạy cảm hoặc ưu tiên nội dung gây tranh cãi.

### Dữ liệu kiểm thử

Comment công khai có Like/Reply cao/thấp; comment Chờ duyệt, Report chưa xử lý, Flag nghiêm trọng, Ẩn, Xóa; metadata series/tập; câu hỏi có/không Spoiler.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US20-001 | Recommendation | Có nhiều comment công khai | Chạy tạo ứng viên theo series/tập | AI trả danh sách ứng viên từ nội dung Hiển thị, có liên kết đúng phim/tập. |
| TC-US20-002 | Ranking signal | Comment khác nhau về Like, Reply, thời gian, liên quan | Kiểm tra output và rationale | Đề xuất có thể hiện tín hiệu chính trong phạm vi hỗ trợ; không chỉ dựa số Like nếu policy yêu cầu chất lượng. |
| TC-US20-003 | Exclusion | Có Chờ duyệt, Report chưa xử lý rủi ro, Flag nghiêm trọng, Ẩn, Xóa | Chạy AI | Các nội dung bị loại không xuất hiện trong ứng viên được đề xuất. |
| TC-US20-004 | Explainability | Có ứng viên được đề xuất | Admin mở chi tiết | Admin thấy lý do/tín hiệu chính, source và thời điểm dữ liệu đủ để đánh giá. |
| TC-US20-005 | Human-in-loop/pin | Có ứng viên | Bỏ qua một ứng viên; thử kiểm tra trạng thái ghim | AI không tự ghim; chỉ thao tác Admin qua US15 mới thay đổi trạng thái. |
| TC-US20-006 | Feedback | Admin bỏ qua/chấp nhận đề xuất | Thực hiện hành động và kiểm tra log | Hành động được ghi nhận để đánh giá chất lượng; không làm thay đổi comment ngoài lựa chọn. |
| TC-US20-007 | Question generation | Series/tập có metadata hợp lệ | Yêu cầu AI tạo câu hỏi | Câu hỏi gắn đúng scope, phù hợp nội dung và không tự đăng. |
| TC-US20-008 | Spoiler safety | Metadata/câu hỏi có khả năng tiết lộ tình tiết | Chạy safety check, xem preview | Câu hỏi bị chặn/chỉnh sửa/cảnh báo theo policy; không lộ Spoiler ngoài phạm vi cho phép. |
| TC-US20-009 | Editing workflow | Có câu hỏi đề xuất | Admin chỉnh sửa, chấp nhận, loại bỏ | Mỗi hành động tạo đúng trạng thái; câu hỏi chỉ được dùng sau bước kiểm soát bắt buộc. |
| TC-US20-010 | No auto-publish | AI trả đề xuất hợp lệ | Không thao tác Admin, theo dõi cộng đồng | Không có comment/câu hỏi tự xuất hiện trong trải nghiệm người dùng. |
| TC-US20-011 | Tracking | Đề xuất được sử dụng và có tương tác sau đó | Kiểm tra event/metric | Ghi nhận đề xuất được dùng, kết quả tương tác và liên kết về source; không duplicate do retry. |
| TC-US20-012 | Privacy/safety | AI không được truy cập dữ liệu ngoài scope | Kiểm tra input/output và role Admin | AI chỉ dùng dữ liệu được phê duyệt; không trả PII hoặc ưu tiên nội dung gây hại/chỉ vì tranh cãi. |

### Điểm cần PO chốt

- Tiêu chí “hay/tích cực”, dữ liệu đầu vào, quy trình duyệt một/hai lớp và metric đánh giá chất lượng AI.
