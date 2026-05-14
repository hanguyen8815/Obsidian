# Obsidian Vault

Vault này dùng để quản lý ghi chú công việc, tài liệu làm việc, kiến thức đã chắt lọc và tài liệu tham chiếu trong Obsidian.

## Mục tiêu

- Ghi nhanh ý mới, đầu việc và nội dung phát sinh hằng ngày.
- Quản lý note phục vụ công việc triển khai, nghiệp vụ, báo cáo và vận hành.
- Chắt lọc kiến thức để tái sử dụng và truy vết quyết định.
- Lưu tài liệu tham chiếu theo cấu trúc ổn định, dễ tìm.

## Cấu trúc chính

- `00-General`: Quy định chung, workflow và quy ước dùng vault.
- `01-Inbox`: Nơi tiếp nhận note mới khi chưa phân loại.
- `10-Daily`: Ghi chép theo ngày.
- `20-Work`: Note phục vụ công việc đang làm.
- `30-Knowlege`: Kiến thức đã làm rõ và có thể tái sử dụng.
- `31-Personal`: Nội dung cá nhân.
- `90-Archive`: Note đã xong hoặc không còn active.
- `95-Reference`: Tài liệu nguồn, ảnh, file tham chiếu.
- `99-Templates`: Mẫu note dùng lại.

## Workflow ngắn gọn

1. Ghi nhanh vào `01-Inbox` hoặc `10-Daily`.
2. Rà lại và phân loại sang đúng nhóm thư mục.
3. Chuyển nội dung đang làm vào `20-Work`.
4. Chắt lọc nội dung ổn định sang `30-Knowlege`.
5. Lưu trữ note không còn active vào `90-Archive`.

## Quy ước quan trọng

- Tên file và tên thư mục viết không dấu để dễ tìm và hạn chế lỗi đồng bộ.
- Nội dung bên trong note viết bằng Tiếng Việt có dấu, mã hóa UTF-8.
- Không đặt tên note chung chung như `Note`, `Test`, `Untitled`.
- Ưu tiên dùng liên kết nội bộ `[[...]]` để nối các note liên quan.
- Trước khi tạo note mới, đọc `00-General/Vault Workflow.md`.
- Khi cần tạo note theo chuẩn, xem thêm `00-General/Obsidian Created Rule.md`.

## Mở vault

1. Clone repo về máy.
2. Mở Obsidian.
3. Chọn `Open folder as vault` và trỏ tới thư mục repo này.

## Lưu ý khi dùng Git

- Kiểm tra kỹ nội dung nhạy cảm trước khi commit.
- Không đưa dữ liệu thật của Người bệnh, mật khẩu, token hoặc file cấu hình nhạy cảm vào repo.
- Rà lại file rác hệ điều hành như `desktop.ini`, `Thumbs.db`, `.DS_Store` trước khi push.

## Tài liệu nên đọc đầu tiên

- `00-General/Vault Workflow.md`
- `00-General/Obsidian Created Rule.md`

