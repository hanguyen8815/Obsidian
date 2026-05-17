---
loai_note: srs-detail
vai_tro: BA
linh_vuc: y-te-ket-noi-thiet-bi
ngay_tao: 2026-05-16
tags:
  - ba
  - srs-detail
  - y-te
  - ket-noi-thiet-bi
  - pvm-2701
  - mapping
  - chi-so-song
---

# [SRS Detail] Danh sách ghép Chỉ số sống SAKURA - Chỉ số sống Monitor - Tích hợp thiết bị PVM-2701 vào HIS

| Thuộc tính | Nội dung |
| --- | --- |
| Trạng thái | YellowDraft |
| Người tạo | Hà Nguyễn |
| Ngày tạo | 16/05/2026 |
| Người review | Hà Nguyễn |
| Ngày review | [Cần cập nhật] |
| Người phê duyệt | Hà Nguyễn |
| Phân hệ | Tích hợp thiết bị / ISOFHTool |
| Ticket / CR / Jira | [Cần xác nhận] |
| Cơ sở y tế áp dụng | MEDI Plus Nam Định và các Cơ sở y tế triển khai cùng mô hình |
| Mức áp dụng | Cấu hình |
| Tài liệu high level liên quan | `2026-05-02_SRS_High level_Tich hop thiet bi PVM-2701 vao HIS.md` |

## 1. Tóm tắt

- Tài liệu này đặc tả chức năng `Danh sách ghép Chỉ số sống SAKURA - Chỉ số sống Monitor` trên ISOFHTool.
- Mục đích của chức năng là khai báo để mapping `Mã chỉ số trên SAKURA` với `Mã chỉ số Monitor` trong bản tin HL7 Monitor trả về.
- Danh mục này phục vụ để ISOFHTool hiểu chỉ số nào từ Monitor sẽ map vào chỉ số nào trên `SAKURA` khi xử lý dữ liệu sinh hiệu.
- Giai đoạn triển khai không bắt buộc hệ thống validate đúng hay sai của `Tên chỉ số`, `Mã chỉ số SAKURA`, `Mã chỉ số`; người dùng khai báo phải tự chịu trách nhiệm nhập đúng theo tài liệu triển khai.

## 2. Người dùng và vai trò

| Nhóm | Vai trò chính | Mức tham gia |
| --- | --- | --- |
| CNTT Cơ sở y tế | Khai báo và cập nhật danh mục mapping | Chính |
| Triển khai ISOFH | Hướng dẫn khai báo, rà soát cấu hình ban đầu | Chính |
| ISOFHTool | Đọc danh mục mapping khi xử lý dữ liệu HL7 từ Monitor | Chính ở xử lý hệ thống |

## 3. Phạm vi

### 3.1. Bao gồm

- Màn hình hoặc chức năng khai báo danh mục mapping giữa chỉ số trên `SAKURA` và chỉ số trong bản tin HL7 của Monitor.
- Xem danh sách mapping đã khai báo.
- Thêm mới mapping.
- Cập nhật mapping.
- Theo dõi `Ngày tạo` và `Ngày cập nhật` của từng dòng dữ liệu.

### 3.2. Không bao gồm

- Validate tính đúng hay sai của `Tên chỉ số`, `Mã chỉ số SAKURA`, `Mã chỉ số`.
- Rule chu kỳ, resend hoặc xử lý giao dịch gửi HIS.
- Danh mục thiết bị Monitor.
- Dashboard vận hành.

## 4. Fact / Assumption / Constraint / Open question

| Loại | Nội dung | Trạng thái |
| --- | --- | --- |
| Fact | Mục đích của danh mục là mapping `Mã chỉ số trên SAKURA` với `Mã chỉ số Monitor` trong bản tin HL7 Monitor trả về. | Đã rõ |
| Fact | `Tên chỉ số`, `Mã chỉ số SAKURA`, `Mã chỉ số Monitor` đều là trường nhập tay dạng text. | Đã rõ |
| Fact | `Ngày tạo` là trường chỉ đọc. | Đã rõ |
| Fact | `Ngày cập nhật` là trường chỉ đọc và lấy theo ngày sửa gần nhất. | Đã rõ |
| Constraint | `03` trường `Tên chỉ số`, `Mã chỉ số SAKURA`, `Mã chỉ số Monitor` không cần validate. | Đã rõ |
| Constraint | Khi triển khai sẽ yêu cầu người dùng khai báo đúng. | Đã rõ |
| Open question | Không có. | Đã rõ |

## 5. Luồng xử lý

### 5.1. Luồng 1 - Xem danh mục mapping

