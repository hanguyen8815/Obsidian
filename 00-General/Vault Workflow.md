# Vault Workflow

## Mục tiêu

- Chuẩn hóa cách tạo, lưu và tìm note trong vault.
- Giảm tình trạng note bị trôi, trùng nội dung hoặc khó truy vết.
- Giúp vault dùng được cho ghi nhanh hằng ngày, làm việc, chắt lọc kiến thức và lưu trữ dài hạn.

## Phạm vi áp dụng

- Áp dụng cho toàn bộ note trong vault này.
- Mọi note mới cần đi qua `01-Inbox` hoặc `10-Daily` trước khi được phân loại nếu chưa xác định rõ nơi lưu.

## Ngôn ngữ và giao tiếp

- Toàn bộ dự án, tài liệu và giao tiếp sử dụng Tiếng Việt với mã hóa UTF-8.
- Luôn trả lời và soạn tài liệu bằng tiếng Việt có dấu, trừ khi người dùng yêu cầu không dấu hoặc cần giữ nguyên ASCII cho mã nguồn/kỹ thuật.
- Tuyệt đối không sử dụng các từ Hán Việt khi có từ Thuần Việt tương đương phù hợp; ví dụ không dùng Bệnh nhân mà dùng Người bệnh.
- Các đối tượng sử dụng được gọi định danh theo nhóm như Người bệnh, Bác sĩ, Điều dưỡng... phải viết hoa chữ cái đầu mỗi từ.
- Khi chưa chắc yêu cầu, phải hỏi lại để làm rõ, không tự suy diễn.

### Quy tắc bắt buộc khi tạo note

- Tên file và tên folder phải viết không dấu để đồng nhất cách lưu, dễ tìm và hạn chế lỗi đồng bộ.
- Nội dung bên trong note bắt buộc viết bằng Tiếng Việt có dấu, dùng mã hóa UTF-8.
- Không được giữ nguyên nội dung template không dấu khi tạo note mới.
- Nếu template đang viết không dấu, phải chuyển toàn bộ tiêu đề, heading, nhãn trường và nội dung sang Tiếng Việt có dấu trước khi lưu note.
- Trước khi hoàn tất note, phải tự rà soát để bảo đảm:
  - tên file không dấu
  - nội dung có dấu
  - không còn heading hoặc mục nào bị sót ở dạng không dấu

## Cấu trúc thư mục chuẩn

- `00-General`: Note những quy định chung, nguyên tắc dùng vault, workflow, quy ước đặt tên, quy tắc review.
- `01-Inbox`: Tiếp nhận note mới trước khi phân loại.
- `10-Daily`: Ghi chép theo ngày, là điểm vào chính mỗi ngày.
- `20-Work`: Note phục vụ công việc đang làm.
- `30-Knowlege`: Kiến thức đã chắt lọc, có thể tái sử dụng.
- `90-Archive`: Note cũ, dự án đã xong, không còn active.
- `95-Reference`: Tài liệu tham khảo, nguồn gốc, tài liệu ngoài, quy chuẩn, ảnh chụp, file đính kèm tham chiếu.
- `99-Templates`: Mẫu note để tạo note mới.

## Workflow mặc định

### 1. Tiếp nhận

- Ý mới, thông tin rời, ghi nhanh, đầu việc phát sinh: tạo trong `01-Inbox`.
- Nội dung phát sinh trong ngày làm việc: ghi vào note ngày trong `10-Daily`.
- Nếu chưa rõ nên lưu ở đâu, luôn ưu tiên `01-Inbox` hoặc `10-Daily`.

### 2. Làm rõ

- Cuối ngày hoặc đầu ngày hôm sau, rà lại note trong `01-Inbox` và `10-Daily`.
- Với mỗi note, xác định rõ nó thuộc một trong các nhóm sau:
- Việc đang làm, nội dung phục vụ delivery hoặc vận hành: chuyển sang `20-Work`.
- Kiến thức đã hiểu và có thể tái sử dụng: chuyển sang `30-Knowlege`.
- Tài liệu chỉ để tham khảo: chuyển sang `95-Reference`.
- Nội dung đã hết giá trị sử dụng trực tiếp: chuyển sang `90-Archive`.

### 3. Thực hiện

- Trong `20-Work`, ưu tiên tổ chức theo chủ đề công việc, dự án, khách hàng hoặc giai đoạn triển khai.
- Mỗi note công việc nên có mục tiêu rõ, trạng thái rõ và liên kết đến các note liên quan.
- Quyết định quan trọng cần được ghi thành note riêng hoặc có mục `Quyết định` để dễ truy vết.

### 4. Chắt lọc

- Khi một nội dung không còn là ghi chép tạm mà đã thành hiểu biết ổn định, tách hoặc chuyển sang `30-Knowlege`.
- Note trong `30-Knowlege` chỉ giữ phần đã được làm rõ, tránh lẫn việc tạm thời hoặc trao đổi rời.
- Mỗi note kiến thức nên trả lời được ít nhất một câu hỏi cụ thể mà sau này có thể tra lại nhanh.

