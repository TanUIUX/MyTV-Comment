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

