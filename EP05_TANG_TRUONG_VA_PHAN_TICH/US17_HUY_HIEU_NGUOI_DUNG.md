# US17 — Huy hiệu người dùng

> Thuộc EP05 — Tăng trưởng và phân tích
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là người dùng tích cực**, tôi muốn được ghi nhận bằng huy hiệu phù hợp, để có động lực tiếp tục đóng góp nội dung chất lượng.

### Ưu tiên

**Could**

### Các loại và tiêu chí đã chốt

#### Fan tích cực
- Xét rolling **30 ngày gần nhất**.
- Hoạt động ít nhất **7 ngày**.
- Có ít nhất **10 comment/reply hợp lệ**.
- Không yêu cầu Like nhận được.

#### Fan trung thành
- Xét rolling **90 ngày gần nhất**.
- Hoạt động ít nhất **30 ngày**.
- Có ít nhất **30 comment/reply hợp lệ**.
- Nhận ít nhất **30 Like** trong 90 ngày.

#### Bình luận nổi bật
- Là huy hiệu **toàn MyTV**, không gắn riêng theo phim/tập trên UI badge.
- User đủ điều kiện nếu có ít nhất một comment đang Hiển thị đạt **≥20 Like + ≥5 Reply** và thuộc **Top 10% Featured Score cao nhất trên toàn MyTV trong 30 ngày gần nhất** tại thời điểm xét.
- Sau khi được cấp, việc comment tụt khỏi Top 10% do ranking biến động **không tự thu hồi badge**.
- Badge bị thu hồi nếu comment nguồn bị Ẩn/Xóa/Từ chối hoặc không còn hợp lệ.

#### Admin cấp / Chuyên gia
- Admin cấp thủ công cho user phù hợp.
- Có thể đặt ngày hết hạn hoặc không; nếu không đặt thì giữ tới khi Admin thu hồi/tắt loại badge.

### Acceptance Criteria

1. Hệ thống đánh giá badge tự động **mỗi ngày 1 lần**.
2. Một **ngày hoạt động** được tính khi user **đã đăng nhập mở/đọc khu vực Bình luận ít nhất 1 lần trong ngày**; mở nhiều lần cùng ngày vẫn chỉ tính 1 active day. Guest không tích lũy active day vì không có account để gắn thành tích.
3. Nếu user không còn đạt điều kiện Fan tích cực/Fan trung thành, áp dụng **grace period 7 ngày**; hết 7 ngày vẫn không đạt mới tự thu hồi.
4. Mỗi user chỉ hiển thị tối đa **1 badge** cạnh tên.
5. Thứ tự ưu tiên hiển thị đã chốt: **Admin cấp/Chuyên gia > Bình luận nổi bật > Fan trung thành > Fan tích cực**.
6. Khi user được cấp hoặc bị thu hồi badge, gửi **push + in-app notification** nếu notification cộng đồng đang bật theo US09.
7. Khi Admin tắt một loại badge, badge đó **ẩn khỏi UI ngay** nhưng lịch sử sở hữu/cấp/thu hồi vẫn được giữ cho audit; bật lại không tạo duplicate.
8. Comment/reply Ẩn/Xóa/Từ chối/spam không được dùng để tích lũy điều kiện badge tự động.
9. Report chưa xác minh không tự làm mất badge.
10. Mọi thay đổi cấu hình/cấp thủ công/thu hồi có audit.

### Quy tắc nghiệp vụ

- “Ngày hoạt động” là **read/open activity có account** của khu vực Bình luận, không yêu cầu Comment/Reply/Like/Rating/Report/Share.
- Một account tối đa **1 active day/ngày** dù mở khu vực Bình luận nhiều lần hoặc ở nhiều phim/tập.
- Guest chỉ đọc không tạo active day cho badge.
- Điều kiện active day và điều kiện số lượng comment/reply của Fan badge là **hai điều kiện độc lập**; việc chỉ đọc có thể tăng active day nhưng không tăng count comment/reply.
- Like của chính tác giả là Like hợp lệ theo US07 và tham gia các count nếu chưa có rule loại trừ riêng.
- Grace period 7 ngày áp dụng badge hành vi Fan tích cực/Fan trung thành; không áp dụng cho source comment trở thành invalid của badge Bình luận nổi bật.
- Khi user có nhiều badge hiệu lực, UI luôn chọn **badge có cấp ưu tiên cao nhất** theo thứ tự đã chốt.

### Điểm cần PO chốt

- Không còn blocker PO cho badge trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US17-001 | Active day | U1 đăng nhập | Mở/đọc khu vực Bình luận ít nhất 1 lần trong ngày | Ngày đó được tính đúng 1 active day cho U1. |
| TC-US17-002 | Active dedup | U1 đăng nhập | Mở khu vực Bình luận nhiều lần, nhiều phim/tập trong cùng ngày | Vẫn chỉ tính 1 active day trong ngày. |
| TC-US17-003 | Guest active | Phiên guest | Mở/đọc khu vực Bình luận nhiều lần | Không tạo active day badge vì không có account. |
| TC-US17-004 | Fan active | 30d: U1 có 7 active days theo rule đọc + 10 comment/reply hợp lệ | Chạy daily job | Cấp Fan tích cực. |
| TC-US17-005 | Fan active boundary | 6 active days hoặc 9 comment/reply | Chạy job | Không cấp. |
| TC-US17-006 | Loyal | 90d: 30 active days, 30 comment/reply, 30 Like nhận | Chạy job | Cấp Fan trung thành. |
| TC-US17-007 | Loyal boundary | Thiếu một trong ba ngưỡng | Chạy job | Không cấp. |
| TC-US17-008 | Daily/grace | User vừa không còn đạt badge Fan | Chạy daily job 7 ngày | Badge giữ trong grace; sau ngày thứ 7 vẫn không đạt thì thu hồi. |
| TC-US17-009 | Featured eligibility | Comment public có ≥20 Like, ≥5 Reply, Top10% FeaturedScore 30d | Chạy job | User đủ điều kiện badge Bình luận nổi bật toàn MyTV. |
| TC-US17-010 | Featured stability | Badge đã cấp, comment tụt Top10% nhưng vẫn public/hợp lệ | Chạy job | Không thu hồi chỉ do ranking tụt. |
| TC-US17-011 | Featured invalid | Comment nguồn bị Ẩn/Xóa/Từ chối | Chạy job | Badge bị thu hồi. |
| TC-US17-012 | Priority display | U1 có đồng thời Admin/Chuyên gia, Bình luận nổi bật, Fan trung thành, Fan tích cực | Mở comment | Chỉ hiển thị 1 badge theo thứ tự Admin/Chuyên gia > Bình luận nổi bật > Fan trung thành > Fan tích cực. |
| TC-US17-013 | Admin badge expiry | Cấp badge với/không expiry | Kiểm tra trước/sau expiry | Có expiry thì hết hiệu lực đúng mốc; không expiry thì giữ tới thu hồi. |
| TC-US17-014 | Toggle type | Tắt loại badge đang có owner | Refresh UI | Badge ẩn ngay; history còn; bật lại không duplicate. |
| TC-US17-015 | Notification | Grant/revoke badge | Kiểm tra push/in-app | Gửi notification cộng đồng theo setting US09. |
