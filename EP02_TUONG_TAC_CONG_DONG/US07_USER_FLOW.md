# User Flow — US07 Like / Unlike

> User Story: [US07_LIKE_VA_UNLIKE_BINH_LUAN.md](US07_LIKE_VA_UNLIKE_BINH_LUAN.md)

**Platforms:** Phone, Web, SmartTV

```mermaid
flowchart TD
 A[Tap Like/Unlike] --> B{Đã login + target public?}
 B -->|Guest| C[Auth gate → login → quay lại, không auto-like]
 B -->|Không public| D[Action unavailable]
 B -->|Hợp lệ| E[Optimistic state + count]
 E --> F[Sync server]
 F -->|Success| G[Giữ state server]
 F -->|Lỗi| H[Auto retry đúng 1 lần]
 H -->|Success| G
 H -->|Vẫn lỗi| I[Dừng retry, revert state/count + báo lỗi]
```

## UX đã chốt

- Hiển thị `👍 {count}` trực tiếp cạnh Like.
- `Dislike` trên SmartTV chỉ có nghĩa **Unlike/bỏ Like**, không có reaction âm riêng.
- Request lỗi retry tự động 1 lần; lần 2 lỗi thì không tính Like, revert về server state.
- SmartTV cho Like/Unlike bằng remote nhưng vẫn yêu cầu login.
