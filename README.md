# MYTV COMMENT BACKLOG

## 5 Epic và 20 User Story

Tài liệu này chuyển yêu cầu tính năng Bình luận trên MyTV thành backlog sản phẩm ở cấp PO. Backlog được tổ chức theo 5 Epic và 20 User Story; các công việc frontend, backend, cơ sở dữ liệu, hạ tầng và kiểm thử được xem là task/sub-task triển khai, không được tính thêm thành User Story.

## 1. Mục tiêu sản phẩm

- Tạo khu vực thảo luận giúp người xem tương tác với phim và từng tập phim.
- Tăng mức độ gắn kết, thời gian sử dụng và khả năng quay lại MyTV.
- Cho phép mọi người đọc bình luận nhưng yêu cầu đăng nhập khi tương tác.
- Kiểm soát nội dung bằng AI kết hợp Admin, đồng thời duy trì trải nghiệm gần thời gian thực.
- Cung cấp công cụ CMS để vận hành, kiểm duyệt, xử lý vi phạm và theo dõi hiệu quả.

## 2. Phạm vi giai đoạn hiện tại

- Áp dụng trước cho dịch vụ Phim truyện.
- Hỗ trợ bình luận chung ở cấp series và bình luận riêng ở từng tập.
- Hỗ trợ người xem chưa đăng nhập, người dùng đã đăng nhập, Admin/CMS và hệ thống AI.
- Việc mở rộng sang thể thao, thiếu nhi, giải trí và các loại nội dung khác không nằm trong 20 User Story này.

## 3. Danh sách Epic

| Epic | Tên Epic | Số User Story | File chi tiết |
|---|---|---:|---|
| EP01 | Trải nghiệm bình luận và đánh giá | 6 | [README EP01](EP01_TRAI_NGHIEM_BINH_LUAN_VA_DANH_GIA/README.md) |
| EP02 | Tương tác cộng đồng | 3 | [README EP02](EP02_TUONG_TAC_CONG_DONG/README.md) |
| EP03 | An toàn và kiểm duyệt | 3 | [README EP03](EP03_AN_TOAN_VA_KIEM_DUYET/README.md) |
| EP04 | CMS quản trị bình luận | 4 | [README EP04](EP04_CMS_QUAN_TRI_BINH_LUAN/README.md) |
| EP05 | Tăng trưởng và phân tích | 4 | [README EP05](EP05_TANG_TRUONG_VA_PHAN_TICH/README.md) |
| **Tổng** |  | **20** |  |

## 3.1. Cấu trúc thư mục

Mỗi Epic có một thư mục riêng. README trong thư mục Epic chứa thông tin Epic, danh sách story và liên kết đến từng file User Story.

| Thư mục | Nội dung |
|---|---|
| [EP01 — Trải nghiệm bình luận và đánh giá](EP01_TRAI_NGHIEM_BINH_LUAN_VA_DANH_GIA/README.md) | US01–US06 |
| [EP02 — Tương tác cộng đồng](EP02_TUONG_TAC_CONG_DONG/README.md) | US07–US09 |
| [EP03 — An toàn và kiểm duyệt](EP03_AN_TOAN_VA_KIEM_DUYET/README.md) | US10–US12 |
| [EP04 — CMS quản trị bình luận](EP04_CMS_QUAN_TRI_BINH_LUAN/README.md) | US13–US16 |
| [EP05 — Tăng trưởng và phân tích](EP05_TANG_TRUONG_VA_PHAN_TICH/README.md) | US17–US20 |

Các file tổng hợp `EPxx_*.md` ở cấp gốc đã được loại bỏ; bản dùng để quản lý chi tiết nằm trong 5 thư mục Epic.

## 4. Danh sách 20 User Story

