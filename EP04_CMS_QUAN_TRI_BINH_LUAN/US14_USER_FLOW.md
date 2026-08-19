# User Flow — US14 Xử lý nội dung trên CMS

> User Story: [US14_XU_LY_NOI_DUNG_TREN_CMS.md](US14_XU_LY_NOI_DUNG_TREN_CMS.md)

**Platform:** Web/Desktop only

```mermaid
flowchart TD
 A[Chọn item trong Detail panel] --> B[Server check current state + scope]
 B --> C{Action}
 C -->|Duyệt| D[Không reason]
 C -->|Từ chối/Ẩn/Xóa mềm| E[Reason; Vi phạm khác bắt note 1–500]
 D --> F[Apply + audit]
 E --> F
 F --> G[Success → tự chuyển item kế tiếp trong queue, giữ filter/search]
 H[Item bị Admin khác cập nhật] --> I[Không chạy action cũ; báo conflict + refresh state]
 J[Bulk] --> K[Checkbox từng row/header Select all, max 100]
 K --> L[Bulk Duyệt/Từ chối/Ẩn/Xóa mềm]
 L --> M[Action cần reason dùng 1 reason chung; Other dùng 1 note chung]
 M --> N[Validate từng item + partial success summary]
```

## UX đã chốt

- Report không tab riêng; Detail panel hiển thị report context.
- Bulk action bar chỉ hiện khi có selection.
- Sau single action thành công tự chuyển item kế tiếp; hết queue thì empty queue, không tự đổi filter.
- Optimistic concurrency: nếu stale, không overwrite im lặng.
