# US12 — Quản lý trạng thái và phạm vi hiển thị bình luận

> Thuộc EP03 — An toàn và kiểm duyệt
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là Admin vận hành**, tôi muốn hệ thống áp dụng trạng thái và phạm vi hiển thị theo cấu hình từng phim/tập, để kiểm soát chính xác ai được xem nội dung ở từng giai đoạn moderation.

### Ưu tiên

**Must**

### Trạng thái nghiệp vụ

| Trạng thái | Tác giả | Cộng đồng | Ý nghĩa |
|---|---:|---:|---|
| Chờ duyệt | Có | Không | Chờ Admin quyết định |
| Hiển thị | Có | Có (*) | Public |
| Từ chối | Có + lý do | Không | Không được duyệt |
| Ẩn | Có + lý do | Không | Admin ẩn khỏi cộng đồng |
| Xóa mềm | Không trong public UI | Không | Giữ dữ liệu theo retention |

(*) **Hiển thị = đủ điều kiện public NẾU KHÔNG có gate hiển thị cao hơn đang active.** `moderation_state = Hiển thị` là điều kiện cần nhưng không phải điều kiện đủ để cộng đồng thực sự xem được nội dung — nếu có Account Lock của tác giả/root author hoặc scope Đóng bình luận đang active, comment vẫn ở `moderation_state = Hiển thị` (không đổi state) nhưng **không public tạm thời** với cộng đồng. Xem đầy đủ thứ tự ưu tiên tại mục "Effective Visibility Resolver" ngay dưới đây.

### Effective Visibility Resolver — thứ tự ưu tiên khi nhiều gate cùng đúng

Một comment/thread có thể rơi vào từ 2 "gate ẩn" trở lên cùng lúc (ví dụ vừa bị Account Lock của tác giả vừa nằm trong scope đang Đóng). Khi đó hệ thống **CHỈ hiển thị một thông báo duy nhất**, chọn theo gate có **mức ưu tiên cao nhất** (số nhỏ hơn = ưu tiên cao hơn = "vĩnh viễn/nghiêm trọng hơn"). Đây là định nghĩa gốc — US01, US02, US08, US09, US18, US19 chỉ dẫn chiếu về mục này, không lặp lại toàn bộ luật.

| Ưu tiên | Gate | Thông báo fallback |
|---|---|---|
| 1 | Self-delete (root cascade **hoặc reply tự xóa riêng lẻ**) | **"Bình luận không còn khả dụng"** |
| 2 | Moderation state riêng (Ẩn/Xóa mềm/Từ chối do CMS/Admin) | **"Bình luận không còn khả dụng"** |
| 3 | Admin root moderation cascade | **"Bình luận không còn khả dụng"** |
| 4 | Account Lock (tác giả/root author) | **"Bình luận hiện không khả dụng"** |
| 5 | Scope Đóng bình luận | **"Khu vực bình luận hiện không khả dụng"** |

Quy tắc chọn thông báo:
- Nếu nhiều gate cùng đúng cho cùng một comment/thread, hệ thống dùng thông báo của gate có **số ưu tiên nhỏ nhất** trong bảng trên (gate 2/3 thắng gate 4, gate 4 thắng gate 5).
- Gate 1, 2 và 3 dùng chung chuỗi hiển thị "Bình luận không còn khả dụng" — self-delete không được khôi phục public, còn moderation riêng/Admin root cascade áp dụng semantics tương ứng tại US14/US05. Các gate này ưu tiên hơn gate 4 và 5.
- Gate chỉ ảnh hưởng đến **thông báo hiển thị/khả năng public**, KHÔNG ảnh hưởng `moderation_state` lưu trữ của comment; việc "thắng" trong bảng ưu tiên chỉ quyết định UI/thông báo nào được người dùng nhìn thấy tại một thời điểm, không xóa hay ghi đè trạng thái của các gate khác.
- Việc mở lại một gate có ưu tiên thấp hơn (ví dụ mở lại scope Đóng) không tự động làm nội dung public nếu vẫn còn gate có ưu tiên cao hơn đang active (ví dụ vẫn còn Account Lock hoặc vẫn Ẩn/Từ chối).
- **Phạm vi áp dụng của các chuỗi fallback trên:** chỉ dùng khi hệ thống phải giải quyết **một target cụ thể** — deep link, share link, notification, mở thread trực tiếp. Trong **danh sách bình luận**, nội dung không đủ điều kiện public bị **bỏ khỏi list, không để placeholder** (US05, US08). Hai hành vi này không xung đột: một là "người dùng yêu cầu đúng comment này", một là "hệ thống dựng danh sách".

