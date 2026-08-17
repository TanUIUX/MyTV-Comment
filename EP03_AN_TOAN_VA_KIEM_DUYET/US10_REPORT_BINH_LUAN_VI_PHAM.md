# US10 — Report bình luận vi phạm

> Thuộc EP03 — An toàn và kiểm duyệt
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


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

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra người dùng gửi Report đúng lý do cho comment/reply, dữ liệu được lưu và chuyển CMS, nhưng Report không tự động thay đổi trạng thái hiển thị.

### Rủi ro chính

- Thiếu lý do hoặc mất liên kết người báo cáo/phiên bản nội dung.
- Một tài khoản spam Report để làm sai tín hiệu.
- Tự động ẩn/xóa khi nhiều Report trái với quyết định nghiệp vụ.

### Dữ liệu kiểm thử

U1/U2 đăng nhập, phiên khách; C1/R1 công khai; đủ 6 lý do; mô tả “Khác” rỗng/quá dài/hợp lệ; Report trùng và Report số lượng lớn.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US10-001 | Functional | U1 đăng nhập; C1 và R1 công khai | Mở menu Report trên C1 rồi R1 | Cả comment gốc và reply đều có thể mở luồng Report. |
| TC-US10-002 | Completeness | Có danh sách lý do chuẩn | Mở form Report | Hiển thị đủ Spoiler, Spam/quảng cáo, Xúc phạm, Không phù hợp, Sai thông tin, Khác. |
| TC-US10-003 | Validation | Chọn “Khác” | Gửi với mô tả rỗng, đúng giới hạn và vượt giới hạn | Mô tả hợp lệ được nhận; dữ liệu rỗng/quá dài bị chặn theo rule cấu hình. |
| TC-US10-004 | Negative | Chưa chọn lý do | Bấm Gửi | Hệ thống yêu cầu chọn lý do; không tạo Report. |
| TC-US10-005 | Data integrity | U1 Report C1 phiên bản V1 | Gửi Report thành công, kiểm tra CMS/database | Có account ID người báo, comment/reply ID, version ID, lý do, thời gian và trạng thái xử lý. |
| TC-US10-006 | Integration | CMS có quyền xử lý Report | Gửi Report rồi mở hàng chờ CMS | Report xuất hiện đúng nội dung, lý do và phiên bản; có thể chuyển cho US14 xử lý. |
| TC-US10-007 | Visibility | C1 đang Hiển thị | Gửi một hoặc nhiều Report | C1 vẫn hiển thị và không thay đổi trạng thái chỉ vì nhận Report. |
| TC-US10-008 | Deduplication | U1 đã Report C1 và chưa có chính sách báo lại | Gửi lại cùng Report | Request trùng bị chặn/thông báo phù hợp; không tạo bản ghi duplicate. |
| TC-US10-009 | Privacy | U1/U2 cùng xem C1 | U2 mở C1 sau khi U1 Report | Không hiển thị danh tính hoặc số lượng Report cho người xem. |
| TC-US10-010 | Authentication/rate limit | Phiên khách hoặc U1 vượt ngưỡng | Chọn Report/gửi liên tiếp qua UI/API | Khách được chuyển đăng nhập; request vượt rate limit bị chặn và không làm sai dữ liệu. |

### Điểm cần PO chốt

- Quy tắc báo lại sau khi CMS bỏ qua Report và thông báo kết quả cho người báo.
- Ngưỡng rate limit theo tài khoản/IP/thời gian.
