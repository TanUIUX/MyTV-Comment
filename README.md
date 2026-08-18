# MYTV COMMENT BACKLOG

## 5 Epic và 20 User Story

Tài liệu chuyển yêu cầu tính năng Bình luận trên MyTV thành backlog sản phẩm ở cấp PO. Backlog gồm 5 Epic và 20 User Story; frontend/backend/database/infra/test là task triển khai, không phát sinh thêm User Story nếu không tạo giá trị người dùng độc lập.

## 1. Mục tiêu sản phẩm

- Tạo khu vực thảo luận cho phim/series/từng tập.
- Mọi người được đọc comment public; đăng nhập mới được tương tác.
- Kết hợp AI + Admin moderation với state/audit rõ ràng.
- Cung cấp CMS để kiểm duyệt, xử lý vi phạm, appeal và theo dõi KPI.
- Tạo cơ chế tăng trưởng qua badge, share/deep link và AI hỗ trợ vận hành.

## 2. Phạm vi hiện tại

- Áp dụng trước cho Phim truyện/VOD có timeline cố định.
- Hỗ trợ comment cấp series và từng episode.
- Live/đang phát trực tiếp ngoài scope timestamp hiện tại.
- **Không triển khai frame/clip trong MVP**; US06 dùng timestamp.
- Mở rộng sang thể thao/thiếu nhi/giải trí ngoài scope 20 User Story hiện tại.

## 3. Danh sách Epic

| Epic | Tên Epic | User Story |
|---|---|---|
| EP01 | Trải nghiệm bình luận và đánh giá | US01–US06 |
| EP02 | Tương tác cộng đồng | US07–US09 |
| EP03 | An toàn và kiểm duyệt | US10–US12 |
| EP04 | CMS quản trị bình luận | US13–US16 |
| EP05 | Tăng trưởng và phân tích | US17–US20 |

## 4. Danh sách 20 User Story

| ID | Tên User Story | Epic | Ưu tiên |
|---|---|---|---|
| US01 | Đọc khu vực bình luận | EP01 | Must |
| US02 | Xem bình luận theo series/tập, số lượng và sắp xếp | EP01 | Must |
| US03 | Đánh giá series và tập phim | EP01 | Must |
| US04 | Đăng bình luận | EP01 | Must |
| US05 | Sửa và xóa bình luận | EP01 | Must |
| US06 | Bình luận kèm mốc thời gian | EP01 | Could |
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
| US18 | Chia sẻ bình luận | EP05 | Could |
| US19 | Thống kê hoạt động bình luận | EP05 | Must |
| US20 | AI hỗ trợ vận hành cộng đồng | EP05 | Could |

> Ghi chú: filename US06/US18 vẫn giữ tên legacy chứa `CANH_PHIM` để không làm gãy liên kết repo; scope nội dung trong file đã chuyển sang timestamp/share comment MVP.

## 5. Các quyết định nghiệp vụ đã khóa

### 5.1. Quyền truy cập và cấu trúc thảo luận

- Guest đọc được comment public nhưng không tạo Comment, Reply, Like, Mention, Report, Rating, Share hoặc interaction event.
- Rating có cả cấp series và episode.
- Reply chỉ sâu một cấp.
- Xóa root comment làm toàn bộ thread biến mất khỏi public UX; xóa reply riêng lẻ không để placeholder.
- Soft-delete comment/reply giữ 90 ngày.

### 5.2. Sắp xếp, ghim và pagination

- Nổi bật là sort mặc định.
- Hard max **3 comment ghim/scope**; Admin drag-drop vị trí 1–3; pin có expiry tùy chọn.
- Featured Score phần không ghim: `0.5×ln(1+Like) + 0.3×ln(1+Reply) + 0.2×e^(-AgeHours/72)`.
- Initial root comments: **10**; lazy load **10/lần**.
- Initial replies: **3**; “Xem thêm phản hồi” tải tối đa **10/lần**, phần còn lại <10 thì tải full.

### 5.3. Comment/Reply/Nickname

- Comment và Reply: **1–1000 ký tự**, emoji-only hợp lệ, whitespace-only không hợp lệ.
- URL chỉ cho `mytv.com.vn` và subdomain thực của domain này.
- Rate limit chung Comment+Reply: **5 nội dung/1 phút/user**.
- Nickname unique không phân biệt hoa/thường, 3–30 ký tự; cho chữ/số/khoảng trắng/`_`/`-`, không URL/phone/control char.
- Nickname đổi không giới hạn số lần nhưng phải AI moderation; pending thì giữ nickname cũ.
- Không có nickname hợp lệ → giữ `0` đầu + 3 số cuối, mask toàn bộ số giữa bằng `*`.

### 5.4. Rating