### Mô hình dữ liệu của gate — nguồn chân lý duy nhất

Toàn bộ tài liệu khác (US05, US08, US14, US15, `SOLUTION_ARCHITECTURE.md`) **chỉ dẫn chiếu mục này** và không được tự định nghĩa tên trường.

| Khái niệm | Lưu ở đâu | Giá trị | Dùng cho |
|---|---|---|---|
| `moderation_state` | trên chính comment/reply | Chờ duyệt · Hiển thị · Từ chối · Ẩn · Xóa mềm | gate 2 |
| `cascade_source` | trên reply, **chỉ cho delete cascade** | `none` · `self_delete` · `admin_root_delete` | gate 1 và gate 3 khi là delete cascade |
| Visibility gate (Admin Ẩn root, Account Lock, scope Đóng) | **không lưu trên comment/reply** | — | gate 3 (hide), 4, 5 |

Hai quy tắc bắt buộc:

1. **Visibility gate được Resolver tính runtime**, từ: `moderation_state` của chính item, `cascade_source`, `moderation_state` của root, trạng thái lock của tác giả và của root author, và `status` của content scope. Cố ý **không** denormalize gate xuống hàng reply — `status` của scope là thuộc tính của phim/tập, nếu nhân bản xuống từng reply thì mỗi lần Admin Đóng/Mở một tập phải ghi lại toàn bộ reply của tập đó, và mọi lần ghi lỗi giữa đường là một reply hiển thị sai. Đây đúng là lớp lỗi mà AD-01 được viết ra để loại bỏ.
2. **Không dùng cờ `hidden_by_root_cascade`.** Trường này đã bị bỏ: trước đây nó gộp cả visibility cascade và delete cascade vào một boolean, nên sau khi tách hai lifecycle thì nó không còn biểu diễn được trạng thái nào một cách đúng đắn. Mọi truy vấn phải đi qua Resolver.

### Trạng thái của version sửa

Ngoài `moderation_state` của comment/reply, **mỗi edit version** có state riêng: `pending` · `approved` · `rejected` · `superseded`.

- Mỗi target chỉ có **một version `pending` hiện hành** (US05 AC3). Khi user gửi edit mới hợp lệ, version `pending` cũ chuyển `superseded` **atomically** trong cùng transaction với việc tạo version mới.
- `superseded` là **state cuối**: version đó **bị loại khỏi moderation queue và khỏi mẫu tính SLA moderation**, không thể public, và mọi action Duyệt/Từ chối lên nó **bị chặn ở cả UI và API**, không ghi audit "thành công".
- Chuyển sang `superseded` vẫn ghi một audit event `superseded_by_version = {id}` để CMS/Audit dựng lại được đầy đủ chuỗi V1 → V2 → V3.

### Acceptance Criteria

1. Chờ duyệt chỉ hiển thị với tác giả; không xuất hiện ở public API/count/notification cho người khác.
2. Bản sửa Chờ duyệt không ghi đè version public cũ; nếu bị Từ chối/chặn, version cũ vẫn Hiển thị.
3. Phim/tập có ba trạng thái vận hành: Chế độ 1, Chế độ 2, Đóng bình luận.
4. Khi Đóng, tab `Bình luận` vẫn hiển thị nhưng bỏ count; hiển thị trạng thái **“Khu vực bình luận hiện không khả dụng”**, ẩn danh sách, ô tương tác và rating; UI/API chặn **Comment, Reply, Like, Mention, Report, Rating và Share mới** — **gồm cả post-watch rating prompt (`O11`) dù prompt nằm ngoài tab Bình luận**: player không được mở prompt khi scope đang Đóng, thay vì mở rồi để API từ chối.
5. Đóng bình luận không xóa dữ liệu lịch sử và không đổi moderation state của comment/reply chỉ vì scope bị Đóng.
6. Khi mở lại, dữ liệu đủ điều kiện được hiển thị lại; nội dung đã Từ chối/Ẩn/Xóa vẫn không public.
7. Khi chuyển **Chế độ 2 → Chế độ 1**, comment đang Chờ duyệt mà **đã có AI result an toàn/Nhẹ trước đó** tự chuyển Hiển thị; các case Trung bình/không đủ điều kiện tiếp tục Chờ duyệt.
8. Khi chuyển **Chế độ 1 → Chế độ 2**, nội dung đã Hiển thị trước effective time vẫn giữ Hiển thị; Chế độ 2 áp dụng cho nội dung mới/version mới từ effective time.
9. Khi chuyển sang **Đóng** trong lúc còn queue, queue được giữ và Admin vẫn xử lý trên CMS; item được Duyệt trong lúc Đóng không public cho tới khi mở lại.
10. Deep link cũ từ Share/Notification tới target trong scope đang Đóng vẫn mở đúng phim/tập nhưng **không hiển thị comment/thread**; thông báo fallback chọn theo **Effective Visibility Resolver** — nếu target CHỈ bị gate scope Đóng thì hiển thị **“Khu vực bình luận hiện không khả dụng”**, nếu target còn bị moderation riêng/Admin root cascade/Account Lock thì hiển thị thông báo của gate có ưu tiên cao hơn. Khi mở lại scope, link cũ hoạt động lại nếu target vẫn hợp lệ và không còn gate ưu tiên cao hơn đang active.
11. Rule “sau X giờ” hỗ trợ ba loại mốc và **Admin chọn loại mốc khi cấu hình**: giờ phát hành MyTV, giờ phát sóng thực tế, hoặc mốc Admin nhập thủ công.
12. Thay đổi mode/schedule chỉ áp dụng từ effective time và có timezone nhất quán.
13. Khi comment bị **Từ chối / Ẩn / Xóa**, tác giả thấy lý do cụ thể trong app và nhận notification nghiệp vụ bắt buộc.
14. Mọi transition/configuration được lưu audit.
15. Trong thời gian scope Đóng, việc user chỉ truy cập trang phim/tập **không tạo active day badge** vì khu vực Bình luận không thể được mở/đọc theo định nghĩa US17.

