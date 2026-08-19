# US11 — AI kiểm duyệt theo hai chế độ

> Thuộc EP03 — An toàn và kiểm duyệt
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là bộ phận vận hành MyTV**, tôi muốn AI kiểm tra nội dung và hỗ trợ hai chế độ kiểm duyệt, để cân bằng tốc độ hiển thị và an toàn theo từng phim/tập.

### Ưu tiên

**Must**

### Phạm vi

- Comment gốc, Reply, phiên bản sửa: áp dụng moderation theo **Mode1/Mode2 của scope series/episode**.
- Nickname mới/đổi: áp dụng **global AI moderation policy riêng**, độc lập Mode1/Mode2 và override theo phim/tập; chỉ có 2 kết quả (dùng ngay hoặc chặn ngay), không có trạng thái Chờ duyệt.
- MVP AI language: **Tiếng Việt + Tiếng Anh**. Ngôn ngữ khác hoặc độ tin cậy không đủ → Chờ duyệt Admin.

### Taxonomy chung

- Spoiler
- Spam/quảng cáo
- Xúc phạm/ngôn từ công kích
- Nội dung không phù hợp
- Sai thông tin
- Vi phạm khác

### Ma trận mức rủi ro

Áp dụng cho **Comment/Reply/Edit** (theo Mode1/Mode2 của scope):
- **Nhẹ** → không che từ, không sửa text.
- **Trung bình** → Chờ duyệt Admin.
- **Nặng** → chặn, không cho gửi/public version mới.

Nickname dùng **chính sách global riêng, KHÔNG theo ma trận 3 mức ở trên** — chỉ 2 kết quả: Nhẹ/An toàn dùng ngay; Trung bình hoặc Nặng (gộp) đều bị chặn ngay tại thời điểm submit, không có trạng thái Chờ duyệt (xem mục "Global policy — Nickname").

> *Xem thêm: [REQUIREMENTS_A11Y_SECURITY.md](../REQUIREMENTS_A11Y_SECURITY.md) mục Security (XSS/Injection) — AI moderation phân loại ngữ nghĩa nội dung, KHÔNG phải hàng rào chống injection; kết quả rủi ro "Nhẹ" không đồng nghĩa nội dung an toàn để render, mọi output vẫn phải qua sanitize/escape ở tầng hiển thị.*

### Acceptance Criteria chung

1. Mọi comment/reply/version sửa và nickname mới/đổi được AI kiểm tra trước khi trở thành dữ liệu public mới.
2. AI lưu taxonomy label, mức Nhẹ/Trung bình/Nặng, reason, model/policy version và thời gian xử lý để CMS tra cứu.
3. Với **Comment/Reply/Edit**, ngưỡng phân loại có default toàn hệ thống do Admin cấu hình trên CMS; series có thể override default và episode có thể override series.
4. Với **Nickname**, hệ thống dùng **global nickname AI policy** với 2 kết quả (Nhẹ/An toàn dùng ngay; Trung bình hoặc Nặng bị chặn ngay, không có trạng thái Chờ duyệt); nickname không kế thừa Mode1/Mode2 hoặc threshold override series/episode.
5. Mọi thay đổi threshold/policy có audit và chỉ áp dụng từ thời điểm hiệu lực.
6. AI có timeout tối đa **5 giây**. Timeout/5xx/không khả dụng → dữ liệu được nhận ở trạng thái **Chờ duyệt Admin**, không tự công khai.
7. Nội dung ngoài tiếng Việt/Anh hoặc AI không đủ confidence → Chờ duyệt Admin.
8. CMS có action **“AI phân loại sai”** để Admin ghi kết quả đúng; lưu AI result ban đầu, kết quả sửa, actor/time nhằm đo false positive/false negative.
9. Retry AI/client không tạo comment/version/nickname queue item trùng.

### Chế độ 1 — AI tiền kiểm, Admin hậu kiểm

Áp dụng **Comment/Reply/Edit** trong scope đang Mode1:
1. Mức Nhẹ/An toàn → Hiển thị ngay, giữ nguyên text.
2. Mức Trung bình → Chờ duyệt, chỉ tác giả thấy.
3. Mức Nặng → chặn không cho gửi/public version mới.
4. Admin vẫn có thể hậu kiểm nội dung đã Hiển thị và xử lý queue.

### Chế độ 2 — Admin duyệt trước

Áp dụng **Comment/Reply/Edit** trong scope đang Mode2:
1. Mức Nặng → chặn.
2. Mức Nhẹ hoặc Trung bình không tự public; đều vào Chờ duyệt Admin.
3. Chỉ Admin Duyệt mới chuyển Hiển thị.

### Global policy — Nickname