### 5. Lưu trữ

- Khi dự án kết thúc hoặc note không còn active, chuyển sang `90-Archive`.
- Không xóa note nếu vẫn còn giá trị truy vết, đối chiếu hoặc học lại sau này.

## Quy tắc phân loại nhanh

- Nếu nội dung là ý mới chưa xử lý: `01-Inbox`.
- Nếu nội dung gắn với ngày làm việc cụ thể: `10-Daily`.
- Nếu nội dung phục vụ công việc hiện tại: `20-Work`.
- Nếu nội dung là bài học, nguyên tắc, cách làm đã chắt lọc: `30-Knowlege`.
- Nếu nội dung là tài liệu nguồn hoặc tài liệu ngoài: `95-Reference`.
- Nếu nội dung đã xong và không còn active: `90-Archive`.
- Nếu nội dung là mẫu chuẩn để tái dùng: `99-Templates`.

## Quy ước tạo note

- Mỗi note chỉ nên có một mục tiêu chính.
- Tiêu đề note phải rõ nghĩa, đọc riêng vẫn hiểu.
- Tên file và folder phải bám theo nội dung đặc thù của note, dự án, đơn vị hoặc chủ đề đang làm.
- Ưu tiên dùng từ Tiếng Việt, nhưng khi đặt tên file và folder phải viết dưới dạng không dấu để người đọc vẫn hiểu ngay nội dung.
- Tên file và folder phải viết không dấu để đồng nhất cách lưu, dễ tìm kiếm và hạn chế lỗi đồng bộ.
- Không đặt tên kiểu `Note 1`, `Test`, `Untitled` hoặc các tên chung chung như `Hop`, `Tai lieu`, `Note moi`.
- Nếu là note công việc, nên đặt tên theo mẫu: `[Chu de] - [Noi dung chinh]`.
- Nếu là note kiến thức, nên đặt tên theo câu hỏi hoặc chủ đề tra cứu.
- Với note theo dự án hoặc đơn vị trong `20-Work`, ưu tiên cấu trúc thư mục con theo STT và nội dung rõ nghĩa, ví dụ: `1. Bien ban lam viec`, `2. Xu ly su co`.
- Với biên bản họp tiến độ dự án nội bộ, ưu tiên lưu tại `20-Work/20.3.1. Bien ban hop noi bo` và đặt tên theo mẫu `YYYY-MM-DD - Bien ban hop tien do [Chu de chinh 1], [Chu de chinh 2]`.
- Chi tiết khi được yêu cầu tạo note được quy định ở `00-General/Obsidian Created Rule.md`.

## Quy ước liên kết

- Dùng liên kết nội bộ `[[...]]` để nối giữa daily note, work note, knowledge note và reference note.
- Khi một note được tạo từ thông tin trong daily note, cần gắn link hai chiều để dễ truy vết.
- Không chép lại cùng một nội dung ở nhiều nơi nếu có thể liên kết.

## Quy tắc review định kỳ

### Hằng ngày

- Kiểm tra `01-Inbox`.
- Chốt note ngày trong `10-Daily`.
- Chuyển các nội dung đã rõ sang đúng thư mục.

### Hằng tuần

- Rà lại `20-Work` để xem note nào còn active, note nào cần archive.
- Chọn các nội dung có giá trị lặp lại để chắt lọc sang `30-Knowlege`.
- Kiểm tra note nào thiếu liên kết, thiếu nguồn hoặc thiếu trạng thái.

### Hằng tháng

- Dọn các note không còn dùng trong `20-Work`.
- Chuẩn hóa lại các note kiến thức quan trọng.
- Cập nhật `99-Templates` nếu phát hiện mẫu note đang thiếu hoặc chưa tiện dùng.

## Nguyên tắc giữ vault gọn và bền

- Inbox chỉ là nơi trung chuyển, không phải nơi lưu lâu dài.
- Daily note là điểm vào, không phải nơi chứa toàn bộ kiến thức dài hạn.
- Work note phục vụ thực thi.
- Knowledge note phục vụ tái sử dụng.
- Archive phục vụ truy vết.
- Reference phục vụ đối chiếu nguồn.

## Checklist khi tạo note mới

- Note này dùng để ghi nhanh, làm việc, chắt lọc hay tham khảo?
- Note này có cần liên kết với note nào đang có sẵn không?
- Note này đã đặt tên đủ rõ chưa?
- Note này nên ở thư mục hiện tại hay cần chuyển sau khi làm rõ?
- Note này có thông tin nhạy cảm cần hạn chế chia sẻ không?

## Ghi chú

- Nếu sau này đổi cấu trúc thư mục, cần cập nhật lại file này trước để giữ thống nhất cách dùng.
- Tên thư mục `30-Knowlege` đang bám theo quy ước hiện tại của vault. Nếu muốn sửa thành `30-Knowledge`, cần đổi đồng bộ ở toàn vault để tránh gãy liên kết.
