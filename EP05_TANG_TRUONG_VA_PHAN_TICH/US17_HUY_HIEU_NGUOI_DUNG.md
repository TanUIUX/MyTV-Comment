# US17 — Huy hiệu người dùng

> Thuộc EP05 — Tăng trưởng và phân tích
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


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

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra cấp/hiển thị/thu hồi huy hiệu theo tiêu chí, loại trừ nội dung không hợp lệ, hỗ trợ nhiều huy hiệu và bảo đảm cấu hình Admin có audit.

### Rủi ro chính

- Huy hiệu cấp dựa trên spam, nội dung Ẩn/Xóa hoặc Report chưa xác minh.
- Cấu hình tắt/bật làm ẩn sai huy hiệu cũ hoặc cấp trùng.
- Huy hiệu hiển thị sai thứ tự/số lượng hoặc thu hồi làm mất comment.

### Dữ liệu kiểm thử

U1 đạt/chưa đạt tiêu chí; comment công khai, Ẩn, Xóa, Spam; Like; nhiều loại huy hiệu; Admin cấu hình, cấp thủ công, thu hồi và role chỉ đọc.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US17-001 | Display | U1 có huy hiệu đang hiệu lực | Mở comment/reply của U1 trên web/mobile | Huy hiệu hiển thị cạnh tên đúng icon/tên theo cấu hình. |
| TC-US17-002 | Eligibility | U1 vừa đạt ngưỡng tiêu chí hợp lệ | Chạy job/sự kiện đánh giá | Huy hiệu được cấp đúng loại, lưu thời điểm và tiêu chí đạt. |
| TC-US17-003 | Negative | U1 chỉ có comment Ẩn/Xóa/Spam | Chạy đánh giá huy hiệu | Nội dung bị loại không làm tăng thành tích; U1 không được cấp nhờ dữ liệu không hợp lệ. |
| TC-US17-004 | Boundary | U1 ở dưới, đúng và trên ngưỡng | Chạy đánh giá từng mức | Dưới ngưỡng không cấp; đúng/trên ngưỡng cấp đúng theo rule. |
| TC-US17-005 | Multiple/order | U1 có nhiều huy hiệu hiệu lực | Cấp/đạt thêm huy hiệu, mở UI | Số lượng và thứ tự hiển thị đúng cấu hình; không duplicate. |
| TC-US17-006 | Admin config | Admin có quyền | Tạo/sửa tên, icon, mô tả, điều kiện; bật/tắt loại | Cấu hình được validate, áp dụng đúng từ thời điểm hiệu lực và có audit. |
| TC-US17-007 | Manual grant | Admin được cấp thủ công | Cấp huy hiệu Chuyên gia cho U1 | Huy hiệu xuất hiện đúng, ghi actor/lý do/thời gian; role không đủ quyền bị chặn. |
| TC-US17-008 | Revoke | U1 có huy hiệu | Admin thu hồi do vi phạm/cấp sai | Huy hiệu không còn hiệu lực theo policy; comment của U1 không bị xóa. |
| TC-US17-009 | Toggle behavior | Loại huy hiệu có người đang sở hữu | Tắt rồi bật lại loại huy hiệu | Ngừng cấp mới; huy hiệu cũ ẩn/giữ đúng cấu hình; bật lại không cấp trùng. |
| TC-US17-010 | Report rule | U1 có Report chưa xác minh | Tạo Report rồi chạy đánh giá | Không tự động thu hồi huy hiệu chỉ dựa trên Report chưa có kết luận. |
| TC-US17-011 | Audit | Cấp tự động/thủ công, cấu hình, thu hồi | Tra cứu audit | Mỗi thay đổi có actor/nguồn, thời gian, lý do và trạng thái trước/sau. |
| TC-US17-012 | Data integrity | U1 có huy hiệu và nhiều comment | Thu hồi huy hiệu, refresh hồ sơ/comment | Chỉ trạng thái huy hiệu thay đổi; comment, Like và lịch sử không bị mất. |

### Điểm cần PO chốt

- Ngưỡng/chu kỳ đánh giá, số huy hiệu tối đa và thông báo cấp/thu hồi.
- Chính sách giữ hay ẩn huy hiệu cũ khi tắt loại huy hiệu.
