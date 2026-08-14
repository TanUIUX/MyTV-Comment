# US19 — Thống kê hoạt động bình luận

> Thuộc EP05 — Tăng trưởng và phân tích
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


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

