# UX Figma Wireframes — MyTV Comment

**Trạng thái:** Low-fi đang triển khai  
**Figma file:** [Wireframe — Bình luận MyTV](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=0-1)  
**Nguồn logic:** `UX_USER_FLOWS.md` + `UX_INFORMATION_ARCHITECTURE_SCREEN_MAP.md`

> Wireframe dùng grayscale, mobile-first để kiểm tra cấu trúc, hierarchy, state và luồng trước khi đi high-fi. Visual style/component cuối cùng chưa được khóa ở giai đoạn này.

---

## P0 — Core Consumer Loop · Mobile v1

**Section Figma:** [MyTV Comment — P0 Low-fi Mobile v1](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=3-2)

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
| O04 | [Edit Comment / Reply](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=4-47) | US05, US11 |
| O05 | [Delete confirmation — root cascade](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=4-64) | US05, US08 |
| O06 | [Report sheet](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=4-79) | US10 |
| S07 | [Comment Lock — interaction blocked](https://www.figma.com/design/drR51WLeN2FkGZbN4FRqFY/Wireframe?node-id=4-110) | US05, US16 |

---

## Các rule UX đã thể hiện trong P0

- Guest được đọc nhưng interaction đi qua Auth Gate; sau login quay lại đúng context và **không auto-execute action cũ**.
- Comment Area giữ rõ context **Series / Episode**.
- Pending moderation chỉ tác giả thấy trạng thái; không làm tăng public count.
- Scope Đóng ẩn comment area/rating/interaction nhưng user vẫn ở đúng content context.
- Deep link target bị moderation/lifecycle không trả 404 và không lộ nội dung cũ.
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