| ID | Tên User Story | Epic | Ưu tiên đề xuất |
|---|---|---|---|
| US01 | Đọc khu vực bình luận | EP01 | Must |
| US02 | Xem bình luận theo series/tập, số lượng và sắp xếp | EP01 | Must |
| US03 | Đánh giá series và tập phim | EP01 | Must |
| US04 | Đăng bình luận | EP01 | Must |
| US05 | Sửa và xóa bình luận | EP01 | Must |
| US06 | Bình luận kèm cảnh phim | EP01 | Could |
| US07 | Like và Unlike bình luận | EP02 | Must |
| US08 | Trả lời bình luận một cấp | EP02 | Must |
| US09 | Mention và nhận thông báo | EP02 | Should |
| US10 | Report bình luận vi phạm | EP03 | Must |
| US11 | AI kiểm duyệt theo hai chế độ | EP03 | Must |
| US12 | Quản lý trạng thái và phạm vi hiển thị bình luận | EP03 | Must |
| US13 | Tra cứu bình luận trên CMS | EP04 | Must |
| US14 | Xử lý nội dung trên CMS | EP04 | Must |
| US15 | Quản lý bình luận nổi bật và cấu hình theo phim | EP04 | Must |
| US16 | Quản lý người dùng vi phạm và audit log | EP04 | Must |
| US17 | Huy hiệu người dùng | EP05 | Could |
| US18 | Chia sẻ bình luận và cảnh phim | EP05 | Could |
| US19 | Thống kê hoạt động bình luận | EP05 | Must |
| US20 | AI hỗ trợ vận hành cộng đồng | EP05 | Could |

## 5. Các quyết định nghiệp vụ đã áp dụng

1. Mọi người có thể đọc bình luận; chỉ người dùng đã đăng nhập mới được tương tác.
2. Điểm đánh giá tồn tại ở cả cấp series và từng tập.
3. Chế độ sắp xếp mặc định là Nổi bật.
4. Reply chỉ sâu một cấp.
5. Người dùng được sửa bình luận bất kỳ lúc nào.
6. Trong khi phiên bản sửa đang chờ kiểm duyệt, người khác tiếp tục thấy phiên bản cũ đã được duyệt.
7. Xóa bình luận gốc sẽ xóa toàn bộ thread.
8. Dữ liệu xóa mềm được giữ 90 ngày để audit.
9. Khi bình luận nhận nhiều Report, bình luận vẫn hiển thị và chờ CMS xử lý; không tự động ẩn chỉ vì số lượng Report.
10. Reply và mention tạo cả push notification và thông báo trong ứng dụng.
11. Khi đóng bình luận cho một phim, toàn bộ khu vực bình luận được ẩn khỏi người xem.

## 6. Quy ước ưu tiên

- **Must:** cần có để phát hành MVP an toàn và tạo được luồng bình luận hoàn chỉnh.
- **Should:** mang lại giá trị cao nhưng có thể phát hành sau luồng cốt lõi.
- **Could:** tính năng tăng trưởng hoặc nâng cao, có thể triển khai sau khi MVP ổn định.

## 7. Các điểm PO cần chốt trước khi đưa story vào sprint

- Giới hạn ký tự của bình luận/reply và quy tắc xử lý URL.
- Kích thước trang, cơ chế “Tải thêm” và số reply hiển thị ban đầu.
- Công thức xếp hạng Nổi bật sau các bình luận được Admin ghim.
- Ngưỡng rủi ro AI và hành động tương ứng: che từ, chặn hay chuyển hàng chờ.
- Quy tắc nickname: duy nhất hay không, tần suất đổi và kiểm duyệt nickname.
- Giới hạn tần suất đăng, Like, Reply, Mention và Report để chống spam.
- Phạm vi khóa tài khoản: chỉ khóa quyền bình luận hay khóa toàn bộ tài khoản MyTV.
- Quyền sử dụng frame/video khi nội dung hết hạn bản quyền hoặc thay đổi nguồn phát.
- SLA xử lý hàng chờ kiểm duyệt và quy trình khi người dùng khiếu nại.

## 8. Nguyên tắc refinement

Mỗi User Story trong bộ tài liệu là một lát cắt giá trị ở cấp PO. Nếu một story không thể hoàn thành trong một sprint, đội sản phẩm có thể tách thành các delivery story nhỏ hơn nhưng phải giữ nguyên mục tiêu và quy tắc nghiệp vụ. Không tách riêng web, mobile, API hoặc database thành User Story nếu chúng chỉ là các phần kỹ thuật cần thiết để hoàn thành cùng một giá trị người dùng.
