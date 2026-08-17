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

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra toàn bộ nội dung mới/bản sửa đi qua AI, taxonomy/risk/policy version được lưu, hai chế độ có state transition đúng và fallback không bypass kiểm duyệt.

### Rủi ro chính

- Nội dung nguy hiểm được công khai do sai ngưỡng hoặc AI timeout.
- Chế độ 2 hiển thị ngay nội dung an toàn thay vì chờ Admin.
- Retry AI tạo nhiều comment hoặc dùng sai phiên bản policy.

### Dữ liệu kiểm thử

Comment gốc, reply, bản sửa; nội dung an toàn, từng nhãn vi phạm, borderline; Chế độ 1/2; AI response success/timeout/error/duplicate; policy version P1/P2.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US11-001 | Coverage | Có comment, reply và bản sửa | Gửi từng loại qua luồng tạo/sửa | Mọi loại đều được gọi AI trước khi trở thành nội dung công khai mới. |
| TC-US11-002 | Classification | Chuẩn bị mỗi nhóm từ cấm, tục tĩu, xúc phạm, spam, quảng cáo, nhạy cảm | Gửi từng nội dung | AI trả đúng label/risk/reason và kết quả được lưu để CMS tra cứu. |
| TC-US11-003 | Audit/data | Policy P1 đang áp dụng | Gửi comment rồi kiểm tra record moderation | Lưu policy version, model/result, thời gian xử lý và lý do; quyết định tái hiện được. |
| TC-US11-004 | Mode 1/safe | Phim ở Chế độ 1; AI trả an toàn | Gửi comment | State chuyển Hiển thị theo chính sách; người khác đọc được. |
| TC-US11-005 | Mode 1/risk | Chế độ 1; AI trả nghi ngờ/rủi ro | Gửi comment | State chuyển Chờ duyệt; chỉ tác giả thấy theo US12; Admin thấy hàng chờ. |
| TC-US11-006 | Mode 1/post-review | Có comment đã Hiển thị và Chờ duyệt | Admin mở hậu kiểm và xử lý | Admin xem được cả hai nhóm; hành động làm đổi state và audit đúng. |
| TC-US11-007 | Mode 2/safe | Phim ở Chế độ 2; AI trả an toàn | Gửi comment | Không công khai ngay; vào hàng chờ Admin. |
| TC-US11-008 | Mode 2/decision | Comment ở hàng chờ Chế độ 2 | Admin duyệt rồi thử từ chối một comment khác | Chỉ comment được duyệt chuyển Hiển thị; comment bị từ chối không lộ cộng đồng. |
| TC-US11-009 | Fallback | AI timeout/5xx/không khả dụng | Gửi comment/reply | Áp dụng fallback an toàn đã cấu hình; không tự bỏ qua kiểm duyệt hoặc công khai ngoài policy. |
| TC-US11-010 | Idempotency | AI response chậm; client retry | Gửi cùng request nhiều lần | Một comment/version duy nhất; không nhân đôi queue, decision hoặc notification. |
| TC-US11-011 | Policy change | Đổi policy từ P1 sang P2 khi có request đang xử lý | Hoàn tất các request trước/sau thời điểm hiệu lực | Mỗi quyết định gắn đúng policy version; thay đổi không làm đổi ngược lịch sử ngoài quy trình. |
| TC-US11-012 | Boundary/config | Nhãn có ngưỡng an toàn, borderline, rủi ro cao | Gửi các mẫu tại ngưỡng | Hành động đúng ma trận cấu hình: che/chặn/hàng chờ; không mặc định cùng một hành động cho mọi nhãn. |

### Điểm cần PO chốt

- Taxonomy, ngưỡng risk, ngôn ngữ/biến thể ký tự và fallback cụ thể.
- SLA AI và cách xử lý false positive/false negative trong UAT.
