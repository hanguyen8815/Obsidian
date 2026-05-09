---
name: generate-hdsd-docx
description: Tạo hoặc cập nhật file Word HDSD từ note Markdown hoặc nội dung hướng dẫn sử dụng. Use when Codex needs to: (1) xuất .docx HDSD chuyên nghiệp từ note, (2) áp chuẩn trình bày HDSD đã chốt như Arial, lề 2cm, header/footer, số trang, khối bước thao tác, (3) chỉnh lại tài liệu HDSD hiện có theo cùng một form thống nhất, hoặc (4) biến note kỹ thuật/vận hành thành tài liệu hướng dẫn sử dụng sẵn sàng gửi người dùng.
---

# Generate HDSD DOCX

Dùng skill này khi người dùng muốn tạo nhanh file Word HDSD mà không muốn mô tả lại từng rule trình bày.

## Workflow

1. Xác định note nguồn `.md` hoặc nội dung cần đưa vào HDSD.
2. Xác định tiêu đề tài liệu:
   - Ưu tiên dòng `# ...` đầu tiên trong note.
   - Nếu không có thì suy ra từ tên file.
3. Chạy `scripts/generate_hdsd_docx.py` để xuất `.docx`.
4. Nếu người dùng yêu cầu chỉnh riêng header, footer, lề, font hoặc khối bước thao tác, truyền option tương ứng cho script.
5. Sau khi xuất xong, kiểm tra lại file đầu ra tồn tại và xác nhận các rule chính đã được áp dụng.

## Mặc định trình bày

- Body font: Arial 11.
- Lề trên, dưới, trái, phải: 2 cm.
- Header: căn phải, cỡ 10, không đậm, nội dung `HDSD {title}`.
- Footer: căn phải, cỡ 10, nội dung `Trang {PAGE}/{NUMPAGES}`.
- Tiêu đề tài liệu: lớn, căn giữa.
- Heading cấp 2 trong note (`##`) được đánh số `1.`, `2.`, `3.` trong tài liệu.
- Các dòng bullet bắt đầu bằng `- Bước 1:`, `- Bước 2:`... hoặc các bullet trong section thao tác sẽ được dựng thành khối bước riêng kiểu HDSD.
- Code block và đường dẫn được đưa vào khối nền nhạt để dễ quét.
- Các mục như `Lưu ý`, `Rủi ro`, `Giới hạn`, `Ghi chú hỗ trợ`, `Trước khi bắt đầu` nên được trình bày thành khối note riêng.

## Chạy script

```bash
python3 scripts/generate_hdsd_docx.py <input.md>
python3 scripts/generate_hdsd_docx.py <input.md> --output <output.docx>
python3 scripts/generate_hdsd_docx.py <input.md> --title "Tên tài liệu"
```

Các option hay dùng:

- `--title`: ép tiêu đề tài liệu nếu không muốn lấy từ note.
- `--header-prefix`: đổi tiền tố header. Mặc định là `HDSD`.
- `--body-font-size`: đổi cỡ chữ nội dung. Mặc định là `11`.
- `--margin-cm`: đổi lề 4 phía. Mặc định là `2`.

## Cách đọc note nguồn

- Đọc `references/hdsd-format.md` trước nếu cần nhớ lại rule trình bày.
- Ưu tiên note có cấu trúc rõ theo heading như:
  - `## Tóm tắt ngắn`
  - `## Trước khi bắt đầu`
  - `## Cách áp dụng`
  - `## Ví dụ hoặc tình huống dùng`
  - `## Rủi ro hoặc giới hạn`
- Nếu note đang là dạng Knowledge Note hoặc Work Note, vẫn có thể dùng; script sẽ cố gắng map section sang form HDSD dễ đọc.

## Điều chỉnh sau khi xuất

- Nếu file đích đang mở trong Word và không ghi đè được, xuất sang tên mới rồi báo người dùng.
- Nếu người dùng chỉ yêu cầu sửa trình bày chứ không đổi nội dung, chạy lại script trên cùng file markdown nguồn là đủ.
- Nếu note có thông tin nhạy cảm, rà lại nội dung trước khi xuất file Word.
