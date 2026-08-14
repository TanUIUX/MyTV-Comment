# EP03 — AN TOÀN VÀ KIỂM DUYỆT

## 1. Thông tin Epic

| Thuộc tính | Nội dung |
|---|---|
| Epic ID | EP03 |
| Tên Epic | An toàn và kiểm duyệt |
| Mục tiêu | Hạn chế nội dung vi phạm bằng Report, AI và quy trình kiểm duyệt linh hoạt mà vẫn duy trì trải nghiệm thảo luận phù hợp |
| Đối tượng | Người dùng đã đăng nhập; Admin; hệ thống AI moderation |
| Giá trị kinh doanh | Giảm rủi ro pháp lý/thương hiệu, bảo vệ người dùng và tối ưu khối lượng vận hành |
| Phạm vi | Report, AI tiền kiểm, hai chế độ duyệt, trạng thái bình luận và cấu hình đóng/mở theo phim |

## 2. Kết quả mong đợi

- Người dùng có kênh báo cáo nội dung không phù hợp.
- Tất cả bình luận, reply và phiên bản sửa đi qua kiểm duyệt.
- Phim thông thường có thể thảo luận gần thời gian thực.
- Nội dung nhạy cảm có thể áp dụng cơ chế Admin duyệt trước.
- Admin kiểm soát được việc mở/đóng và cơ chế kiểm duyệt theo từng phim.
- Mọi chuyển đổi trạng thái quan trọng được lưu dấu vết.

## 3. Chỉ số gợi ý

- Tỷ lệ nội dung an toàn được tự động hiển thị.
- Tỷ lệ nội dung AI chuyển hàng chờ và tỷ lệ Admin xác nhận vi phạm.
- Tỷ lệ lọt vi phạm sau khi đã hiển thị.
- Thời gian trung bình xử lý hàng chờ và Report.
- Số Report hợp lệ/không hợp lệ.
- Tỷ lệ lỗi hoặc timeout của AI moderation.

---


## Danh sách User Story

| ID | User Story | File |
|---|---|---|
| US10 | Report bình luận vi phạm | [US10](US10_REPORT_BINH_LUAN_VI_PHAM.md) |
| US11 | AI kiểm duyệt theo hai chế độ | [US11](US11_AI_KIEM_DUYET_THEO_HAI_CHE_DO.md) |
| US12 | Quản lý trạng thái và phạm vi hiển thị bình luận | [US12](US12_QUAN_LY_TRANG_THAI_VA_PHAM_VI_HIEN_THI_BINH_LUAN.md) |

[← Quay lại backlog tổng](../README.md)

## 4. Điều kiện hoàn thành Epic

- Report được chuyển đúng vào CMS và không tự động làm ẩn bình luận ngoài chính sách.
- Hai chế độ AI hoạt động đúng và có fallback an toàn.
- Không có nội dung Chờ duyệt bị lộ cho người dùng khác.
- Phiên bản cũ được giữ công khai đúng trong quá trình kiểm duyệt bản sửa.
- Đóng bình luận ẩn đúng toàn bộ khu vực và không xóa dữ liệu.
- Mọi quyết định/trạng thái quan trọng có audit và có thể tra cứu trên CMS.