### Quy tắc nghiệp vụ

- Mode1: Nhẹ Hiển thị, Trung bình queue, Nặng chặn theo US11.
- Mode2: Nhẹ/Trung bình queue, Nặng chặn.
- Đóng là lớp visibility/interaction gate; không hủy moderation data và không đồng nghĩa moderation Ẩn/Xóa.
- Deep link trong scope Đóng dùng fallback theo **Effective Visibility Resolver**: nếu target CHỈ bị gate scope Đóng thì hiển thị “Khu vực bình luận hiện không khả dụng”; nếu target còn có moderation riêng (Ẩn/Xóa mềm/Từ chối), Admin root cascade hoặc Account Lock thì dùng thông báo của gate có ưu tiên cao hơn theo bảng ưu tiên ở mục “Effective Visibility Resolver”.
- Notification reason cho Từ chối/Ẩn/Xóa không bị tắt bởi switch thông báo cộng đồng US09.
- Active day chỉ được ghi khi comment area thực sự khả dụng để user đăng nhập mở/đọc.
- Concurrency tại effective time: mọi request submit Comment/Reply/Rating/Share xử lý theo mode/scope **có hiệu lực tại thời điểm hệ thống nhận request**, không theo thời điểm AI trả kết quả — kể cả khi AI xử lý bất đồng bộ và trả kết quả sau khi effective time đã đổi, quyết định mode/scope áp dụng vẫn chốt tại thời điểm nhận request ban đầu. Với Comment/Reply/Edit, threshold/policy version cũng được snapshot tại thời điểm nhận request và lưu cùng record; policy mới không tự reclassify request đang in-flight.
- Chỉ các cặp (state hiện tại → action) hợp lệ mới được thực hiện: Duyệt và Từ chối chỉ áp dụng từ Chờ duyệt; Ẩn chỉ áp dụng từ Hiển thị. Mọi tổ hợp khác bị chặn ở cả UI lẫn API, không ghi audit "thành công" và không đổi state. Với **edit version**, Duyệt/Từ chối chỉ áp dụng từ `pending`; version `superseded` không nhận bất kỳ action moderation nào.
- **Khóa bình luận không chặn Rating**: sanction Khóa bình luận chỉ chặn Comment/Reply/Edit mới. Rating của user đang Khóa bình luận vẫn được gửi/đổi và vẫn tính vào aggregate công khai theo US03.

> *Xem thêm: [REQUIREMENTS_A11Y_SECURITY.md](../REQUIREMENTS_A11Y_SECURITY.md) mục IDOR/Session — API phải kiểm tra quyền xem theo từng gate hiển thị (Chờ duyệt/Ẩn/Xóa mềm/Account Lock/scope Đóng) ở tầng server, không chỉ ẩn ở UI; mục WCAG 2.1 AA — thông báo fallback ("Bình luận không còn khả dụng", "Bình luận hiện không khả dụng", "Khu vực bình luận hiện không khả dụng") phải được screen reader đọc rõ khi thay thế nội dung comment/thread.*

