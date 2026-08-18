# US20 — AI hỗ trợ vận hành cộng đồng

> Thuộc EP05 — Tăng trưởng và phân tích
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là Admin/biên tập viên nội dung**, tôi muốn chủ động yêu cầu AI đề xuất comment đáng chú ý hoặc câu hỏi thảo luận, để giảm thời gian vận hành nhưng vẫn kiểm soát hoàn toàn nội dung được ghim/đăng.

### Ưu tiên

**Could**

### Acceptance Criteria — Cách gọi AI

1. AI không tự chạy để public nội dung; Admin chủ động bấm **“AI đề xuất”**.
2. Nút AI đề xuất luôn khả dụng cho role được quyền trong scope sản phẩm hiện tại; **không cần toggle bật/tắt tính năng theo KPI**.
3. AI chỉ dùng dữ liệu trong **phim/tập hiện tại**: comment, reply, Like, Rating, Report và metadata nội dung liên quan.
4. Không dùng lịch sử hành vi user trên toàn MyTV hoặc dữ liệu ngoài MyTV làm input cho feature này.

### Acceptance Criteria — Đề xuất comment nổi bật

1. AI chỉ xét comment đang Hiển thị.
2. Tín hiệu ưu tiên: **chất lượng nội dung + mức liên quan tới phim/tập + Like + Reply**.
3. Loại nội dung Chờ duyệt/Từ chối/Ẩn/Xóa, risk cao, Report nghiêm trọng chưa xử lý hoặc Flag nghiêm trọng theo policy.
4. AI trả rationale/tín hiệu chính để Admin đánh giá.
5. AI **không tự ghim**. Admin xem đề xuất rồi quyết định ghim qua US15 hoặc bỏ qua.
6. Không chỉnh sửa text comment của user khi ghim; nếu cần xử lý nội dung phải đi moderation/edit flow phù hợp.

### Acceptance Criteria — Đề xuất câu hỏi/chủ đề

1. Admin bấm “AI đề xuất” để sinh câu hỏi/chủ đề gắn đúng series/tập.
2. AI proposal phải qua safety/Spoiler check.
3. Admin có thể **chỉnh sửa → duyệt → đăng** ngay; **không cần Admin thứ hai phê duyệt**.
4. Nếu Admin không xác nhận, AI proposal không được public.

### KPI chất lượng AI

- Tỷ lệ Admin **chấp nhận** đề xuất.
- Tỷ lệ Admin **chỉnh sửa** trước khi đăng.
- Tỷ lệ Admin **bỏ** đề xuất.

Các KPI dùng để theo dõi chất lượng, không tự động disable tính năng.

### Quy tắc nghiệp vụ

- Human-in-the-loop bắt buộc: AI đề xuất, Admin quyết định.
- Input bị giới hạn theo phim/tập hiện tại để giảm rủi ro privacy/context leakage.
- AI không tự pin, không tự publish.
- Mọi accept/edit/discard/pin/post liên quan proposal phải tracking để US19 đo hiệu quả.

### Điểm cần PO chốt

- Không còn blocker PO cho workflow AI Ops trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US20-001 | Trigger | Admin có quyền | Không bấm AI đề xuất rồi theo dõi | Không có proposal/public action tự phát. |
| TC-US20-002 | Input scope | E1 có dữ liệu và account có lịch sử ở E2/khác | Bấm AI đề xuất ở E1 | Input/output chỉ dựa dữ liệu E1/phim hiện tại theo scope được phép. |
| TC-US20-003 | Candidate | Có comment khác nhau quality/relevance/Like/Reply | Bấm AI đề xuất | Ranking proposal phản ánh bốn nhóm tín hiệu và có rationale. |
| TC-US20-004 | Exclusion | Có pending/rejected/hidden/deleted/high-risk/severe-report | Bấm AI đề xuất | Các item bị loại không xuất hiện trong candidate list. |
| TC-US20-005 | No auto-pin | Có candidate tốt | Không thao tác Admin | Không thay đổi pin state. |
| TC-US20-006 | Pin decision | Admin chấp nhận candidate | Ghim qua US15 | Pin chỉ thay đổi sau thao tác Admin; text user không bị rewrite. |
| TC-US20-007 | Question workflow | Admin bấm AI đề xuất câu hỏi | Edit → Approve → Post | Nội dung chỉ public sau Admin xác nhận; không cần reviewer thứ hai. |
| TC-US20-008 | Safety | Proposal có Spoiler/risk | Safety check | Proposal bị chặn/cảnh báo/chỉnh theo policy trước khi Admin đăng. |
| TC-US20-009 | KPI tracking | Accept/edit/discard nhiều proposal | Mở metric | Tính đúng ba tỷ lệ KPI chất lượng. |
| TC-US20-010 | No disable | KPI discard cao | Kiểm tra CMS | Feature không tự disable và không có toggle quality-based trong scope hiện tại. |
