# US07 — Like và Unlike bình luận

> Thuộc EP02 — Tương tác cộng đồng
> [← Quay lại README của Epic](README.md) · [← Backlog MyTV](../README.md)

### User Story

**Là người dùng đã đăng nhập**, tôi muốn Like hoặc Unlike một bình luận/reply, để thể hiện sự đồng tình hoặc yêu thích đối với ý kiến đó.

### Ưu tiên

**Must**

### Acceptance Criteria

1. User đăng nhập có thể Like/Unlike comment hoặc reply đang Hiển thị, kể cả **comment/reply của chính mình**.
2. Mỗi tài khoản chỉ có một trạng thái Like hiện hành trên mỗi comment/reply.
3. UI áp dụng **optimistic update ngay lập tức** sau thao tác Like/Unlike.
4. Client gom các thay đổi Like/Unlike và đồng bộ xuống BE theo batch sau tối đa **5 giây** thay vì gửi từng thao tác riêng lẻ ngay lập tức.
5. Nếu user rời màn hình hoặc app chuyển background trước 5 giây, client **flush batch sớm**.
6. BE là nguồn trạng thái cuối cùng; nếu kết quả server khác optimistic state, client reconcile về trạng thái BE.
7. Double-click/retry không làm tăng/giảm trùng Like.
8. Guest chọn Like được chuyển sang login; không tạo Like trước xác thực.
9. Comment bị Ẩn/Xóa hoặc user không còn quyền xem không thể nhận Like mới.
10. Net Like hiện tại được dùng cho sort Được yêu thích/Featured Score; lịch sử thao tác Like/Unlike có thể được tracking riêng cho US19.

### Quy tắc nghiệp vụ

- Self-like là hợp lệ và được tính như Like khác.
- Một account, một Like hiện hành trên một comment/reply.
- Optimistic UI không thay đổi nguyên tắc BE là source of truth.
- Batch window 5 giây là rule client; phải hỗ trợ idempotency/dedup ở BE.

### Điểm cần PO chốt

- Không còn blocker PO cho Like/Unlike trong scope hiện tại.

---

## Test Cases

| ID | Loại | Tiền điều kiện / dữ liệu | Bước kiểm thử | Kết quả mong đợi |
|---|---|---|---|---|
| TC-US07-001 | Functional | U1 đăng nhập; C1/R1 public | Like C1/R1 | UI đổi trạng thái ngay, mỗi target có đúng một Like của U1. |
| TC-US07-002 | Self-like | U1 là tác giả C1 | U1 Like C1 | Được phép; Net Like tăng như bình thường. |
| TC-US07-003 | Unlike | U1 đã Like C1 | Unlike | UI giảm ngay; state cuối BE không còn Like U1. |
| TC-US07-004 | Batch timing | Thực hiện nhiều Like/Unlike | Theo dõi request trong 5 giây | UI phản hồi ngay nhưng client gom request và sync batch tối đa sau 5 giây. |
| TC-US07-005 | Early flush | Có batch chưa gửi | Rời màn hình/chuyển background trước 5 giây | Batch được flush sớm, không mất state. |
| TC-US07-006 | Reconcile | Mock BE từ chối/mismatch | Like optimistic rồi nhận response | UI reconcile theo BE và thông báo lỗi phù hợp. |
| TC-US07-007 | Idempotency | Double-click/retry | Gửi thao tác lặp | Không nhân đôi Like/event hợp lệ. |
| TC-US07-008 | Authentication | Guest | Chọn Like | Yêu cầu login, không tạo Like trước auth. |
| TC-US07-009 | Invalid target | C1 Ẩn/Xóa | Like qua UI/API | Bị chặn; Net Like public không đổi. |
| TC-US07-010 | Integration | Net Like thay đổi | Mở sort Được yêu thích/Nổi bật | Ranking dùng Net Like hiện hành sau reconcile. |