### Điểm cần PO chốt

- Không còn blocker PO về state transition/schedule trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US12-001 | Visibility | C1 Chờ duyệt | U1/U2 mở UI/API | U1 thấy status; U2 không thấy UI/API/count/notification. |
| TC-US12-002 | Versioning | C1 public, V2 pending | U2 mở trước/sau duyệt V2 | Trước duyệt thấy C1; sau duyệt thấy V2. |
| TC-US12-003 | Mode2→1 safe | Pending có AI Nhẹ/an toàn | Chuyển Mode2 → Mode1 | Item tự Hiển thị từ effective time, không cần Admin duyệt lại. |
| TC-US12-004 | Mode2→1 medium | Pending có AI Trung bình | Chuyển Mode2 → Mode1 | Item tiếp tục Chờ duyệt. |
| TC-US12-005 | Mode1→2 | Có C1 đã public | Chuyển Mode1 → Mode2 | C1 vẫn public; nội dung mới sau effective time vào queue. |
| TC-US12-006 | Close UI/API | Phim/tập Đóng | Mở UI và gọi Comment/Reply/Like/Mention/Report/Rating/Share APIs | Toàn bộ comment area/rating ẩn; mọi interaction mới trong scope bị chặn. |
| TC-US12-007 | Close with queue | Có pending item | Đóng rồi Admin duyệt item | Queue vẫn xử lý; item đã duyệt không public trong lúc Đóng. |
| TC-US12-008 | Reopen | Đang Đóng có data cũ/item đã duyệt | Mở lại | Data đủ điều kiện public lại; state xử lý riêng vẫn được tôn trọng. |
| TC-US12-009 | Closed deep link | Scope Đóng; TH1: C1 hợp lệ, chỉ bị gate scope Đóng. TH2: C1 vừa trong scope Đóng vừa bị Ẩn/Từ chối riêng. TH3: C1 trong scope Đóng và tác giả đang Account Lock | Mở deep link Share/Notification tới C1 ở từng TH | Cả 3 TH đều mở đúng phim/tập, không hiển thị C1 và không đổi state C1. Thông báo theo Effective Visibility Resolver: TH1 “Khu vực bình luận hiện không khả dụng”; TH2 “Bình luận không còn khả dụng” (gate 2 thắng gate 5); TH3 “Bình luận hiện không khả dụng” (gate 4 thắng gate 5). |
| TC-US12-010 | Reopen deep link | Scope mở lại, C1 vẫn hợp lệ | Mở lại deep link cũ | Link lại mở đúng C1/thread, không cần tạo link mới. |
| TC-US12-011 | X-hours | Cấu hình lần lượt 3 loại mốc | Kiểm tra trước/đúng/sau mốc | Transition đúng loại mốc Admin chọn và timezone. |
| TC-US12-012 | Reason notification | Admin Từ chối/Ẩn/Xóa C1 | U1 mở app/notification | U1 thấy lý do cụ thể và nhận notification bắt buộc. |
| TC-US12-013 | Active day closed | U1 đăng nhập; scope đang Đóng | U1 mở trang phim/tập nhiều lần | Không phát sinh active day vì comment area không khả dụng. |
| TC-US12-014 | Audit | Đổi mode/schedule/state | Tra audit | Có actor/time/before-after/effective config. |
| TC-US12-015 | State × Action matrix | 5 state (Chờ duyệt/Hiển thị/Từ chối/Ẩn/Xóa mềm) × 8 action (Duyệt/Từ chối/Ẩn/Xóa mềm/Undo/Bỏ qua Report/Flag/Spoiler) | Với mỗi state, thử lần lượt cả 8 action qua UI và API | Chỉ tổ hợp hợp lệ được thực hiện: Duyệt chỉ từ Chờ duyệt; Từ chối chỉ từ Chờ duyệt; Ẩn chỉ từ Hiển thị. Mọi tổ hợp không hợp lệ khác bị chặn ở cả UI và API, không ghi audit "thành công", không đổi state. |
| TC-US12-016 | Concurrency tại effective time | Scope sắp chuyển Đóng, hoặc Mode1 sắp chuyển Mode2 tại effective time T | User gửi Comment/Reply/Rating/Share ngay trước T và ngay sau T | Request nhận trước T được xử lý theo mode/scope cũ; request nhận sau T được xử lý theo mode/scope mới; quyết định bám theo **thời điểm hệ thống nhận request**, không phụ thuộc thời điểm AI trả kết quả (kể cả khi AI trả kết quả chậm, sau T). |
