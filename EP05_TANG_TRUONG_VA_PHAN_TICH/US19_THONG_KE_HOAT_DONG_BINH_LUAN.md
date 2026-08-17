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

## Phân tích kiểm thử

### Mục tiêu

Xác nhận dashboard tính đúng các chỉ số theo phim/series/tập/thời gian, phân biệt trạng thái, xử lý event retry không trùng và bảo vệ dữ liệu/role.

### Rủi ro chính

- Dashboard cộng Chờ duyệt vào số công khai hoặc tính duplicate event.
- Bộ lọc/ranking sai chiều dữ liệu hoặc độ trễ không minh bạch.
- Người không có quyền xem được dữ liệu chi tiết/PII.

### Dữ liệu kiểm thử

Event comment/reply/Like/Unlike/Report/AI/moderation có timestamp, phim/series/tập; event retry/trùng; state công khai, Chờ duyệt, Ẩn, Xóa; role dashboard khác nhau.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US19-001 | Functional | Có event ở nhiều phim/series/tập | Mở dashboard và xem theo từng chiều | Hiển thị đúng số comment theo phim, series, tập và khoảng thời gian. |
| TC-US19-002 | Metrics | Có Like, Unlike, Reply, Report | Kiểm tra các card/bảng tương ứng | Số liệu đúng định nghĩa data dictionary và cùng phạm vi thời gian/filter. |
| TC-US19-003 | State | Có comment công khai, Chờ duyệt, Ẩn, Xóa | Đối chiếu chỉ số tạo mới/đang hiển thị/bị xử lý | Các nhóm được tách đúng; Chờ duyệt không vào “công khai” nếu chưa hiển thị. |
| TC-US19-004 | Filter/time | Có event ở biên ngày/giờ | Chọn khoảng thời gian, phim, tập và kết hợp filter | Kết quả đúng inclusive/exclusive rule; filter không làm mất hoặc nhân đôi event. |
| TC-US19-005 | Ranking | Nhiều phim/tập có engagement khác nhau | Xem bảng xếp hạng | Thứ hạng theo công thức engagement score đã chốt; tie-break ổn định. |
| TC-US19-006 | Freshness | Event mới phát sinh | Ghi nhận thời điểm event và refresh dashboard | Dữ liệu cập nhật trong độ trễ công bố; UI thể hiện thời điểm cập nhật. |
| TC-US19-007 | Reconciliation | Có dataset chuẩn và báo cáo xuất | So sánh dashboard với báo cáo/API tổng hợp | Số liệu khớp định nghĩa; sai lệch được phát hiện/giải thích nếu pipeline trễ. |
| TC-US19-008 | Deduplication | Gửi cùng event nhiều lần/retry | Nạp pipeline và kiểm tra metric | Event chỉ được tính theo định nghĩa một lần; retry không làm tăng sai số. |
| TC-US19-009 | Authorization | Role quản lý, Admin vận hành, role không có quyền | Mở dashboard/detail/export | Mỗi role chỉ thấy dữ liệu được cấp; PII bị ẩn/loại trong báo cáo không cần thiết. |
| TC-US19-010 | Empty state | Filter không có kết quả | Chọn phim/tập/thời gian không có event | Hiển thị trạng thái không có dữ liệu, không nhầm thành 0 lỗi hoặc dữ liệu cũ. |
| TC-US19-011 | Error handling | Mock data service timeout/5xx | Tải/refresh dashboard | Hiển thị lỗi và nút thử lại; không trộn dữ liệu cũ với filter mới không rõ trạng thái. |
| TC-US19-012 | Minimum KPI | Có event AI, thời gian xử lý queue, hidden/deleted | Kiểm tra bộ KPI tối thiểu | Dashboard có các KPI đã cam kết hoặc đánh dấu rõ KPI chưa khả dụng. |

### Điểm cần PO chốt

- Data dictionary, độ trễ dữ liệu, engagement score và phạm vi export CSV/XLSX/BI.
