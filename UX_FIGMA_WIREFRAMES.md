# UX Figma Wireframes — MyTV Comment

**Trạng thái:** Low-fi đang triển khai  
**Figma file:** [Wireframe — Bình luận MyTV](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=0-1)  
**Nguồn logic:** `UX_USER_FLOWS.md` + `UX_INFORMATION_ARCHITECTURE_SCREEN_MAP.md`

> Wireframe dùng grayscale, mobile-first để kiểm tra cấu trúc, hierarchy, state và luồng trước khi đi high-fi. Visual style/component cuối cùng chưa được khóa ở giai đoạn này.

---

## P0 — Connected User Flow · Mobile v1

**User Flow chính:** [MyTV Comment — P0 Connected User Flow Mobile v1](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=7-2)

Flow này nối toàn bộ P0 thành **một hành trình có nhánh**, thay vì chỉ đặt các screen độc lập cạnh nhau:

`Content Detail / Comment Area → Thread → Interaction → Auth/Composer/Action → Moderation result`

Các nhánh ngoại lệ được nối vào cùng flow:

- Guest interaction → Auth Gate → quay lại đúng context, **không auto-execute action cũ**.
- Comment/Reply/Edit → moderation → Pending khi applicable.
- Scope Đóng → Comment Area Closed.
- Target moderation/lifecycle invalid → Target unavailable (`S03` — “Bình luận không còn khả dụng”).
- Account Lock của tác giả/root author → Bình luận hiện không khả dụng (`S04` — trạng thái **tạm thời**, link hoạt động lại sau khi gỡ khóa).
- Comment Lock → Interaction blocked.
- Thread actions → Edit / Delete / Report.

---

## P0 — Screen Inventory · Mobile v1

**Section reference:** [MyTV Comment — P0 Low-fi Mobile v1](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=3-2)

Screen Inventory được giữ lại làm thư viện/reference để Design/Dev/QA mở trực tiếp từng state. Source-of-truth để review flow P0 là **Connected User Flow** ở trên.

### Core screens

| Screen ID | Wireframe | Coverage chính |
|---|---|---|
| C01/C02 | [Content Detail / Watch + Comment Area](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=3-5) | US01, US02, US03, US04, US07, US08 |
| C05 | [Focused Thread / Deep-link target](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=3-48) | US01, US08, US09, US18 |
| C06 | [Comment Composer](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=3-93) | US04, US06, US11 |
| C07 | [Reply Composer](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=3-115) | US08, US09, US11 |
| O01 | [Auth Gate — Guest interaction](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=3-139) | US01, US03, US04, US07, US08, US10, US18 |

### Actions & exception states

| Screen ID | Wireframe | Coverage chính |
|---|---|---|
| S06 | [Pending moderation — author only](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=4-2) | US04, US05, US11, US12 |
| S05 | [Comment Area closed](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=4-20) | US01, US12, US18 |
| S03 | [Target no longer available](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=4-33) | US12, US18 |
| S04 | Target temporarily unavailable — Account Lock của tác giả *(frame chưa dựng trong file Figma)* | US12, US16, US18 |
| O04 | [Edit Comment / Reply](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=4-47) | US05, US11 |
| O05 | [Delete confirmation — root cascade](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=4-64) | US05, US08 |
| O06 | [Report sheet](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=4-79) | US10 |
| S07 | [Comment Lock — interaction blocked](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=4-110) | US05, US16 |

---

## Các rule UX đã thể hiện trong P0

- Guest được đọc nhưng interaction đi qua Auth Gate; sau login quay lại đúng context và **không auto-execute action cũ**.
- Comment Area giữ rõ context nội dung hiện tại: **phim lẻ** (comment/rating cấp phim) hoặc **tập đang xem** của phim bộ. Không có scope Series và không có bộ chuyển Series↔Tập phía người xem; CMS vẫn cấu hình được ở cấp series.
- Comment là **tab riêng** trong trang nội dung; public count nằm ở nhãn tab `Bình luận ({count})`.
- Pending moderation chỉ tác giả thấy trạng thái; không làm tăng public count.
- Scope Đóng giữ tab `Bình luận` nhưng **bỏ count**, ẩn rating/list/composer/interaction; user vẫn ở đúng content context.
- Deep link target bị moderation/lifecycle không trả 404 và không lộ nội dung cũ.
- Ba chuỗi fallback là **ba frame khác nhau**, không tái sử dụng lẫn nhau: `S05` scope Đóng → “Khu vực bình luận hiện không khả dụng” (giữ tab `Bình luận`, bỏ count); `S04` Account Lock → “Bình luận hiện không khả dụng” (tạm thời); `S03` moderation/self-delete/Admin cascade → “Bình luận không còn khả dụng” (target hiện không khả dụng; self-delete không Undo, Admin root delete có thể Undo trong 90 ngày theo lifecycle).
- Edit giữ public version cũ trong lúc version mới chờ moderation.
- Self-delete root cảnh báo cascade toàn thread và **không thể Undo public lại**.
- Report là tín hiệu; gửi Report không tự động ẩn comment.
- Comment Lock chặn Comment/Reply/Mention/Edit nhưng vẫn cho self-delete/Like/Report/Rating/Share theo rule hiện hành.

---

## Tiếp theo

- P1 Consumer: Rating detail, Like optimistic/reconcile, Timestamp entitlement, Notification Center, Share/deep-link states.
- P2 Consumer: Comment Lock appeal, Account Lock full-screen, Badge states.
- CMS P0: Moderation Queue, Search, Comment Detail, single/bulk moderation, Pin/Config.
- Sau khi mobile P0 được review: dựng responsive Web low-fi dựa trên cùng Screen ID/flow logic.
