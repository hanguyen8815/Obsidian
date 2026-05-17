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
  - find-patient
  - hl7
---

# [SRS Detail] Nhận yêu cầu gửi thông tin Người bệnh và trả thông tin cho Monitor - Tích hợp thiết bị PVM-2701 vào HIS

| Thuộc tính | Nội dung |
| --- | --- |
| Trạng thái | YellowDraft |
| Người tạo | Hà Nguyễn |
| Ngày tạo | 16/05/2026 |
| Người review | Hà Nguyễn |
| Ngày review | [Cần cập nhật] |
| Người phê duyệt | Hà Nguyễn |
| Phân hệ | Sinh hiệu / Tích hợp thiết bị / ISOFHTool |
| Ticket / CR / Jira | [Cần xác nhận] |
| Cơ sở y tế áp dụng | Pilot tại MEDI Plus Nam Định, định hướng dùng chung cho nhiều Cơ sở y tế |
| Mức áp dụng | Tích hợp |
| Tài liệu high level liên quan | `2026-05-02_SRS_High level_Tich hop thiet bi PVM-2701 vao HIS.md` |

## 1. Tóm tắt

- Tài liệu này đặc tả luồng Monitor gửi yêu cầu lấy thông tin Người bệnh qua `QRY^A19`, ISOFHTool gọi sang `SAKURA` để lấy thông tin và phản hồi lại Monitor bằng `ADR^A19`.
- Đây là luồng bắt buộc trước khi đo theo tích hợp tự động.
- Dữ liệu tra cứu dùng `Mã hồ sơ` và chỉ lấy các trường hợp còn hiệu lực điều trị với trạng thái `< Đã ra viện (100)`.

## 2. Người dùng và vai trò

| Nhóm | Vai trò chính | Mức tham gia |
| --- | --- | --- |
| Điều dưỡng / Nhân viên y tế | Quét `Mã hồ sơ`, bấm `Find Patient`, đối chiếu thông tin trước khi đo | Chính |
| Monitor | Gửi `QRY^A19`, nhận `ADR^A19`, hiển thị thông tin Người bệnh | Chính ở thiết bị |
| ISOFHTool | Nhận bản tin HL7, gọi API `SAKURA`, parse phản hồi và trả lại Monitor | Chính ở xử lý hệ thống |
| `SAKURA` | Trả thông tin Người bệnh hợp lệ theo `Mã hồ sơ` | Chính ở hệ thống đích |

## 3. Phạm vi

### 3.1. Bao gồm

- Nhận yêu cầu `Find Patient` từ Monitor bằng bản tin HL7 `QRY^A19`.
- Parse `Mã hồ sơ` từ bản tin yêu cầu.
- Gọi API `nbDotDieuTriId` của `SAKURA` để lấy thông tin Người bệnh.
- Chỉ trả kết quả cho các trường hợp có trạng thái `< Đã ra viện (100)`.
- Parse dữ liệu nghiệp vụ thành bản tin HL7 `ADR^A19` để trả lại Monitor.

### 3.2. Không bao gồm

- Rule nhận dữ liệu sinh hiệu `ORU^R01`.
- Rule `ACK` sau khi nhận sinh hiệu.
- Dashboard nghiệp vụ hoặc dashboard vận hành.

## 4. Fact / Assumption / Constraint / Open question

| Loại | Nội dung | Trạng thái |
| --- | --- | --- |
| Fact | Quy trình ghép Người bệnh trên Monitor dùng `Mã hồ sơ` và chức năng `Find Patient`. | Đã rõ |
| Fact | `Find Patient` là thao tác bắt buộc trước khi đo theo luồng tự động. | Đã rõ |
| Fact | Monitor gửi bản tin HL7 `QRY^A19` đến ISOFHTool. | Đã rõ |
| Fact | ISOFHTool gọi API `nbDotDieuTriId` của `SAKURA` để lấy thông tin Người bệnh. | Đã rõ |
| Fact | Chỉ lấy các trường hợp có trạng thái `< Đã ra viện (100)`. | Đã rõ |
| Fact | ISOFHTool trả lại Monitor bằng bản tin HL7 `ADR^A19`. | Đã rõ |
| Constraint | Nếu Monitor không hiển thị đủ thông tin tối thiểu thì không được đo theo luồng tự động. | Đã rõ |
| Open question | `Tuổi` sẽ do `SAKURA` trả sẵn hay Tool suy ra từ `Ngày sinh`. | Mở |

## 5. Luồng xử lý

### 5.1. Luồng chính

