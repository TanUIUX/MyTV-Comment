# US11 — AI kiểm duyệt theo hai chế độ

> Thuộc EP03 — An toàn và kiểm duyệt
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


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

