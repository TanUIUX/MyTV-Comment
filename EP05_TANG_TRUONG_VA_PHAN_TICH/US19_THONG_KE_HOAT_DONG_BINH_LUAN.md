# US19 — Thống kê hoạt động bình luận

> Thuộc EP05 — Tăng trưởng và phân tích
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là quản lý sản phẩm/Admin vận hành**, tôi muốn theo dõi KPI bình luận và tương tác theo phim/tập/thời gian, để đánh giá hiệu quả và ưu tiên vận hành.

### Ưu tiên

**Must**

### Engagement Score

`Engagement Score = Comment × 2 + Reply × 2 + Net Like × 1 + Rating × 1 + Share × 2`

- Chỉ tính Comment/Reply hợp lệ trong dữ liệu chính thức; nội dung **Ẩn / Từ chối / Xóa do vi phạm không được tính** và aggregate phải điều chỉnh khi state thay đổi.
- Engagement dùng **Net Like hiện tại**, không dùng tổng số lần bấm Like lịch sử.

### KPI MVP

- Comment gốc mới/công khai.
- Reply mới/công khai.
- Unique commenters.
- Net Like hiện tại + tổng thao tác Like/Unlike lịch sử.
- Rating.
- Share.
- Report + tỷ lệ Report được xác nhận vi phạm.
- Nội dung Ẩn/Xóa/Từ chối.
- Tỷ lệ AI tự Hiển thị ở Mode1.
- Thời gian xử lý moderation queue/SLA.
- Engagement Score và ranking phim/tập.

### Acceptance Criteria

1. Dashboard filter theo phim/series/tập và khoảng thời gian.
2. **Unique commenters**: trong một khoảng thời gian + scope filter, mỗi account chỉ tính **1 lần** bất kể có bao nhiêu comment/reply; đổi filter thì tính lại unique trong scope mới.
3. Dashboard cập nhật với độ trễ tối đa **5 phút** và hiển thị thời điểm cập nhật gần nhất.
4. Ranking phim/tập dùng Engagement Score đã chốt; tie-break phải ổn định.
5. Dashboard hiển thị Net Like và tổng thao tác Like/Unlike; Engagement chỉ dùng Net Like.
6. Khi comment/reply bị Ẩn/Từ chối/Xóa do vi phạm, score/KPI chính thức được điều chỉnh ở pipeline/đối soát tiếp theo.
7. Có job **đối soát tự động mỗi ngày** với dữ liệu nguồn; phát hiện lệch thì tự hiệu chỉnh aggregate và ghi log reconciliation.
8. Hỗ trợ export dashboard/report theo filter + khoảng thời gian hiện tại ở cả **CSV + XLSX**.
9. Retry/duplicate event không làm tăng KPI sai.
10. Quyền dữ liệu/export tuân US13; báo cáo không tự đưa PII nếu không cần.
11. Dashboard có empty/error state rõ ràng.

### Quy tắc nghiệp vụ

- Data dictionary phải định nghĩa rõ Comment/Reply hợp lệ, Share, Rating, Net Like, Unique commenter và denominator của các tỷ lệ.
- Nội dung Chờ duyệt chưa được tính vào public KPI.
- Daily reconciliation là lớp bảo đảm tính đúng dài hạn cho pipeline gần real-time 5 phút.

### Điểm cần PO chốt

- Không còn blocker PO cho bộ KPI/engagement/freshness hiện tại; data dictionary chi tiết là đầu việc refinement dữ liệu dựa trên các rule đã khóa.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US19-001 | Metrics | Có event Comment/Reply/Like/Rating/Share | Mở dashboard | Các KPI đúng scope/time. |
| TC-US19-002 | Engagement | Có dữ liệu chuẩn | Tính score | Khớp `2C + 2R + NetLike + Rating + 2Share`. |
| TC-US19-003 | Invalid content | Comment/reply đang tính score rồi bị Ẩn/Từ chối/Xóa | Refresh sau pipeline/reconciliation | Nội dung bị loại khỏi score chính thức. |
| TC-US19-004 | Like semantics | Like rồi Unlike nhiều lần | Kiểm tra dashboard | Có thể thấy tổng action history; Engagement chỉ dùng Net Like hiện hành. |
| TC-US19-005 | Unique | U1 có nhiều comment/reply trong cùng filter | Tính Unique commenters | U1 chỉ tính 1. |
| TC-US19-006 | Filter scope | U1 tham gia nhiều phim/tập | Đổi filter | Unique được tính lại trong từng scope, không cộng sai ở dashboard tổng. |
| TC-US19-007 | Freshness | Phát sinh event mới | Refresh | Dữ liệu xuất hiện trong tối đa 5 phút; UI có last-updated. |
| TC-US19-008 | Reconciliation | Cố tình tạo aggregate lệch | Chạy daily job | Job phát hiện, tự sửa và ghi reconciliation log. |
| TC-US19-009 | Export | Có filter/time range | Export CSV/XLSX | File khớp dashboard/data dictionary và filter hiện tại. |
| TC-US19-010 | Extended KPI | Có Report/AI/moderation data | Mở KPI | Có confirmed-report rate, hidden/deleted/rejected, AI auto-display rate, queue time. |
| TC-US19-011 | Dedup | Event retry/duplicate | Nạp pipeline | Event không bị tính lặp ngoài định nghĩa. |
