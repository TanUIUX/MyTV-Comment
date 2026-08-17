# US13 — Tra cứu bình luận trên CMS

> Thuộc EP04 — CMS quản trị bình luận
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là Admin**, tôi muốn xem, tìm kiếm và lọc toàn bộ bình luận theo nhiều tiêu chí, để nhanh chóng xác định nội dung cần kiểm duyệt hoặc điều tra.

### Giá trị

- Giảm thời gian tìm kiếm thủ công.
- Hỗ trợ xử lý hàng chờ và sự cố theo đúng ngữ cảnh.
- Tạo một nguồn dữ liệu quản trị thống nhất.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Admin đã đăng nhập CMS và có quyền phù hợp.
- Dữ liệu bình luận và metadata được đồng bộ với CMS.

### Acceptance Criteria

1. Admin xem được danh sách bình luận và reply trên toàn bộ phạm vi được phân quyền.
2. Mỗi bản ghi hiển thị tối thiểu: nội dung, tác giả, phim/series, tập, thời gian đăng, trạng thái, số Like, số Reply và số Report.
3. Admin tìm kiếm được theo từ khóa nội dung.
4. Admin tìm kiếm được theo tài khoản/nickname hoặc định danh hợp lệ được phép xem.
5. Admin lọc được theo phim, tập và khoảng thời gian.
6. Admin lọc được theo trạng thái: Hiển thị, Chờ duyệt, Bị Report, Có Spoiler, Spam/nhãn tương ứng, Ẩn, Từ chối và Đã xử lý theo mô hình dữ liệu thực tế.
7. Admin có thể kết hợp nhiều bộ lọc và xóa điều kiện lọc để quay về danh sách mặc định.
8. Kết quả có phân trang hoặc tải thêm và không lặp/bỏ sót bản ghi.
9. Admin mở được màn hình chi tiết để xem thread, phiên bản nội dung, kết quả AI, Report/Flag và lịch sử xử lý liên quan.
10. Khi một bình luận gốc có reply, Admin xem được toàn bộ ngữ cảnh thread trước khi xử lý.
11. Dữ liệu nhạy cảm chỉ hiển thị cho vai trò được cấp quyền; CMS không mặc định công khai đầy đủ thông tin thuê bao.
12. Trạng thái mới được phản ánh trong danh sách theo độ trễ vận hành được thống nhất.

### Quy tắc nghiệp vụ

- Quyền xem dữ liệu phải giới hạn theo vai trò/phạm vi của Admin.
- Tìm kiếm và lọc không làm thay đổi dữ liệu.
- Danh sách mặc định nên ưu tiên nội dung cần xử lý theo SLA, nhưng quy tắc cụ thể cần PO/vận hành chốt.

### Phụ thuộc

- Mô hình dữ liệu bình luận, trạng thái và phiên bản.
- Hệ thống phân quyền CMS.
- US11, US12 về nhãn AI và trạng thái.

### Điểm cần PO chốt

- Danh sách mặc định ưu tiên Chờ duyệt, Report hay thời gian mới nhất.
- Nhu cầu export dữ liệu và giới hạn dữ liệu được export.
- Phạm vi dữ liệu cá nhân từng vai trò Admin được xem.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra Admin tra cứu được đúng dữ liệu theo quyền, tìm/lọc kết hợp không bỏ sót, xem đủ ngữ cảnh và không lộ PII ngoài phạm vi.

### Rủi ro chính

- Admin nhìn thấy dữ liệu ngoài phạm vi hoặc số thuê bao đầy đủ.
- Kết hợp bộ lọc trả kết quả sai; phân trang lặp/bỏ sót.
- Chi tiết không hiển thị version/AI/Report/thread nên quyết định thiếu ngữ cảnh.

### Dữ liệu kiểm thử

Admin các role khác nhau; comment/reply ở nhiều phim/tập, thời gian, trạng thái, nhãn, số Like/Reply/Report; dữ liệu đủ lớn hơn một trang.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US13-001 | Authorization | Admin role toàn quyền và role giới hạn | Đăng nhập CMS, mở danh sách | Mỗi role chỉ thấy phạm vi/phim/dữ liệu được cấp; API áp dụng cùng quyền. |
| TC-US13-002 | Completeness | Có comment/reply ở nhiều trạng thái | Mở danh sách mặc định | Mỗi record có nội dung, tác giả, phim/series, tập, thời gian, trạng thái, Like, Reply, Report tối thiểu. |
| TC-US13-003 | Search | Comment chứa từ khóa ở giữa/cuối text | Tìm bằng từ khóa có dấu/không dấu theo rule | Trả đúng record khớp; không trả kết quả ngoài phạm vi. |
| TC-US13-004 | Search/account | Có nickname và định danh được phép xem | Tìm theo nickname/account | Trả đúng tài khoản; PII được masking theo role. |
| TC-US13-005 | Filter | Có dữ liệu nhiều phim/tập/thời gian | Lọc riêng theo phim, tập, khoảng ngày | Mỗi bộ lọc trả đúng dữ liệu và biên ngày được xử lý nhất quán. |
| TC-US13-006 | Filter combination | Có dữ liệu giao nhau | Kết hợp keyword + phim + tập + trạng thái + thời gian | Kết quả thỏa tất cả điều kiện; xóa một filter cập nhật đúng tập kết quả. |
| TC-US13-007 | Status filter | Có Hiển thị, Chờ duyệt, Report, Spoiler, Spam, Ẩn, Từ chối, Đã xử lý | Chọn lần lượt từng trạng thái | Chỉ record đúng nhãn/state xuất hiện; trạng thái được giải thích đúng UI. |
| TC-US13-008 | Pagination | Kết quả > một trang, ID ổn định | Phân trang/tải thêm đến cuối, quay lại trang trước | Không lặp/bỏ sót; tổng số và thứ tự ổn định theo query. |
| TC-US13-009 | Detail/context | C1 có R1, version, AI result, Report/Flag | Mở màn hình chi tiết | Thấy thread, version, kết quả AI, Report/Flag và lịch sử liên quan trước khi xử lý. |
| TC-US13-010 | Data freshness | C1 vừa đổi state ở CMS khác | Refresh danh sách theo SLA | State mới phản ánh trong độ trễ đã công bố; không hiển thị state cũ vô thời hạn. |
| TC-US13-011 | Security/privacy | Role không được xem PII | Kiểm tra UI, API, export nếu có | Dữ liệu nhạy cảm bị che/loại bỏ; không bypass bằng endpoint chi tiết. |

### Điểm cần PO chốt

- Thứ tự mặc định theo Chờ duyệt/Report/SLA và khả năng export.
- Ma trận PII được xem theo từng role Admin.
