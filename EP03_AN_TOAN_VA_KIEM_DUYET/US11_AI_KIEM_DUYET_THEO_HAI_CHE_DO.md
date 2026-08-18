# US11 — AI kiểm duyệt theo hai chế độ

> Thuộc EP03 — An toàn và kiểm duyệt
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là bộ phận vận hành MyTV**, tôi muốn AI kiểm tra nội dung và hỗ trợ hai chế độ kiểm duyệt, để cân bằng tốc độ hiển thị và an toàn theo từng phim/tập.

### Ưu tiên

**Must**

### Phạm vi

- Comment gốc, Reply, phiên bản sửa.
- Nickname mới/đổi.
- MVP AI language: **Tiếng Việt + Tiếng Anh**. Ngôn ngữ khác hoặc độ tin cậy không đủ → Chờ duyệt Admin.

### Taxonomy chung

- Spoiler
- Spam/quảng cáo
- Xúc phạm/ngôn từ công kích
- Nội dung không phù hợp
- Sai thông tin
- Vi phạm khác

### Ma trận mức rủi ro

- **Nhẹ** → không che từ, không sửa text.
- **Trung bình** → Chờ duyệt Admin.
- **Nặng** → chặn, không cho gửi/public version mới.

### Acceptance Criteria chung

1. Mọi comment/reply/version sửa/nickname mới được AI kiểm tra trước khi trở thành dữ liệu public mới.
2. AI lưu taxonomy label, mức Nhẹ/Trung bình/Nặng, reason, model/policy version và thời gian xử lý để CMS tra cứu.
3. Ngưỡng phân loại có **default toàn hệ thống** do Admin cấu hình trên CMS; series có thể override default và episode có thể override series.
4. Mọi thay đổi threshold/policy có audit và chỉ áp dụng từ thời điểm hiệu lực.
5. AI có timeout tối đa **5 giây**. Timeout/5xx/không khả dụng → nội dung được nhận ở trạng thái **Chờ duyệt Admin**, không tự công khai.
6. Nội dung ngoài tiếng Việt/Anh hoặc AI không đủ confidence → Chờ duyệt Admin.
7. CMS có action **“AI phân loại sai”** để Admin ghi kết quả đúng; lưu AI result ban đầu, kết quả sửa, actor/time nhằm đo false positive/false negative.
8. Retry AI/client không tạo comment/version/queue item trùng.

### Chế độ 1 — AI tiền kiểm, Admin hậu kiểm

1. Mức Nhẹ/An toàn → Hiển thị ngay, giữ nguyên text.
2. Mức Trung bình → Chờ duyệt, chỉ tác giả thấy.
3. Mức Nặng → chặn không cho gửi/public version mới.
4. Admin vẫn có thể hậu kiểm nội dung đã Hiển thị và xử lý queue.

### Chế độ 2 — Admin duyệt trước

1. Mức Nặng → chặn.
2. Mức Nhẹ hoặc Trung bình không tự public; đều vào Chờ duyệt Admin.
3. Chỉ Admin Duyệt mới chuyển Hiển thị.

### Quy tắc nghiệp vụ

- Chế độ 1 là mặc định.
- Không còn hành vi tự che `***` cho mức Nhẹ trong policy hiện tại.
- AI hỗ trợ quyết định; Admin có quyền override theo policy và feedback phải được lưu.
- Fallback AI lỗi luôn fail-safe về queue, không fail-open.

### Điểm cần PO chốt

- Không còn blocker PO cho ma trận AI moderation trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US11-001 | Coverage | Comment/reply/edit/nickname | Gửi từng loại | Mọi dữ liệu public mới đi AI trước. |
| TC-US11-002 | Mode1/light | Mode1, AI Nhẹ | Gửi | Nội dung giữ nguyên text và Hiển thị. |
| TC-US11-003 | Mode1/medium | Mode1, AI Trung bình | Gửi | Chờ duyệt; chỉ tác giả thấy. |
| TC-US11-004 | Heavy | Mode1/2, AI Nặng | Gửi | Bị chặn; không tạo nội dung public mới. |
| TC-US11-005 | Mode2 | AI Nhẹ/Trung bình | Gửi | Cả hai vào queue; chỉ Admin duyệt mới public. |
| TC-US11-006 | Timeout | AI >5s/5xx/down | Gửi | Nội dung vào Chờ duyệt; không fail-open. |
| TC-US11-007 | Language | VI/EN/ngôn ngữ khác | Gửi | VI/EN xử lý AI; ngôn ngữ ngoài phạm vi hoặc low-confidence vào queue. |
| TC-US11-008 | Threshold inheritance | Có default, series override, episode override | Gửi cùng mẫu ở các scope | Episode ưu tiên override riêng; nếu không có thì series; nếu không có thì default. |
| TC-US11-009 | Policy audit | Đổi threshold/policy | Tra audit và request trước/sau effective time | Có actor/time/before-after; mỗi decision gắn đúng policy version. |
| TC-US11-010 | Feedback | AI phân loại sai | Admin chọn “AI phân loại sai” và kết quả đúng | Lưu AI result + corrected result + actor/time để đo FP/FN. |
| TC-US11-011 | Idempotency | Client/AI retry | Gửi request lặp | Một comment/version/queue item duy nhất. |
