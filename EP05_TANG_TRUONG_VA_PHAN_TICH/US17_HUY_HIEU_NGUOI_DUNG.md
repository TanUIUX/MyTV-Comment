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
2. Nếu user không còn đạt điều kiện Fan tích cực/Fan trung thành, áp dụng **grace period 7 ngày**; hết 7 ngày vẫn không đạt mới tự thu hồi.
3. Mỗi user chỉ hiển thị tối đa **1 badge** cạnh tên.
4. Với các badge đã được xếp cấp, thứ tự ưu tiên đã chốt: **Admin cấp/Chuyên gia > Fan trung thành > Fan tích cực**.
5. Khi user được cấp hoặc bị thu hồi badge, gửi **push + in-app notification** nếu notification cộng đồng đang bật theo US09.
6. Khi Admin tắt một loại badge, badge đó **ẩn khỏi UI ngay** nhưng lịch sử sở hữu/cấp/thu hồi vẫn được giữ cho audit; bật lại không tạo duplicate.
7. Comment/reply Ẩn/Xóa/Từ chối/spam không được dùng để tích lũy điều kiện badge tự động.
8. Report chưa xác minh không tự làm mất badge.
9. Mọi thay đổi cấu hình/cấp thủ công/thu hồi có audit.

### Quy tắc nghiệp vụ

- “Ngày hoạt động” sử dụng định nghĩa activity hợp lệ trong hệ thống tracking; phải nhất quán trong data dictionary US19.
- Like của chính tác giả là Like hợp lệ theo US07 và tham gia các count nếu chưa có rule loại trừ riêng.
- Grace period 7 ngày áp dụng badge hành vi Fan tích cực/Fan trung thành; không áp dụng cho source comment trở thành invalid của badge Bình luận nổi bật.

### Điểm cần PO chốt

- Xác định vị trí/cấp ưu tiên của badge **Bình luận nổi bật** so với Admin/Chuyên gia, Fan trung thành và Fan tích cực khi user sở hữu đồng thời nhiều badge nhưng UI chỉ hiển thị 1.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US17-001 | Fan active | 30d: U1 active 7 ngày, 10 comment/reply hợp lệ | Chạy daily job | Cấp Fan tích cực. |
| TC-US17-002 | Fan active boundary | 6 ngày hoặc 9 comment/reply | Chạy job | Không cấp. |
| TC-US17-003 | Loyal | 90d: 30 active days, 30 comment/reply, 30 Like nhận | Chạy job | Cấp Fan trung thành. |
| TC-US17-004 | Loyal boundary | Thiếu một trong ba ngưỡng | Chạy job | Không cấp. |
| TC-US17-005 | Daily/grace | User vừa không còn đạt badge Fan | Chạy daily job 7 ngày | Badge giữ trong grace; sau ngày thứ 7 vẫn không đạt thì thu hồi. |
| TC-US17-006 | Featured eligibility | Comment public có ≥20 Like, ≥5 Reply, Top10% FeaturedScore 30d | Chạy job | User đủ điều kiện badge Bình luận nổi bật toàn MyTV. |
| TC-US17-007 | Featured stability | Badge đã cấp, comment tụt Top10% nhưng vẫn public/hợp lệ | Chạy job | Không thu hồi chỉ do ranking tụt. |
| TC-US17-008 | Featured invalid | Comment nguồn bị Ẩn/Xóa/Từ chối | Chạy job | Badge bị thu hồi. |
| TC-US17-009 | Max display | User có nhiều badge | Mở comment | Chỉ hiển thị 1 badge theo cấp ưu tiên được xác định. |
| TC-US17-010 | Admin badge expiry | Cấp badge với/không expiry | Kiểm tra trước/sau expiry | Có expiry thì hết hiệu lực đúng mốc; không expiry thì giữ tới thu hồi. |
| TC-US17-011 | Toggle type | Tắt loại badge đang có owner | Refresh UI | Badge ẩn ngay; history còn; bật lại không duplicate. |
| TC-US17-012 | Notification | Grant/revoke badge | Kiểm tra push/in-app | Gửi notification cộng đồng theo setting US09. |
