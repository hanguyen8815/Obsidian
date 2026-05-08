## Quy tắc tạo Obsidian note mặc định
Khi người dùng yêu cầu "note lại", "tạo note", "lưu note", AI được phép tự suy ra template, folder và tên note theo ngữ cảnh rồi đề xuất tên note, loại template và folder phù hợp theo nội dung; trừ khi nội dung quá mơ hồ hoặc có nhiều cách phân loại ngang nhau.


### Mapping mặc định

- Nếu là báo cáo tiến độ tháng:
  - Template: `99-Templates/Monthly Report.md`
  - Folder: `20-Work/20.3-Bao cao tien do`
  - Tên note: `YYYY-MM. Bao cao tien do Thang M.YYYY`

- Nếu là biên bản họp với khách hàng
  - Template: `99-Templates/Meeting Minutes.md`
  - Folder: `20-Work/[Folder Du an/Don vi tuong ung]/1. Bien ban lam viec`
  - Tên note: `YYYY-MM-DD - [Du an/Don vi] - Bien ban lam viec`

- Nếu là note xử lý sự cố:
  - Template: `99-Templates/Work Note.md`
  - Folder: `20-Work/[Folder Du an/Don vi tuong ung]/2. Xu ly su co`
  - Tên note: `YYYY-MM-DD - [Du an/Don vi] - [Noi dung su co]`

- Nếu là note Thông tin đối tác:
  - Template: `99-Templates/Project Overview.md`
  - Folder: `20-Work/20.2-Doi tac`
  - Tên note: `[Ten doi tac] - Thong tin tong hop`

- Nếu là note Kế hoạch công việc:
  - Template: `99-Templates/Work Note.md`
  - Folder: `20-Work/20.4-Ke hoach cong viec`
  - Tên note: `YYYY-MM-DD - [Chu de] - Ke hoach cong viec`

- Nếu là decision:
  - Template: `99-Templates/Decision Log.md`
  - Folder: 30-Knowlege/30.4-Decision
  - Tên note: `YYYY-MM-DD - Decision log - [Chu de]`


- Nếu là note Nghiệp vụ:
  - Template: `99-Templates/BA/BA-02-Use-Case.md`
  - Folder: `20-Work/20.5-Nghiep vu`
  - Tên note: `YYYY-MM-DD_NV_[Chu de]`

- Nếu là note Quy trình:
  - Template: `99-Templates/BA/BA-02-Use-Case.md`
  - Folder: `20-Work/20.6-Quy trinh`
  - Tên note: `YYYY-MM-DD_QT-[Chu de]`

- Nếu là note Danh muc:
  - Template: `99-Templates/Work Note.md`
  - Folder: `20-Work/20.7-Danh muc`
  - Tên note: `[Ten danh muc]`

- Nếu là Hướng dẫn sử dụng (HDSD):
  - Template: `99-Templates/Work Note.md`
  - Folder: `20-Work/20.8-HDSD`
  - Tên note: `YYYY-MM-DD_HDSD_[Phan he/Tinh nang]`

- Nếu là testcase:
  - Template: `99-Templates/Test/Test-01-Test-Scenario.md`
  - Folder: `20-Work/20.9-Testcase, UAT`
  - Tên note: `YYYY-MM-DD-TC_[Chu de]`

- Nếu là UAT:
  - Template: `99-Templates/Test/Test-02-UAT-Note.md`
  - Folder: `20-Work/20.9-Testcase, UAT`
  - Tên note: `[Chu de] - UAT`

- Nếu là note ho tro, hotfix:
  - Template: `99-Templates/Trien-khai/TK-02-Ghi-nhan-van-de.md`
  - Folder: `20-Work/20.10-Ho tro, Hotfix`
  - Tên note: `YYYY-MM-DD - [Don vi/Khach hang] - [Noi dung ho tro hotfix]`

- Nếu là Biên bản bàn giao, nghiệm thu:
  - Template: `99-Templates/Trien-khai/TK-01-Bien-ban-lam-viec.md`
  - Folder: `20-Work/20.11-Bien ban ban giao, nghiem thu`
  - Tên note: `YYYY-MM-DD - [Don vi/He thong] - [Ban giao/Nghiem thu]`
  - Nếu là nghiem thu ket noi thiet bi thi uu tien template: `99-Templates/Y-te-Ket-noi-thiet-bi/Trien-khai/TK-05-Bien-ban-nghiem-thu-ket-noi.md`

- Nếu là cong van:
  - Template: `99-Templates/Work Note.md`
  - Folder: `20-Work/20.12-Cong van`
  - Tên note: `[So cong van] - [Noi dung chinh]`

- Nếu là ghi nhanh công việc chưa phân loại:
  - Template: `99-Templates/Inbox Note.md`
  - Folder: `01-Inbox`

- Nếu là note công việc đang triển khai:
  - Template: `99-Templates/Work Note.md`
  - Folder: `20-Work`

- Nếu là note kiến thức đã chắt lọc:
  - Template: `99-Templates/Knowledge Note.md`
  - Folder: `30-Knowlege`
  - Tên note: YYYY-MM-DD_Knowlege_[Chủ đề]

### Nguyên tắc đặt tên file và folder

- Tên file và folder phải bám theo nội dung đặc thù của note, dự án, đơn vị hoặc chủ đề đang làm.
- Ưu tiên dùng từ Tiếng Việt, nhưng khi đặt tên file và folder phải viết dưới dạng không dấu để người đọc vẫn hiểu ngay nội dung.
- Tên file và folder phải viết không dấu để đồng nhất cách lưu, dễ tìm kiếm và hạn chế lỗi đồng bộ.
- Không đặt tên chung chung như `Hop`, `Tai lieu`, `Note moi`, `Test`.
- Nếu đã có quy ước ngày hoặc STT thì giữ ngày hoặc STT ở đầu tên để dễ sắp xếp.

### Nguyên tắc ưu tiên

- Nếu người dùng nói rõ template hoặc folder thì ưu tiên theo người dùng.
- Nếu đã có note cùng chủ đề/tháng thì ưu tiên cập nhật note hiện có thay vì tạo note mới.
- Nếu chưa tìm được folder dự án hoặc đơn vị phù hợp thì AI đề xuất tạo folder mới theo cấu trúc dùng chung trong `20-Work/[Folder Du an/Don vi tuong ung]`, tối thiểu gồm: `1. Bien ban lam viec`, `2. Xu ly su co`.
- Nếu chỉ có một mapping phù hợp rõ ràng thì AI không cần hỏi lại template và folder.
- AI chỉ cần xác nhận lại khi:
  - có từ 2 hướng phân loại hợp lý trở lên
  - note có thể thuộc nhiều dự án khác nhau
  - nội dung chứa thông tin nhạy cảm cần hỏi trước khi lưu

### Quyền tạo note

- Khi mapping đã rõ theo rule trên, AI được phép đề xuất ngắn gọn 1 dòng rồi tạo/cập nhật note luôn nếu người dùng dùng các cụm như:
  - "note lại"
  - "lưu thành note"
  - "cập nhật note"
- Không cần hỏi lại template và folder trong các trường hợp này.
