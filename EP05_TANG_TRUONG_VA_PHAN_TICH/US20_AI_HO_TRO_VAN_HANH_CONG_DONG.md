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

