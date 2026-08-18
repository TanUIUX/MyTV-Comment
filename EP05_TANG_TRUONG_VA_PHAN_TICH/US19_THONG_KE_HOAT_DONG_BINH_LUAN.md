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
- `Rating` = **số lượng rating hợp lệ hiện hành** trong scope/thời gian đang tính; mỗi account có một rating hợp lệ đóng góp **+1**, bất kể rating 1★ hay 5★.
- `Share` = số **Share event hợp lệ khi OS share sheet mở thành công** theo US18; không cần callback xác nhận user đã gửi sang ứng dụng đích.

### KPI MVP

- Comment gốc mới/công khai.
- Reply mới/công khai.
- Unique commenters.
- Net Like hiện tại + tổng thao tác Like/Unlike lịch sử.
- Rating: số rating hợp lệ hiện hành + điểm rating trung bình/tổng lượt hiển thị theo US03.
- Share: số share sheet mở thành công theo rule US18.
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
6. Trong Engagement, **mỗi rating hợp lệ hiện hành = 1 đơn vị Rating**, không nhân theo số sao; điểm sao/average là KPI chất lượng riêng.
7. Trong Engagement, **mỗi lần OS share sheet mở thành công = 1 Share**; cancel/đóng sheet sau đó không hoàn tác event đã ghi; retry/lỗi kỹ thuật phải dedup.
8. Khi comment/reply bị Ẩn/Từ chối/Xóa do vi phạm, score/KPI chính thức được điều chỉnh ở pipeline/đối soát tiếp theo.
9. Có job **đối soát tự động mỗi ngày** với dữ liệu nguồn; phát hiện lệch thì tự hiệu chỉnh aggregate và ghi log reconciliation.
10. Hỗ trợ export dashboard/report theo filter + khoảng thời gian hiện tại ở cả **CSV + XLSX**.
11. Retry/duplicate event không làm tăng KPI sai.
12. Quyền dữ liệu/export tuân US13; báo cáo không tự đưa PII nếu không cần.
13. Dashboard có empty/error state rõ ràng.

### Quy tắc nghiệp vụ

- Data dictionary phải định nghĩa rõ Comment/Reply hợp lệ, Net Like, Unique commenter và denominator của các tỷ lệ; semantics Rating/Share trong Engagement đã khóa tại US này.
- Rating 1★ và 5★ có cùng trọng số interaction `+1 Rating`; chất lượng rating được phản ánh ở KPI average riêng, không làm thay đổi trọng số Engagement của một hành động rating.
- Share event được ghi tại mốc share sheet mở thành công; không phụ thuộc callback hoàn tất share của OS/app đích.
- Nội dung Chờ duyệt chưa được tính vào public KPI.
- Daily reconciliation là lớp bảo đảm tính đúng dài hạn cho pipeline gần real-time 5 phút.

### Điểm cần PO chốt

- Không còn blocker PO cho bộ KPI/engagement/freshness hiện tại; data dictionary chi tiết là đầu việc refinement dữ liệu dựa trên các rule đã khóa.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US19-001 | Metrics | Có event Comment/Reply/Like/Rating/Share | Mở dashboard | Các KPI đúng scope/time. |
| TC-US19-002 | Engagement | C=2, R=3, NetLike=4, Rating hợp lệ hiện hành=5, Share sheet-open=6 | Tính score | Khớp `2C + 2R + NetLike + Rating + 2Share = 31`. |
| TC-US19-003 | Rating semantics | U1 rating 1★, U2 rating 5★ đều hợp lệ hiện hành | Tính Engagement | Rating đóng góp tổng **2**, không phải 6; average rating được tính riêng theo US03. |
| TC-US19-004 | Rating lock | U1 có rating rồi account bị khóa toàn bộ | Refresh sau pipeline | Rating U1 bị loại khỏi count Rating dùng cho Engagement và khỏi aggregate công khai theo US03; mở khóa tính lại nếu record còn. |
| TC-US19-005 | Share semantics | User mở share sheet thành công rồi cancel | Kiểm tra KPI | Share tăng 1; không cần callback hoàn tất gửi. |
| TC-US19-006 | Invalid content | Comment/reply đang tính score rồi bị Ẩn/Từ chối/Xóa | Refresh sau pipeline/reconciliation | Nội dung bị loại khỏi score chính thức. |
| TC-US19-007 | Like semantics | Like rồi Unlike nhiều lần | Kiểm tra dashboard | Có thể thấy tổng action history; Engagement chỉ dùng Net Like hiện hành. |
| TC-US19-008 | Unique | U1 có nhiều comment/reply trong cùng filter | Tính Unique commenters | U1 chỉ tính 1. |
| TC-US19-009 | Filter scope | U1 tham gia nhiều phim/tập | Đổi filter | Unique được tính lại trong từng scope, không cộng sai ở dashboard tổng. |
| TC-US19-010 | Freshness | Phát sinh event mới | Refresh | Dữ liệu xuất hiện trong tối đa 5 phút; UI có last-updated. |
| TC-US19-011 | Reconciliation | Cố tình tạo aggregate lệch | Chạy daily job | Job phát hiện, tự sửa và ghi reconciliation log. |
| TC-US19-012 | Export | Có filter/time range | Export CSV/XLSX | File khớp dashboard/data dictionary và filter hiện tại. |
| TC-US19-013 | Extended KPI | Có Report/AI/moderation data | Mở KPI | Có confirmed-report rate, hidden/deleted/rejected, AI auto-display rate, queue time. |
| TC-US19-014 | Dedup | Event retry/duplicate, gồm Share retry do lỗi kỹ thuật | Nạp pipeline | Event không bị tính lặp ngoài định nghĩa; Engagement không tăng sai. |