| Bước | Tác nhân | Mô tả xử lý | Kết quả |
| --- | --- | --- | --- |
| 1 | CNTT / Triển khai | Mở chức năng `Danh sách ghép Chỉ số sống SAKURA - Chỉ số sống Monitor` | Hệ thống hiển thị danh sách mapping đã có |
| 2 | Hệ thống | Hiển thị từng dòng dữ liệu theo cấu hình đang lưu | Người dùng xem được thông tin mapping hiện tại |

### 5.2. Luồng 2 - Thêm mới mapping

| Bước | Tác nhân | Mô tả xử lý | Kết quả |
| --- | --- | --- | --- |
| 1 | CNTT / Triển khai | Chọn thêm mới dòng mapping | Hệ thống mở vùng nhập liệu |
| 2 | CNTT / Triển khai | Nhập `Tên chỉ số`, `Mã chỉ số SAKURA`, `Mã chỉ số Monitor` | Có dữ liệu chờ lưu |
| 3 | Hệ thống | Lưu dữ liệu người dùng đã nhập mà không validate đúng sai của `03` trường text | Tạo mới một dòng mapping |
| 4 | Hệ thống | Tự sinh `Ngày tạo` theo thời điểm lưu | Hoàn tất thêm mới |

### 5.3. Luồng 3 - Cập nhật mapping

| Bước | Tác nhân | Mô tả xử lý | Kết quả |
| --- | --- | --- | --- |
| 1 | CNTT / Triển khai | Chọn một dòng mapping đã có | Hệ thống hiển thị thông tin hiện tại |
| 2 | CNTT / Triển khai | Sửa lại `Tên chỉ số`, `Mã chỉ số SAKURA`, `Mã chỉ số Monitor` nếu cần | Có dữ liệu chờ lưu |
| 3 | Hệ thống | Lưu dữ liệu người dùng đã sửa mà không validate đúng sai của `03` trường text | Dòng mapping được cập nhật |
| 4 | Hệ thống | Tự cập nhật `Ngày cập nhật` theo ngày sửa gần nhất | Hoàn tất cập nhật |

## 6. Quy tắc nghiệp vụ

- BR-01: Mỗi dòng trong danh mục đại diện cho một mapping giữa chỉ số trên `SAKURA` và chỉ số trong bản tin HL7 Monitor.
- BR-02: `Tên chỉ số` là trường dạng text do người dùng tự nhập.
- BR-03: `Mã chỉ số SAKURA` là trường dạng text do người dùng tự nhập.
- BR-04: `Mã chỉ số Monitor` là trường dạng text do người dùng tự nhập.
- BR-05: Hệ thống không validate tính đúng hay sai của `Tên chỉ số`, `Mã chỉ số SAKURA`, `Mã chỉ số Monitor`.
- BR-06: Trách nhiệm khai báo đúng thuộc về người dùng trong giai đoạn triển khai và vận hành.
- BR-07: `Ngày tạo` là trường chỉ đọc, hệ thống tự sinh khi tạo mới.
- BR-08: `Ngày cập nhật` là trường chỉ đọc, hệ thống tự cập nhật theo ngày sửa gần nhất.

## 7. Quy tắc kiểm tra dữ liệu

| Mã | Điều kiện kiểm tra | Kết quả khi không đạt |
| --- | --- | --- |
| VR-01 | Không áp dụng validate đúng sai cho `Tên chỉ số` | Hệ thống vẫn cho lưu |
| VR-02 | Không áp dụng validate đúng sai cho `Mã chỉ số SAKURA` | Hệ thống vẫn cho lưu |
| VR-03 | Không áp dụng validate đúng sai cho `Mã chỉ số Monitor` | Hệ thống vẫn cho lưu |

## 8. Điều kiện chấp nhận

- AC-01: Người dùng xem được danh sách các dòng mapping đã khai báo.
- AC-02: Người dùng thêm mới được một dòng mapping với `Tên chỉ số`, `Mã chỉ số SAKURA`, `Mã chỉ số Monitor`.
- AC-03: Hệ thống không validate đúng sai của `03` trường text khi lưu.
- AC-04: Hệ thống tự sinh `Ngày tạo` khi tạo mới.
- AC-05: Người dùng sửa được một dòng mapping đã có.
- AC-06: Hệ thống tự cập nhật `Ngày cập nhật` theo ngày sửa gần nhất.
- AC-07: `Ngày tạo` và `Ngày cập nhật` là trường chỉ đọc trên màn hình.

## 9. Trường hợp ngoại lệ

