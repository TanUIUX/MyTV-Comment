# US03 — Đánh giá series và tập phim

> Thuộc EP01 — Trải nghiệm bình luận và đánh giá
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là người dùng đã đăng nhập**, tôi muốn đánh giá series hoặc tập phim bằng thang điểm 5 sao và có thể thay đổi đánh giá, để thể hiện cảm nhận của mình với cộng đồng.

### Giá trị

- Cung cấp tín hiệu chất lượng nội dung cho người xem khác.
- Tạo thêm hình thức tương tác nhẹ bên cạnh bình luận.
- Cung cấp dữ liệu phục vụ phân tích và đề xuất nội dung.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Nội dung được phép đánh giá.
- Người dùng đã đăng nhập khi gửi hoặc thay đổi đánh giá.

### Acceptance Criteria

1. Đầu khu vực Bình luận hiển thị thang điểm 5 sao, điểm trung bình và tổng số lượt đánh giá.
2. Người chưa đăng nhập xem được điểm trung bình và tổng lượt đánh giá.
3. Khi người chưa đăng nhập chọn đánh giá, hệ thống yêu cầu đăng nhập.
4. Mỗi tài khoản chỉ có một đánh giá hiện hành trên mỗi series và một đánh giá hiện hành trên mỗi tập.
5. Đánh giá ở cấp series và đánh giá ở cấp tập là hai bản ghi độc lập.
6. Người dùng có thể thay đổi đánh giá đã gửi mà không tạo thêm lượt đánh giá mới.
7. Khi có đánh giá mới hoặc thay đổi đánh giá, điểm trung bình và tổng lượt được tính lại tự động.
8. Sau khi gửi thành công, giao diện hiển thị rõ số sao hiện tại của người dùng.
9. Nếu gửi thất bại, hệ thống giữ trạng thái cũ và thông báo để người dùng thử lại.

### Quy tắc nghiệp vụ

- Giá trị hợp lệ từ 1 đến 5 sao.
- Hệ thống phải chống gửi trùng do người dùng bấm nhiều lần hoặc lỗi mạng.
- Quy tắc làm tròn điểm trung bình cần thống nhất trên các nền tảng.

### Phụ thuộc

- Hệ thống tài khoản MyTV.
- Dịch vụ nội dung định danh series/tập.

### Điểm cần PO chốt

- Số chữ số thập phân của điểm trung bình.
- Có loại trừ đánh giá của tài khoản bị khóa/vi phạm hay không.

---

## Phân tích kiểm thử

### Mục tiêu

Xác nhận điểm trung bình, tổng lượt đánh giá và bản ghi đánh giá của người dùng đúng độc lập ở cấp series/tập, đồng thời thao tác gửi lại có tính idempotent.

### Rủi ro chính

- Gộp nhầm rating series với rating tập.
- Thay đổi rating làm tăng tổng lượt hoặc tính sai trung bình.
- Retry/bấm nhiều lần tạo nhiều bản ghi hoặc làm sai điểm.

### Dữ liệu kiểm thử

Series S1, tập E1; hai tài khoản U1/U2; dữ liệu ban đầu có điểm trung bình và tổng lượt; phiên khách, phiên đăng nhập và tình huống API lỗi.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US03-001 | Functional | Nội dung có rating | Mở khu vực Bình luận bằng phiên khách | Hiển thị thang 5 sao, điểm trung bình và tổng lượt đánh giá hiện tại. |
| TC-US03-002 | Authentication | Phiên khách | Chọn một mức sao | Hiển thị yêu cầu đăng nhập; không tạo rating mới. |
| TC-US03-003 | Boundary | U1 đã đăng nhập | Gửi lần lượt 1 sao và 5 sao ở hai bản ghi dữ liệu độc lập | Giá trị biên 1 và 5 được chấp nhận và lưu đúng. |
| TC-US03-004 | Negative/API | U1 đã đăng nhập | Gửi 0, 6, số âm, số thập phân hoặc giá trị không phải số qua API | Request bị từ chối; không thay đổi điểm/tổng lượt. |
| TC-US03-005 | Data isolation | S1 và E1 đều cho phép rating | U1 đánh giá S1 rồi E1 | Tạo hai rating độc lập; điểm ở S1 không làm thay đổi điểm của E1. |
| TC-US03-006 | Update | U1 đã có rating 3 sao | Đổi rating thành 5 sao | Chỉ bản ghi của U1 được cập nhật; tổng lượt không tăng; điểm trung bình tính lại đúng. |
| TC-US03-007 | Idempotency | Request gửi chậm hoặc retry cùng idempotency key | Bấm gửi nhiều lần/đẩy lại request | Chỉ có một kết quả hợp lệ; UI không nhân đôi lượt hoặc bản ghi. |
| TC-US03-008 | Error handling | Mock lỗi mạng/500 khi gửi rating | Gửi hoặc đổi rating | Trạng thái cũ được giữ; hiển thị lỗi và cho phép thử lại; không có bản ghi nửa chừng. |

### Điểm cần PO chốt trước khi nghiệm thu số học

- Số chữ số thập phân và quy tắc làm tròn điểm trung bình.
- Có loại trừ rating của tài khoản bị khóa/vi phạm khỏi dữ liệu công khai hay không.