- Một rating hiện hành/account/scope; đổi rating không tăng tổng lượt; user được xóa rating.
- Average hiển thị **1 chữ số thập phân và luôn làm tròn lên** đến 0.1.
- Khóa bình luận không loại rating; khóa toàn account loại rating khỏi aggregate và mở khóa thì tự tính lại nếu record còn.

### 5.5. Edit/Delete và timestamp

- User sửa comment/reply bất kỳ lúc nào; bản cũ vẫn public khi bản mới pending.
- Bản sửa được duyệt có nhãn **“Đã chỉnh sửa”**; user không xem version history, CMS/Audit xem được.
- User tự xóa không cần reason và không tự restore; Admin có thể Undo khi dữ liệu còn retention.
- MVP không có frame/clip. US06 chỉ gắn tối đa **1 timestamp/comment hoặc reply**; user lấy current time hoặc chỉnh tay.
- Bấm timestamp → player seek tới mốc và tiếp tục phát; share deep link không tự seek.

### 5.6. Like/Mention/Notification

- User được Like content của chính mình.
- Like/Unlike dùng optimistic UI; client gom batch tối đa **5 giây** rồi sync BE, flush sớm khi rời màn hình/background; BE là source of truth.
- Mention suggestion ưu tiên user trong thread, sau đó user đã tham gia phim/tập.
- Một switch chung bật/tắt notification tương tác cộng đồng; in-app retention **90 ngày**.
- Notification moderation/chế tài/appeal/kết quả Report là bắt buộc và không bị switch cộng đồng tắt.

### 5.7. Report

- Taxonomy chung: Spoiler; Spam/quảng cáo; Xúc phạm/ngôn từ công kích; Nội dung không phù hợp; Sai thông tin; Vi phạm khác.
- Chọn “Khác” bắt buộc description.
- Không được Report content của chính mình.
- Cùng target được Report lại sau **24 giờ**; rate limit **10 Report/1 giờ/user**.
- Report không tự Ẩn/Xóa content dù số lượng lớn.
- Reporter nhận **in-app notification** khi Report được xử lý, không lộ sanction chi tiết.
- CMS dùng một action **“Bỏ qua Report”** cho kết luận không vi phạm; không có “Duyệt giữ nguyên” riêng.

### 5.8. AI moderation và state transition

- Risk: **Nhẹ → giữ nguyên text/Hiển thị ở Mode1; Trung bình → Chờ duyệt; Nặng → chặn**.
- Mode2: Nhẹ/Trung bình đều Chờ Admin; Nặng chặn.
- AI timeout **5 giây**/lỗi/down → Chờ duyệt Admin.
- MVP AI language: Việt + Anh; ngôn ngữ khác/low-confidence → Chờ duyệt.
- Threshold có default hệ thống, series override, episode override; mọi thay đổi audit.
- Admin có feedback **“AI phân loại sai”** để lưu corrected result.
- Mode2→Mode1: pending có AI safe/Nhẹ tự Hiển thị; Medium tiếp tục pending.
- Mode1→Mode2: content đã Hiển thị vẫn giữ; mode mới chỉ áp dụng dữ liệu mới từ effective time.
- Đóng bình luận giữ queue; Admin vẫn xử lý nhưng item được duyệt không public tới khi mở lại.
- “Sau X giờ” cho Admin chọn: giờ phát hành MyTV / giờ phát sóng / mốc Admin nhập.

### 5.9. CMS moderation

- Queue mặc định: **risk cao trước**, cùng risk thì item chờ lâu hơn trước.
- PII đầy đủ được Admin/Moderator có quyền CMS xem trong scope được cấp.
- Export CSV + XLSX theo filter; mặc định không có full PII, muốn export PII phải tick chủ động.
- Duyệt không cần reason. Chỉ **Từ chối/Ẩn/Xóa** bắt buộc reason taxonomy chuẩn + note optional.
- Bỏ qua Report/Flag/Spoiler không bắt buộc reason.
- Từ chối/Ẩn/Xóa → tác giả thấy reason trong app + nhận push/in-app notification.
- Bulk moderation hard max **100**, partial success; reason chung cho batch và có thể override từng item.
- Undo Từ chối/Ẩn/Xóa mềm khi còn retention; mọi Moderator có quyền tương ứng được Undo; Undo không bắt reason nhưng tạo audit event mới.
- Moderation pending SLA **24 giờ**; quá SLA vẫn pending, đánh dấu Quá SLA và ưu tiên queue, không auto-approve/reject.

### 5.10. Sanction/Appeal/Audit