| Bước | Tác nhân | Mô tả xử lý | Kết quả |
| --- | --- | --- | --- |
| 1 | Điều dưỡng | Quét barcode `Mã hồ sơ` trên Monitor | Monitor có giá trị để tra cứu |
| 2 | Điều dưỡng | Chọn `Find Patient` | Monitor phát sinh yêu cầu tra cứu |
| 3 | Monitor | Gửi bản tin HL7 `QRY^A19` sang ISOFHTool | Tool nhận request |
| 4 | ISOFHTool | Parse `Mã hồ sơ` từ bản tin request | Có dữ liệu để gọi `SAKURA` |
| 5 | ISOFHTool | Gọi API `nbDotDieuTriId` của `SAKURA` theo `Mã hồ sơ` | `SAKURA` nhận yêu cầu |
| 6 | `SAKURA` | Kiểm tra hồ sơ có tồn tại và có trạng thái `< Đã ra viện (100)` hay không | Có hoặc không có kết quả hợp lệ |
| 7a | `SAKURA` | Nếu hợp lệ, trả thông tin Người bệnh cho ISOFHTool | Tool nhận thông tin hợp lệ |
| 7b | `SAKURA` | Nếu không hợp lệ, trả kết quả không tìm thấy hoặc không đủ điều kiện | Tool nhận kết quả không hợp lệ |
| 8a | ISOFHTool | Parse dữ liệu trả về thành bản tin HL7 `ADR^A19` | Có bản tin phản hồi cho Monitor |
| 8b | ISOFHTool | Gửi `ADR^A19` về Monitor | Monitor nhận phản hồi |
| 9 | Monitor | Hiển thị tối thiểu `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính`; nếu hỗ trợ thì hiển thị thêm `Tuổi` | Điều dưỡng đối chiếu trước khi đo |
| 10 | Điều dưỡng | Chỉ tiếp tục đo khi thông tin hiển thị đúng và đủ | Được đi tiếp sang luồng đo sinh hiệu |

## 6. Quy tắc nghiệp vụ

- BR-01: `Mã hồ sơ` là định danh dùng để tra cứu thông tin Người bệnh cho tích hợp này.
- BR-02: Một `Mã hồ sơ` chỉ tương ứng với một lượt hoặc đợt điều trị hợp lệ trong phạm vi tích hợp.
- BR-03: `Find Patient` là bước bắt buộc trước khi đo theo luồng tự động.
- BR-04: Chỉ trả thông tin Người bệnh khi hồ sơ có trạng thái `< Đã ra viện (100)`.
- BR-05: Nếu `Mã hồ sơ` không tồn tại hoặc đã ra viện thì không trả thông tin để tránh đo vào hồ sơ cũ.
- BR-06: ISOFHTool phải phản hồi lại Monitor bằng bản tin HL7 `ADR^A19` theo dữ liệu `SAKURA` trả về.
- BR-07: Monitor phải hiển thị tối thiểu `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính`; nếu thiếu một trong các trường này thì không được tiếp tục đo theo luồng tự động.

## 7. Quy tắc kiểm tra dữ liệu

| Mã | Điều kiện kiểm tra | Kết quả khi không đạt |
| --- | --- | --- |
| VR-01 | Bản tin nhận từ Monitor phải là `QRY^A19` | Không gọi `SAKURA`, ghi log kỹ thuật |
| VR-02 | Parse được `Mã hồ sơ` từ request | Không gọi `SAKURA`, ghi log kỹ thuật |
| VR-03 | `SAKURA` trả được thông tin Người bệnh hợp lệ | Không cho tiếp tục đo theo luồng tự động |
| VR-04 | Trạng thái hồ sơ `< Đã ra viện (100)` | Không trả thông tin cho Monitor |
| VR-05 | Dữ liệu phản hồi đủ `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính` để parse `ADR^A19` | Không cho tiếp tục đo theo luồng tự động |

## 8. Điều kiện chấp nhận

- AC-01: Khi bấm `Find Patient`, Monitor gửi bản tin `QRY^A19` sang ISOFHTool.
- AC-02: ISOFHTool parse được `Mã hồ sơ` và gọi API `nbDotDieuTriId` của `SAKURA`.
- AC-03: Nếu `Mã hồ sơ` hợp lệ và có trạng thái `< Đã ra viện (100)`, `SAKURA` trả dữ liệu để Tool tạo `ADR^A19`.
- AC-04: ISOFHTool gửi được `ADR^A19` về Monitor.
- AC-05: Monitor hiển thị tối thiểu `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính`.
- AC-06: Nếu hồ sơ không hợp lệ hoặc đã ra viện thì không trả thông tin cho Monitor.
- AC-07: Nếu Monitor hiển thị thiếu thông tin tối thiểu thì không được tiếp tục đo theo luồng tự động.

## 9. Trường hợp ngoại lệ

- EX-01: Bản tin nhận từ Monitor không phải `QRY^A19`.
- EX-02: Không parse được `Mã hồ sơ` từ request.
- EX-03: `Mã hồ sơ` không tồn tại trên `SAKURA`.
- EX-04: `Mã hồ sơ` đã ở trạng thái `Đã ra viện (100)` hoặc lớn hơn.
- EX-05: `SAKURA` lỗi hoặc timeout khi tra cứu.
- EX-06: Tool tạo được phản hồi nhưng Monitor hiển thị thiếu thông tin tối thiểu.

