# HDSD Format

## Mục tiêu

Chuẩn hóa cách Codex tạo file HDSD Word từ note Markdown để không phải chốt lại từng rule trình bày ở mỗi lần làm.

## Rule chính

- Font mặc định: Arial.
- Body size: 11.
- Header size: 10, không đậm, căn phải.
- Header text: `HDSD {title}`.
- Footer size: 10, căn phải.
- Footer text: `Trang {PAGE}/{NUMPAGES}`.
- Lề trên, dưới, trái, phải: 2 cm.
- Tiêu đề lớn, căn giữa.
- Section `##` đánh số rõ ràng.
- Phần bước thao tác hiển thị nhãn `Bước 1`, `Bước 2`... trong ô màu riêng, độ rộng ô nhãn vừa với nội dung.
- Khối mã, đường dẫn, command hiển thị nền nhạt.
- Mục lưu ý, rủi ro, ghi chú hỗ trợ hiển thị khối note riêng.

## Mapping nội dung gợi ý

- `## Tóm tắt ngắn`: bullet highlight đầu tài liệu.
- `## Trước khi bắt đầu`: khối điều kiện cần có.
- `## Cách áp dụng`: phần chính của hướng dẫn.
- `## Ví dụ hoặc tình huống dùng`: ví dụ mở rộng.
- `## Phím tắt dùng nhanh`: bullet shortcut.
- `## Rủi ro hoặc giới hạn`: khối lưu ý màu vàng.
- `## Ghi chú hỗ trợ`: khối theo dõi xử lý hoặc handoff.

## Xử lý ngoại lệ

- Nếu không có heading `#`, lấy title từ tên file và thay `_` bằng khoảng trắng.
- Nếu output path bị khóa do file đang mở, xuất file tên mới rồi báo rõ lý do.
- Nếu section `Cách áp dụng` không có bước rõ ràng, vẫn giữ bullet thường thay vì cố ép thành step box.