- EX-01: Người dùng nhập sai mã nhưng vẫn lưu.
  - Hệ thống không chặn.
  - Việc nhập sai sẽ được xử lý theo quy trình triển khai và đối soát cấu hình.

## 10. Message lỗi hoặc cảnh báo chính

- Không có message validate bắt buộc cho `Tên chỉ số`, `Mã chỉ số SAKURA`, `Mã chỉ số Monitor` trong phạm vi tài liệu này.

## 11. Trường thông tin

| Trường | Kiểu dữ liệu | Cách nhập | Bắt buộc | Mô tả |
| --- | --- | --- | --- | --- |
| `Tên chỉ số` | Text | Tự nhập | Có | Tên chỉ số dùng để người dùng đọc hiểu |
| `Mã chỉ số SAKURA` | Text | Tự nhập | Có | Mã chỉ số tương ứng trên `SAKURA` |
| `Mã chỉ số Monitor` | Text | Tự nhập | Có | Mã chỉ số trong bản tin HL7 Monitor trả về |
| `Ngày tạo` | DateTime | ReadOnly | Hệ thống tự sinh | Lấy theo thời điểm tạo mới bản ghi |
| `Ngày cập nhật` | DateTime | ReadOnly | Hệ thống tự cập nhật | Lấy theo ngày sửa gần nhất |

## 12. Danh sách hoặc tìm kiếm

### 12.1. Cột hiển thị tối thiểu

- `Tên chỉ số`.
- `Mã chỉ số SAKURA`.
- `Mã chỉ số Monitor`.
- `Ngày tạo`.
- `Ngày cập nhật`.

### 12.2. Tìm kiếm

- Phase I chưa cần chức năng tìm kiếm.
- Khi bổ sung tìm kiếm ở giai đoạn sau, hệ thống hỗ trợ tìm theo các tiêu chí:
  - `Tên chỉ số`.
  - `Mã chỉ số SAKURA`.
  - `Mã chỉ số Monitor`.
- `Ngày tạo`, `Ngày cập nhật` chưa cần đưa vào tiêu chí tìm kiếm ở Phase I; nếu phát sinh nhu cầu sẽ bổ sung sau.

## 13. Trạng thái dữ liệu

- Không áp dụng trạng thái nghiệp vụ riêng cho bản ghi trong phạm vi tài liệu này.

## 14. Dữ liệu hoặc nghiệp vụ phát sinh

| Hạng mục | Mô tả |
| --- | --- |
| Bản ghi mapping | Dữ liệu cấu hình ghép chỉ số giữa `SAKURA` và bản tin HL7 Monitor |
| `Ngày tạo` | Dữ liệu hệ thống tự sinh khi tạo mới |
| `Ngày cập nhật` | Dữ liệu hệ thống tự cập nhật khi sửa bản ghi |

## 15. Ghi chú dữ liệu hoặc kỹ thuật

- Tài liệu này chỉ chốt phạm vi nghiệp vụ của danh mục mapping.
- Không tự bổ sung rule validate nếu chưa có yêu cầu mới.
- Nếu về sau cần chặn trùng, chặn rỗng hoặc validate theo danh mục chuẩn thì phải cập nhật version tài liệu sau.

## 16. Báo cáo, in, xuất liên quan

- Không nằm trong phạm vi của tài liệu này.

## 17. Liên kết

- HLR: `2026-05-02_SRS_High level_Tich hop thiet bi PVM-2701 vao HIS.md`
- Danh sách thiết bị Monitor: `2026-05-16_SRS_Detail_Khai bao danh sach thiet bi Monitor_Tich hop thiet bi PVM-2701 vao HIS.md`
- Find Patient: `2026-05-16_SRS_Detail_Nhan yeu cau gui thong tin NB va tra thong tin cho Monitor_Tich hop thiet bi PVM-2701 vao HIS.md`
- Nhận dữ liệu sinh hiệu: `2026-05-04_SRS_Detail_API mapping HL7 va rule chu ky_Tich hop thiet bi PVM-2701 vao HIS.md`

## 18. Điểm cần xác nhận trước khi triển khai

- Không có.

## 19. Checklist BA trước khi chốt

- [x] Đã viết lại đúng mục đích của danh mục mapping.
- [x] Đã chốt đúng `05` trường thông tin cần có.
- [x] Đã chốt `03` trường text không cần validate, gồm `Tên chỉ số`, `Mã chỉ số SAKURA`, `Mã chỉ số Monitor`.
- [x] Đã chốt `Ngày tạo`, `Ngày cập nhật` là trường chỉ đọc.
- [x] Đã giữ đúng tinh thần người dùng phải tự khai báo đúng khi triển khai.