## 10. Message lỗi hoặc cảnh báo chính

| Mã | Tình huống | Message / cảnh báo |
| --- | --- | --- |
| MSG-01 | Request không đúng loại `QRY^A19` | `[Cần xác nhận] Bản tin tra cứu Người bệnh không đúng định dạng` |
| MSG-02 | Không tìm thấy hồ sơ hợp lệ | `[Cần xác nhận] Không tìm thấy Người bệnh hợp lệ theo Mã hồ sơ` |
| MSG-03 | Hồ sơ đã ra viện | `[Cần xác nhận] Hồ sơ đã ra viện, không thể ghép trên Monitor` |
| MSG-04 | Monitor hiển thị thiếu thông tin tối thiểu | `[Cần xác nhận] Thông tin Người bệnh hiển thị chưa đủ để tiếp tục đo` |

## 11. Trường thông tin

### 11.1. Request từ Monitor sang Tool

| Trường | Bắt buộc | Nguồn | Ghi chú |
| --- | --- | --- | --- |
| `MSH-9` | Có | Monitor | Giá trị kỳ vọng `QRY^A19` |
| `MSH-10` | Có | Monitor | `messageControlId` để đối soát request / response |
| `QRD-1` | Có | Monitor | Thời điểm request |
| `QRD-4` | Có | Monitor | Mã truy vấn của Monitor |
| `QRD-8` | Có | Monitor | `Mã hồ sơ` dùng để tra cứu trên `SAKURA` |

### 11.2. Response từ Tool về Monitor

| Trường | Bắt buộc | Nguồn | Ghi chú |
| --- | --- | --- | --- |
| `MSH-9` | Có | Tool | Giá trị phản hồi `ADR^A19` |
| `MSA-1` | Có | Tool | Giá trị thành công theo bản tin HL7 đã chốt |
| `MSA-2` | Có | Tool | Trả lại đúng `messageControlId` của request |
| `PID-3` | Có | `SAKURA` | `Mã hồ sơ` |
| `PID-5` | Có | `SAKURA` | `Họ tên` |
| `PID-7` | Có | `SAKURA` | `Ngày sinh` |
| `PID-8` | Có | `SAKURA` | `Giới tính` |
| `Tuổi` | Không | `SAKURA` hoặc Tool | `[Cần xác nhận]` cách lấy giá trị |

## 12. Danh sách hoặc tìm kiếm

- Không áp dụng cho chức năng API nghiệp vụ này.

## 13. Trạng thái dữ liệu

- Không phát sinh trạng thái bản ghi nghiệp vụ riêng trong `SAKURA` từ luồng `Find Patient`.
- ISOFHTool chỉ cần lưu log truy vết request và response.

## 14. Dữ liệu hoặc nghiệp vụ phát sinh

| Hạng mục | Mô tả |
| --- | --- |
| Log request `Find Patient` | Lưu `Mã hồ sơ`, thời điểm request, `messageControlId`, kết quả tra cứu |
| Log response `ADR^A19` | Lưu kết quả phản hồi để đối soát khi cần |

## 15. Ghi chú dữ liệu hoặc kỹ thuật

- Chưa chốt tên endpoint kỹ thuật thật, method HTTP thật và cơ chế auth thật giữa ISOFHTool và `SAKURA`.
- Tài liệu này không tự sinh tên bảng DB hoặc tên trường kỹ thuật ngoài các trường HL7 đã có căn cứ.

## 16. Báo cáo, in, xuất liên quan

- Không nằm trong phạm vi của tài liệu này.

## 17. Liên kết

- HLR: `2026-05-02_SRS_High level_Tich hop thiet bi PVM-2701 vao HIS.md`
- API, mapping HL7 và rule chu kỳ: `2026-05-04_SRS_Detail_API mapping HL7 va rule chu ky_Tich hop thiet bi PVM-2701 vao HIS.md`
- HDSD vận hành: `2026-05-02_HDSD_Van hanh tich hop thiet bi PVM-2701 vao HIS.md`

## 18. Điểm cần xác nhận trước khi triển khai

- `Tuổi` sẽ do `SAKURA` trả sẵn hay Tool suy ra từ `Ngày sinh`.
- Text message hoặc cách Monitor hiển thị khi không tìm thấy Người bệnh thuộc phạm vi thiết bị hay cần log thêm ở Tool tới mức nào.
- Tên endpoint và contract kỹ thuật thật của API `nbDotDieuTriId`.

## 19. Checklist BA trước khi chốt

- [x] Đã mô tả rõ luồng `Find Patient` từ Monitor tới `SAKURA` và trả về Monitor.
- [x] Đã chốt điều kiện lọc trạng thái `< Đã ra viện (100)`.
- [x] Đã chốt thông tin tối thiểu phải hiển thị trên Monitor.
- [x] Đã có ngoại lệ và điều kiện chấp nhận chính.
- [x] Đã nêu rõ các điểm còn cần xác nhận kỹ thuật.
