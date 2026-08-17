# US15 — Quản lý bình luận nổi bật và cấu hình theo phim

> Thuộc EP04 — CMS quản trị bình luận
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)


### User Story

**Là Admin vận hành nội dung**, tôi muốn ghim bình luận nổi bật và cấu hình trạng thái/cơ chế kiểm duyệt theo từng phim, để định hướng thảo luận và áp dụng mức kiểm soát phù hợp.

### Giá trị

- Làm nổi bật nội dung thảo luận có chất lượng.
- Cho phép phản ứng nhanh với nội dung hoặc giai đoạn nhạy cảm.
- Giảm phụ thuộc vào cấu hình kỹ thuật thủ công.

### Ưu tiên

**Must**

### Điều kiện tiên quyết

- Admin có quyền vận hành nội dung.
- Phim đã tồn tại trong CMS và liên kết đúng với dịch vụ bình luận.

### Acceptance Criteria — Bình luận nổi bật

1. Admin có thể ghim một bình luận đang Hiển thị lên khu vực Nổi bật.
2. Admin có thể bỏ ghim hoặc thay thế bình luận đã ghim.
3. Hệ thống giới hạn số lượng bình luận ghim theo cấu hình; giá trị đề xuất mặc định là ba.
4. Khi đạt giới hạn, CMS yêu cầu Admin bỏ ghim/thay thế trước khi thêm mới hoặc thực hiện theo UX đã duyệt.
5. Bình luận bị ẩn hoặc xóa tự động không còn được ghim công khai.
6. Thứ tự các bình luận ghim được áp dụng nhất quán trên web/mobile.
7. Mọi thao tác ghim/bỏ ghim được lưu audit.

### Acceptance Criteria — Cấu hình theo phim

1. Admin có thể chọn Mở hoặc Đóng khu vực Bình luận cho từng phim.
2. Khi Mở, Admin chọn Chế độ 1 hoặc Chế độ 2 theo định nghĩa tại US11.
3. Khi Đóng, toàn bộ khu vực Bình luận bị ẩn theo US12 và hệ thống ngừng nhận tương tác mới.
4. Admin thấy rõ trạng thái hiện hành và thời điểm hiệu lực của cấu hình.
5. Admin có thể cấu hình mốc thời gian chuyển sang trạng thái “Chờ duyệt — chỉ tác giả thấy” nếu nghiệp vụ sử dụng quy tắc sau X giờ.
6. Hệ thống cảnh báo tác động trước khi Admin thay đổi cấu hình đang áp dụng cho phim có nhiều tương tác.
7. Thay đổi cấu hình được truyền tới ứng dụng và API trong độ trễ được thống nhất.
8. Toàn bộ lịch sử cấu hình được lưu với người thao tác và thời gian.

### Quy tắc nghiệp vụ

- Nổi bật là chế độ sắp xếp mặc định trên ứng dụng.
- AI có thể đề xuất ứng viên nổi bật tại US20, nhưng Admin là người quyết định ghim trong phạm vi hiện tại.
- Đóng bình luận không xóa bình luận cũ.
- Cấu hình áp dụng theo phim; khả năng ghi đè ở cấp tập cần được coi là phạm vi bổ sung nếu PO yêu cầu.

### Phụ thuộc

- US02 — Hiển thị và sắp xếp.
- US11, US12 — Chế độ kiểm duyệt và trạng thái.
- US20 — AI hỗ trợ vận hành cộng đồng.

### Điểm cần PO chốt

- Có cho phép sắp xếp thủ công thứ tự ba bình luận ghim hay theo thời gian ghim.
- Có cần ngày hết hạn ghim.
- Mốc “X giờ” được tính từ sự kiện nào.
- Có cần cấu hình riêng theo tập phim trong tương lai.

---

## Phân tích kiểm thử

### Mục tiêu

Kiểm tra Admin ghim/bỏ ghim đúng giới hạn, cấu hình Mở/Đóng/Chế độ 1/2 theo từng phim, thời điểm hiệu lực, propagation và audit.

### Rủi ro chính

- Ghim comment không công khai, vượt giới hạn hoặc hiển thị sai thứ tự.
- Cấu hình một phim ảnh hưởng phim/tập khác.
- Đóng UI nhưng API vẫn nhận tương tác; thay đổi cấu hình không truyền tới app đúng SLA.

### Dữ liệu kiểm thử

Phim P1/P2, comment Hiển thị/Ẩn/Xóa/Chờ duyệt, cấu hình giới hạn ghim 3, Admin có/không có quyền, mốc hiệu lực hiện tại và tương lai.

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US15-001 | Pinning | P1 có comment Hiển thị C1 | Admin ghim C1 | C1 xuất hiện khu vực Nổi bật; audit lưu actor/thời gian/phim. |
| TC-US15-002 | Pinning | C1 đã ghim | Admin bỏ ghim rồi refresh app | C1 không còn ưu tiên; trạng thái hai phía nhất quán. |
| TC-US15-003 | Boundary | Đã có 3 comment ghim, giới hạn = 3 | Thử ghim C4 | CMS yêu cầu bỏ ghim/thay thế hoặc xử lý đúng UX; không vượt quá 3. |
| TC-US15-004 | Invalid pin | C1 Chờ duyệt/Ẩn/Xóa | Thử ghim qua UI và API | Không cho ghim nội dung không công khai; không tạo record ghim. |
| TC-US15-005 | Ordering | Có nhiều comment ghim | Kiểm tra web/mobile và đổi thứ tự theo cấu hình | Thứ tự ghim nhất quán; tie/order tuân quy tắc đã chốt. |
| TC-US15-006 | Config isolation | P1 và P2 đang Mở | Đổi cấu hình P1 sang Đóng/Chế độ 2 | Chỉ P1 đổi; P2 và dữ liệu/ghim của P2 không bị ảnh hưởng. |
| TC-US15-007 | Config state | P1 đang Mở | Chọn Chế độ 1, Chế độ 2, Đóng và kiểm tra app/API | State hiển thị đúng; Đóng ẩn khu vực và ngừng tương tác; mode 1/2 áp dụng đúng US11. |
| TC-US15-008 | Effective time | Cấu hình có thời điểm hiệu lực | Lưu mốc tương lai; kiểm tra trước/đúng/sau mốc | App/API chỉ áp dụng từ thời điểm hiệu lực, timezone nhất quán. |
| TC-US15-009 | Impact warning | P1 có nhiều tương tác | Thay đổi cấu hình đang áp dụng | CMS hiển thị cảnh báo tác động và yêu cầu xác nhận theo thiết kế. |
| TC-US15-010 | Propagation | Đã đổi cấu hình P1 | Kiểm tra app, CMS cache và public API trong SLA | Các kênh hội tụ state mới; không có cửa sổ cho phép tương tác trái cấu hình sau SLA. |
| TC-US15-011 | Audit | Thực hiện ghim/bỏ ghim/đổi cấu hình | Tra cứu lịch sử | Ghi actor, thời gian, phim, trước/sau, lý do và thời điểm hiệu lực. |
| TC-US15-012 | Authorization | Admin vận hành và role không có quyền | Thử thao tác ghim/cấu hình UI/API | Chỉ role được cấp phép thao tác; request trái quyền bị từ chối. |

### Điểm cần PO chốt

- Thứ tự ghim, ngày hết hạn và việc override ở cấp tập.
- Cách tính mốc “sau X giờ” và hành vi comment đang xử lý khi đổi cấu hình.