Nickname KHÔNG dùng trạng thái Chờ duyệt; chỉ có **2 kết quả**, quyết định ngay tại thời điểm submit:

1. Nickname mức **Nhẹ/An toàn** → dùng ngay, cập nhật nickname công khai ngay lập tức.
2. Nickname mức **Trung bình HOẶC Nặng** (gộp) → **bị chặn ngay tại thời điểm submit**, không tạo bất kỳ hàng chờ nào; user thấy lỗi ngay; nickname không đổi; tiếp tục hiển thị nickname hợp lệ cũ, hoặc fallback mask số điện thoại nếu chưa từng có nickname hợp lệ theo US04.
3. Kết quả này **không thay đổi** theo Mode1/Mode2 của phim/tập nơi user đang thao tác.

### Quy tắc nghiệp vụ

- Chế độ 1 là mặc định cho **content scope**.
- Không còn hành vi tự che `***` cho mức Nhẹ trong policy hiện tại.
- Nickname là identity toàn account nên dùng global policy riêng; không lấy episode/series mode làm đầu vào quyết định nickname; nickname KHÔNG có trạng thái Chờ duyệt (chỉ 2 kết quả: dùng ngay hoặc chặn ngay khi submit).
- AI hỗ trợ quyết định; Admin có quyền override theo policy và feedback phải được lưu.
- Fallback AI lỗi luôn fail-safe về queue, không fail-open.
- Edit (version sửa) áp dụng rate limit riêng: tối đa **5 lần sửa/phút/comment hoặc reply**, tính độc lập theo từng target; request vượt ngưỡng bị chặn trước khi gọi AI/tạo queue item.


### Điểm cần PO chốt

- Không còn blocker PO cho ma trận AI moderation trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US11-001 | Coverage | Comment/reply/edit/nickname | Gửi từng loại | Mọi dữ liệu public mới đi AI trước. |
| TC-US11-002 | Mode1/light | Mode1, AI Nhẹ | Gửi Comment/Reply/Edit | Nội dung giữ nguyên text và Hiển thị. |
| TC-US11-003 | Mode1/medium | Mode1, AI Trung bình | Gửi Comment/Reply/Edit | Chờ duyệt; chỉ tác giả thấy. |
| TC-US11-004 | Heavy | Content Mode1/2, AI Nặng | Gửi Comment/Reply/Edit | Bị chặn; không tạo dữ liệu public mới. |
| TC-US11-005 | Mode2 | AI Nhẹ/Trung bình | Gửi Comment/Reply/Edit | Cả hai vào queue; chỉ Admin duyệt mới public. |
| TC-US11-006 | Nickname global policy | E1 Mode1, E2 Mode2; cùng mẫu nickname Nhẹ/An toàn và Trung bình/Nặng | Đổi nickname từ E1/E2 | Kết quả giống nhau ở mọi scope, không có trạng thái Chờ duyệt: Nhẹ/An toàn dùng ngay; Trung bình hoặc Nặng đều bị chặn ngay tại thời điểm submit, nickname không đổi, tiếp tục hiển thị nickname hợp lệ cũ/mask số điện thoại; Mode1/2 không ảnh hưởng kết quả. |
| TC-US11-007 | Timeout | AI >5s/5xx/down | Gửi content/nickname | Dữ liệu vào Chờ duyệt; không fail-open. |
| TC-US11-008 | Language | VI/EN/ngôn ngữ khác | Gửi | VI/EN xử lý AI; ngôn ngữ ngoài phạm vi hoặc low-confidence vào queue. |
| TC-US11-009 | Threshold inheritance | Có default, series override, episode override | Gửi cùng mẫu Comment/Reply/Edit ở các scope | Episode ưu tiên override riêng; nếu không có thì series; nếu không có thì default. Nickname không dùng inheritance này. |
| TC-US11-010 | Policy audit | Đổi content threshold/global nickname policy | Tra audit và request trước/sau effective time | Có actor/time/before-after; mỗi decision gắn đúng policy version. |
| TC-US11-011 | Feedback | AI phân loại sai | Admin chọn “AI phân loại sai” và kết quả đúng | Lưu AI result + corrected result + actor/time để đo FP/FN. |
| TC-US11-012 | Idempotency | Client/AI retry | Gửi request lặp | Một comment/version/nickname queue item duy nhất. |
| TC-US11-013 | Edit rate limit | U1 có C1/R1 | Gửi 5 version sửa trên cùng target trong 1 phút rồi gửi lần thứ 6 | 5 lần đầu đi qua pipeline moderation bình thường; lần thứ 6 trên cùng target bị chặn trước khi gọi AI/tạo queue item. C1 và R1 có quota độc lập. |