- Nhẹ → Cảnh báo; Trung bình → Khóa bình luận tạm; Nặng → Admin chọn Khóa bình luận hoặc Khóa tài khoản theo bối cảnh.
- Khóa bình luận chặn Comment+Reply+Mention; vẫn cho Like/Report/Rating/Share. Không có khóa bình luận vĩnh viễn.
- Preset khóa bình luận: 10 phút, 1 giờ, 1 ngày, 3 ngày, 7 ngày, 1 tháng hoặc custom.
- Khóa tài khoản chỉ cho vi phạm chính sách nghiêm trọng; mọi Admin/Moderator có quyền moderation đều được thực hiện.
- Khóa account có temporary cùng preset/custom hoặc Permanent.
- Khóa account tự Ẩn toàn bộ comment/reply public của user; mở khóa tự public lại item chỉ bị Ẩn do account lock nếu content vẫn hợp lệ.
- User bị khóa bình luận/account được Appeal trong app; SLA **48 giờ**. Quá SLA giữ sanction, đánh dấu Quá SLA và ưu tiên queue.
- Audit log retention **2 năm**, độc lập soft-delete 90 ngày.

### 5.11. Huy hiệu

- Fan tích cực: rolling 30d, ≥7 active days + ≥10 comment/reply hợp lệ.
- Fan trung thành: rolling 90d, ≥30 active days + ≥30 comment/reply hợp lệ + ≥30 Like nhận.
- Auto badge evaluation mỗi ngày; không đạt thì grace **7 ngày** trước khi thu hồi Fan badge.
- UI hiển thị tối đa **1 badge**; thứ tự ưu tiên cuối cùng: **Admin/Chuyên gia > Bình luận nổi bật > Fan trung thành > Fan tích cực**.
- Bình luận nổi bật: comment public ≥20 Like + ≥5 Reply + Top10% Featured Score toàn MyTV trong 30 ngày tại thời điểm xét; đã cấp thì không thu hồi chỉ do ranking tụt, nhưng thu hồi nếu source comment không còn hợp lệ.
- Badge Admin/Chuyên gia có expiry tùy chọn.
- Grant/revoke gửi push + in-app theo notification cộng đồng.
- Tắt badge type → ẩn UI ngay nhưng giữ history/audit.

### 5.12. Share

- Share là interaction, người gửi phải login; người nhận logout vẫn đọc được comment public.
- MVP dùng **OS share sheet**, không direct SDK Facebook/Zalo/TikTok.
- Share text không chứa comment; CTA chính thức **“Xem nội dung này trên MyTV”**.
- Preview: poster/thumbnail + tên phim/tập + CTA + logo MyTV; không comment/Spoiler/PII.
- Deep link không hết hạn; chưa cài app → mở MyTV Web đúng context.
- Target Ẩn/Xóa → fallback phim/tập + thông báo **“Bình luận không còn khả dụng”**.
- Comment có timestamp: share mở đúng comment nhưng không auto-seek player.

### 5.13. Analytics và AI Ops

- Engagement Score: `Comment×2 + Reply×2 + Net Like×1 + Rating×1 + Share×2`.
- Nội dung Ẩn/Từ chối/Xóa do vi phạm không được tính vào Engagement chính thức.
- Dashboard freshness tối đa **5 phút**; daily reconciliation tự sửa aggregate lệch và ghi log.
- Unique commenters: 1 account/1 scope filter/1 khoảng thời gian.
- Dashboard hiển thị cả total Like actions và Net Like; Engagement chỉ dùng Net Like.
- Export dashboard/report CSV + XLSX.
- KPI MVP giữ cả interaction + confirmed Report rate + hidden/deleted/rejected + AI auto-display rate + queue processing time.
- AI Ops chỉ chạy khi Admin bấm **“AI đề xuất”**; input chỉ từ phim/tập hiện tại.
- AI candidate ưu tiên quality + relevance + Like + Reply; loại risk nghiêm trọng.
- AI không tự ghim/đăng; Admin có thể chỉnh câu hỏi → duyệt → đăng, không cần reviewer thứ hai.
- KPI AI: tỷ lệ accept / edit / discard; KPI thấp không tự disable feature.

## 6. Điểm PO còn lại sau vòng refinement

- **Không còn blocker PO đang mở trong 20 User Story theo vòng refinement hiện tại.**

Các nội dung tiếp theo nên là thiết kế UI, data dictionary, technical implementation hoặc policy vận hành chi tiết dựa trên các business rule đã khóa; chỉ mở lại quyết định PO nếu phát hiện hành vi sản phẩm mới hoặc mâu thuẫn mới trong refinement/implementation.

## 7. Nguyên tắc refinement

Mỗi User Story là một lát cắt giá trị ở cấp PO. Nếu story quá lớn cho một sprint, có thể tách delivery story nhưng phải giữ nguyên business rule/acceptance đã khóa. Không tách web/mobile/API/database thành User Story riêng nếu chỉ là phần kỹ thuật để hoàn thành cùng giá trị.