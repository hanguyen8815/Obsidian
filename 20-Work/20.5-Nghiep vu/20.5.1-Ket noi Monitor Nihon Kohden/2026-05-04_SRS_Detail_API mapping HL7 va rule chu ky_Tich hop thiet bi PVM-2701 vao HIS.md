---
loai_note: srs-detail
vai_tro: BA
linh_vuc: y-te-ket-noi-thiet-bi
ngay_tao: 2026-05-04
tags:
  - ba
  - srs-detail
  - y-te
  - ket-noi-thiet-bi
  - pvm-2701
  - hl7
---

# [SRS Detail] Nhận dữ liệu sinh hiệu từ Monitor và chuyển cho SAKURA - API, mapping HL7, ACK và rule chu kỳ - Tích hợp thiết bị PVM-2701 vào HIS

| Thuộc tính | Nội dung |
| --- | --- |
| Trạng thái | YellowDraft |
| Người tạo | Hà Nguyễn |
| Ngày tạo | 04/05/2026 |
| Người review | Hà Nguyễn |
| Ngày review | [Cần cập nhật] |
| Người phê duyệt | Hà Nguyễn |
| Phân hệ | Sinh hiệu / Tích hợp thiết bị / Hồ sơ điều trị |
| Ticket / CR / Jira | [Cần xác nhận] |
| Cơ sở y tế áp dụng | Pilot tại MEDI Plus, định hướng dùng chung cho nhiều Cơ sở y tế |
| Mức áp dụng | Tích hợp + Cấu hình |
| Tài liệu high level liên quan | `2026-05-02_SRS_High level_Tich hop thiet bi PVM-2701 vao HIS.md` |

## 1. Tóm tắt

- Tài liệu này đặc tả chi tiết cho luồng nhận dữ liệu sinh hiệu từ Monitor, phản hồi `ACK`, chuẩn hóa dữ liệu HL7, áp rule chu kỳ và gọi API cập nhật sinh hiệu sang `SAKURA`.
- Tại bước gửi dữ liệu sinh hiệu, Monitor gửi bản tin HL7 `ORU^R01` sang ISOFHTool; Tool phản hồi lại bản tin HL7 `ACK`, lấy `thời điểm đo` từ `OBR-7` hoặc `OBX-14` theo rule tại mục 9 và 10 để gom dữ liệu theo chu kỳ rồi gửi sang `SAKURA`.
- Luồng `Find Patient` đã được tách sang tài liệu riêng: `2026-05-16_SRS_Detail_Nhan yeu cau gui thong tin NB va tra thong tin cho Monitor_Tich hop thiet bi PVM-2701 vao HIS.md`.
- Danh sách ghép chỉ số sống được tách sang tài liệu riêng: `2026-05-16_SRS_Detail_Danh sach ghep Chi so song SAKURA - Chi so song Monitor_Tich hop thiet bi PVM-2701 vao HIS.md`.
- Tài liệu này phục vụ handover cho DEV, QA, BA và triển khai ở mức detail.
- Tài liệu này chưa chốt tên endpoint kỹ thuật thật, tên bảng DB, enum kỹ thuật, text message hiển thị cuối cùng cho người dùng. Các phần đó được đánh dấu `[Cần xác nhận]` nếu chưa có căn cứ.

## 2. Người dùng và vai trò

| Nhóm | Vai trò chính | Mức tham gia |
| --- | --- | --- |
| Điều dưỡng | Quét `Mã hồ sơ`, đối chiếu Người bệnh trên Monitor, thực hiện đo | Chính ở nghiệp vụ |
| ISOFHTool | Tra cứu Người bệnh, nhận bản tin HL7, áp rule chu kỳ, gửi HIS, lưu log, retry, resend | Chính ở xử lý kỹ thuật |
| HIS | Trả thông tin Người bệnh theo rule, nhận dữ liệu sinh hiệu, chống trùng, lưu dữ liệu | Chính ở hệ thống đích |
| CNTT Cơ sở y tế | Cấu hình Tool, theo dõi lỗi, thực hiện resend | Chính ở vận hành |
| Triển khai ISOFH | Hỗ trợ cấu hình, go-live, theo dõi lỗi giai đoạn đầu | Chính ở triển khai |
| DEV / QA | Xây dựng, kiểm thử và đối soát luồng tích hợp | Phụ |

## 3. Phạm vi

### 3.1. Bao gồm

- Đặc tả payload nghiệp vụ tối thiểu cho API gửi dữ liệu sinh hiệu vào HIS.
- Đặc tả mapping các thành phần HL7 cần dùng từ Monitor PVM-2701.
- Đặc tả rule phản hồi `ACK` sau khi ISOFHTool nhận `ORU^R01`.
- Đặc tả rule chu kỳ `10 phút` của Phase I.
- Đặc tả rule chọn gói dữ liệu cuối cùng trong chu kỳ.
- Đặc tả xử lý bản ghi đến trễ theo `OBR-7` hoặc `OBX-14` của cùng `PID-3`.
- Đặc tả rule retry, resend và trạng thái giao dịch gửi HIS.
- Đặc tả dữ liệu trắng, log kỹ thuật và các lỗi chính cần theo dõi.

### 3.2. Không bao gồm

- Thiết kế UI chi tiết của dashboard vận hành lỗi.
- Thiết kế DB chi tiết, tên bảng, tên cột kỹ thuật thật.
- Chi tiết text message cuối cùng cho từng popup hoặc toast trên Tool hay HIS.
- Rule phân quyền chi tiết theo user-level trên ISOFHTool ở Phase I.
- Dashboard báo cáo vận hành sau pilot.

## 4. Fact / Assumption / Constraint / Open question

| Loại | Nội dung | Trạng thái |
| --- | --- | --- |
| Fact | Bản tin sinh hiệu Monitor gửi sang Tool là HL7 `ORU^R01`. | Đã rõ |
| Fact | Khi ISOFHTool nhận được bản tin HL7 `ORU^R01` từ Monitor thì phải phản hồi lại bản tin HL7 `ACK` để Monitor duy trì kết nối và tiếp tục gửi các bản tin tiếp theo nếu có. | Đã rõ |
| Fact | Chu kỳ ghi nhận sinh hiệu ở Phase I là `10 phút`, tính từ thời điểm `Find Patient`. | Đã rõ |
| Fact | Mỗi lần `Find Patient` thành công sẽ mở một chu kỳ mới; nếu đang có chu kỳ mở thì chu kỳ cũ đóng tại thời điểm `Find Patient` mới. | Đã rõ |
| Fact | Trong cùng một chu kỳ, Tool gom nhiều bản tin `ORU^R01` thành 1 gói dữ liệu gửi HIS; mỗi chỉ số lấy giá trị hợp lệ cuối cùng theo `thời điểm đo` chuẩn trong chu kỳ. | Đã rõ |
| Fact | `thời điểm đo` chuẩn của gói dữ liệu được lấy theo giá trị muộn nhất trong các chỉ số đã chọn của chu kỳ. | Đã rõ |
| Fact | Bản ghi đến trễ được xử lý theo `OBR-7` hoặc `OBX-14` của cùng `PID-3`, vẫn nhận vào chu kỳ đã đóng và cho phép resend nếu làm thay đổi gói dữ liệu cuối cùng đã gửi. | Đã rõ |
| Fact | Dữ liệu trắng vẫn đi vào Tool nhưng chỉ lưu log kỹ thuật và bảng lỗi với `Lý do lỗi = Không có Mã hồ sơ`, không gửi sang HIS. | Đã rõ |
| Fact | Phase I: lỗi kết nối retry tự động `3 lần`; lỗi dữ liệu không retry tự động. | Đã rõ |
| Fact | Phase I trước mắt tập trung `05` nhóm chỉ số sống cơ bản: `Mạch`, `Nhiệt độ`, `Huyết áp`, `Nhịp thở`, `SpO2`. | Đã rõ |
| Constraint | Giai đoạn đầu ưu tiên pilot nhanh, hạn chế thay đổi lớn về hạ tầng. | Đã rõ |
| Constraint | Không được làm mất dữ liệu đã nhận từ Monitor khi HIS lỗi tạm thời. | Đã rõ |
| Open question | Tên endpoint thật, phương thức HTTP thật, cơ chế auth thật giữa Tool và HIS là gì. | Mở |
| Open question | Thời gian giữ log kỹ thuật và bảng lỗi trên ISOFHTool là bao lâu. | Mở |
| Open question | Tên trường kỹ thuật thật trên HIS để lưu `Mã máy`, `thời điểm đo`, `mã giao dịch Tool`, `Lý do lỗi` là gì. | Mở |

## 5. Luồng xử lý

### 5.1. Luồng 1 - Nhận bản tin HL7, phản hồi `ACK` và áp rule chu kỳ

| Bước | Tác nhân | Mô tả xử lý | Kết quả |
| --- | --- | --- | --- |
| 1 | Monitor | Gửi bản tin HL7 sang ISOFHTool sau khi đo | Tool nhận bản tin |
| 2 | ISOFHTool | Parse bản tin HL7 `ORU^R01`, đọc `PID-3`, `OBR-7`, `OBX-14`, `OBX-3`, `OBX-5`, `OBX-6` | Có dữ liệu đầu vào để xử lý |
| 3 | ISOFHTool | Trả bản tin HL7 `ACK` cho Monitor sau khi nhận và parse được bản tin ở mức tối thiểu để tiếp tục xử lý | Monitor duy trì kết nối và tiếp tục gửi dữ liệu nếu có |
| 4 | ISOFHTool | Xác định `Mã hồ sơ` từ `PID-3` | Biết ngữ cảnh Người bệnh hoặc xác định dữ liệu trắng |
| 5a | ISOFHTool | Nếu không có `PID-3` hoặc không xác định được `Mã hồ sơ` hợp lệ theo luồng đang mở | Ghi log kỹ thuật và bảng lỗi với `Lý do lỗi = Không có Mã hồ sơ`; không gửi HIS |
| 5b | ISOFHTool | Nếu có `PID-3`, xác định `thời điểm đo` chuẩn theo rule ưu tiên `OBR-7`, fallback `OBX-14` ở mục 9 | Có thời điểm đo chuẩn để vào chu kỳ |
| 6 | ISOFHTool | Xác định chu kỳ theo `Mã hồ sơ` và thời điểm `Find Patient` thành công gần nhất; mỗi lần `Find Patient` thành công mở một chu kỳ mới | Bản tin được gán vào đúng chu kỳ đang mở hoặc chu kỳ cũ nếu là bản ghi đến trễ |
| 7 | ISOFHTool | Dùng danh sách ghép chỉ số sống đang hiệu lực để xác định chỉ số nào được gửi `SAKURA`, chỉ số nào chỉ log kỹ thuật | Có bộ chỉ số đủ điều kiện cho chu kỳ |
| 8 | ISOFHTool | Gom các bản tin của cùng chu kỳ thành 1 gói dữ liệu; với từng chỉ số, giữ giá trị hợp lệ cuối cùng theo `thời điểm đo` chuẩn, nếu trùng `thời điểm đo` thì lấy bản tin Tool nhận sau cùng | Có tập chỉ số cuối cùng của chu kỳ |
| 9 | ISOFHTool | Khi chu kỳ đóng, tạo 1 giao dịch `Chờ gửi HIS` chứa toàn bộ các chỉ số cuối cùng hợp lệ của chu kỳ | Có transaction sẵn sàng gửi HIS |
| 10 | ISOFHTool | Gửi 1 gói dữ liệu tổng hợp sang HIS kèm `Mã máy`, `thời điểm đo` gói và danh sách chỉ số đã gom | HIS nhận bản ghi sinh hiệu |
| 11a | HIS | Nhận thành công | Tool cập nhật `Đã gửi HIS` |
| 11b | HIS / Tool | Lỗi kết nối hoặc lỗi dữ liệu | Tool cập nhật `Gửi HIS lỗi` theo rule phase |

### 5.2. Luồng 2 - Xử lý bản ghi đến trễ

| Bước | Tác nhân | Mô tả xử lý | Kết quả |
| --- | --- | --- | --- |
| 1 | ISOFHTool | Nhận bản tin sau thời điểm chu kỳ liên quan đã đóng | Phát hiện bản ghi đến trễ |
| 2 | ISOFHTool | Dùng `thời điểm đo` chuẩn lấy từ `OBR-7` hoặc `OBX-14` của cùng `PID-3` để xác định chu kỳ thực tế, không dùng thời điểm Tool nhận bản tin | Gán đúng chu kỳ cũ |
| 3a | ISOFHTool | Nếu bản ghi đến trễ không làm thay đổi bộ chỉ số cuối cùng của gói dữ liệu chu kỳ | Chỉ lưu log kỹ thuật |
| 3b | ISOFHTool | Nếu bản ghi đến trễ làm thay đổi ít nhất 1 chỉ số cuối cùng hoặc làm thay đổi `thời điểm đo` gói của chu kỳ | Đưa vào diện cần resend để cập nhật lại HIS |
| 4 | CNTT / Triển khai | Thực hiện resend theo từng bản ghi khi kiểm tra hợp lệ | HIS nhận lại dữ liệu cập nhật |

## 6. Quy tắc nghiệp vụ

- BR-01: Bản tin nhận sinh hiệu từ Monitor trong phạm vi tích hợp này là `ORU^R01`.
- BR-02: Sau khi ISOFHTool nhận và parse được bản tin ở mức tối thiểu, Tool phải phản hồi lại Monitor bằng bản tin HL7 `ACK` để Monitor duy trì kết nối và tiếp tục gửi dữ liệu nếu có.
- BR-03: `Mã hồ sơ` dùng trong tích hợp phải xác định duy nhất đúng 1 lượt khám hoặc 1 đợt điều trị trong HIS.
- BR-04: Chu kỳ ghi nhận sinh hiệu ở Phase I là `10 phút`, tính từ thời điểm `Find Patient`.
- BR-05: Mỗi lần `Find Patient` thành công đều mở một chu kỳ mới. Nếu đang có chu kỳ mở, chu kỳ cũ đóng ngay tại thời điểm `Find Patient` mới, kể cả khi quét lại cùng `Mã hồ sơ`.
- BR-06: Trong cùng một chu kỳ, Tool gom nhiều bản tin `ORU^R01` thành 1 gói dữ liệu duy nhất để gửi sang HIS khi chu kỳ đóng.
- BR-07: Với từng chỉ số trong cùng một chu kỳ, Tool chọn giá trị hợp lệ cuối cùng theo `thời điểm đo` chuẩn lấy từ `OBR-7`, nếu `OBR-7` trống thì fallback `OBX-14`.
- BR-08: Nếu có nhiều giá trị của cùng 1 chỉ số có cùng `thời điểm đo` chuẩn trong cùng một chu kỳ thì ưu tiên giá trị từ bản tin Tool nhận sau cùng.
- BR-09: `thời điểm đo` của gói dữ liệu gửi HIS là `thời điểm đo` muộn nhất trong các chỉ số cuối cùng đã được chọn của chu kỳ.
- BR-10: HIS chống trùng theo `Mã hồ sơ + thời điểm đo` của gói dữ liệu sinh hiệu.
- BR-11: Mốc thời gian chuẩn để gom chu kỳ, chọn chỉ số cuối cùng, chống trùng và resend là `thời điểm đo` trong bản tin HL7, không dùng thời điểm Tool nhận bản tin.
- BR-12: Bản ghi đến trễ được xử lý theo `OBR-7` hoặc `OBX-14` của cùng `PID-3`, vẫn nhận vào chu kỳ đã đóng.
- BR-13: Nếu bản ghi đến trễ làm thay đổi ít nhất 1 chỉ số cuối cùng hoặc làm thay đổi `thời điểm đo` gói dữ liệu của chu kỳ thì Tool phải cho phép resend để cập nhật lại HIS.
- BR-14: Dữ liệu trắng không gửi sang HIS.
- BR-15: Sai `Mã máy` không được xem là lỗi resend nếu HIS vẫn nhận dữ liệu thành công.
- BR-16: Phase I chỉ bắt buộc gửi `05` nhóm chỉ số sống cơ bản: `Mạch`, `Nhiệt độ`, `Huyết áp`, `Nhịp thở`, `SpO2`.
- BR-17: Các chỉ số mở rộng như `MAP`, `PI`, `rPR` không mặc định đưa vào phạm vi gửi `SAKURA` ở Phase I nếu chưa có xác nhận bổ sung.
- BR-18: Phase I: lỗi kết nối retry tự động `3 lần`; lỗi dữ liệu không retry tự động.
- BR-19: Phase I bắt buộc hỗ trợ resend thủ công theo từng giao dịch lỗi.

## 7. Quy tắc kiểm tra dữ liệu

### 7.1. Validate cho `Find Patient`

| Mã | Điều kiện kiểm tra | Kết quả khi không đạt |
| --- | --- | --- |
| VR-FP-00 | Monitor gửi đúng loại bản tin HL7 `QRY^A19` cho thao tác `Find Patient` | Ghi log kỹ thuật, không tra cứu HIS |
| VR-FP-01 | Có `Mã hồ sơ` gửi sang HIS | Không trả thông tin Người bệnh |
| VR-FP-02 | `Mã hồ sơ` tồn tại trên HIS | Không trả thông tin Người bệnh |
| VR-FP-03 | `Mã hồ sơ` thuộc lượt hoặc đợt điều trị có trạng thái `< Đã ra viện (100)` | Không trả thông tin Người bệnh |
| VR-FP-04 | HIS trả đủ dữ liệu để ISOFHTool parse bản tin HL7 `ADR^A19` phản hồi cho Monitor | Ghi log kỹ thuật, không cho tiếp tục đo theo luồng tự động |
| VR-FP-05 | Kết quả hiển thị trên Monitor có tối thiểu `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính` | Không cho tiếp tục đo theo luồng tự động |

### 7.2. Validate cho bản tin HL7 nhận từ Monitor

| Mã | Điều kiện kiểm tra | Kết quả khi không đạt |
| --- | --- | --- |
| VR-HL7-01 | Có bản tin HL7 hợp lệ để parse | Ghi log kỹ thuật, đưa vào bảng lỗi |
| VR-HL7-02 | Có `PID-3` hoặc xác định được `Mã hồ sơ` theo rule mapping | Nếu không có thì xem là dữ liệu trắng, không gửi HIS |
| VR-HL7-03 | Có `OBR-7` hoặc `OBX-14` để xác định `thời điểm đo` chuẩn | Ghi lỗi dữ liệu, không gửi HIS |
| VR-HL7-04 | Có ít nhất 1 chỉ số hợp lệ từ `OBX-3` và `OBX-5` để đưa vào gói dữ liệu của chu kỳ | Ghi lỗi dữ liệu, không gửi HIS |
| VR-HL7-05 | `Mã máy` đã được cấu hình trên Tool | `[Cần xác nhận]` có chặn gửi hay chỉ log cảnh báo; hiện tại nếu HIS vẫn nhận thì không xem là lỗi resend |
| VR-HL7-06 | Nếu `OBR-7` trống và dùng `OBX-14` fallback thì các chỉ số được chọn trong cùng một gói phải xác định được `thời điểm đo` hợp lệ | Nếu không xác định được thì ghi lỗi dữ liệu, không gửi HIS |
| VR-HL7-07 | Chỉ số nhận được phải có trong danh sách ghép chỉ số sống đang hiệu lực nếu muốn gửi `SAKURA` | Nếu không có thì chỉ log kỹ thuật, không tự gửi |

## 8. API nghiệp vụ cần có

### 8.1. API 1 - Gửi dữ liệu sinh hiệu vào HIS

#### Mục tiêu

- HIS nhận 1 gói dữ liệu sinh hiệu tổng hợp cho mỗi chu kỳ từ ISOFHTool.

#### Đầu vào nghiệp vụ tối thiểu

| Trường | Bắt buộc | Nguồn | Ghi chú |
| --- | --- | --- | --- |
| `maHoSo` | Có | Từ `PID-3` sau khi Tool parse | Dùng để gắn dữ liệu với đúng hồ sơ |
| `thoiDiemDo` | Có | Tool tính từ các chỉ số cuối cùng của chu kỳ | Là `thời điểm đo` muộn nhất trong các chỉ số đã chọn của gói, dùng để chống trùng |
| `maMay` | Có | Từ cấu hình Tool | Lưu kèm để truy vết |
| `chiSoSinhHieu` | Có | Từ các `OBX` hợp lệ trong cùng chu kỳ | Gồm danh sách chỉ số sau mapping; mỗi chỉ số lấy giá trị hợp lệ cuối cùng theo rule chu kỳ |
| `nguonDuLieu` | [Cần xác nhận] | Tool sinh | Ví dụ `PVM-2701`, `PVM-4761`, `SVM-7260` |
| `maGiaoDichTool` | [Cần xác nhận] | Tool sinh | Dùng để truy vết resend và log |

#### Đầu ra nghiệp vụ tối thiểu

| Trường | Ý nghĩa |
| --- | --- |
| `ketQua` | Thành công hoặc thất bại |
| `lyDo` | Mã lỗi hoặc mô tả lỗi nếu có |
| `daChongTrung` | [Cần xác nhận] nếu HIS có trả cờ này |

## 9. Mapping HL7 sang dữ liệu nghiệp vụ HIS

### 9.1. Mapping mức ngữ cảnh

| Nguồn HL7 | Tên nghiệp vụ | Bắt buộc | Cách dùng |
| --- | --- | --- | --- |
| `PID-3` | `Mã hồ sơ` | Có | Dùng để ghép đúng Người bệnh và gửi HIS |
| `OBR-7` | `Thời điểm đo` của nhóm chỉ số | Có | Nguồn thời gian mặc định để xác định chu kỳ và chống trùng |
| `OBX-14` | `Thời điểm đo` của chỉ số | Không | Nếu có gửi và cần dùng ở mức chỉ số thì ưu tiên theo rule detail bên dưới |
| `PV1-3` | Vị trí giường / thông tin thiết bị tại thời điểm đo | Không | Dùng cho đối soát hoặc log nếu cần |

### 9.2. Rule chọn thời điểm đo

| Điều kiện | Rule áp dụng |
| --- | --- |
| Bản tin có `OBR-7` | Dùng `OBR-7` làm `thời điểm đo` chuẩn cho các chỉ số trong bản tin để gom chu kỳ, chọn giá trị cuối cùng và chống trùng |
| `OBR-7` trống, nhưng có `OBX-14` | Dùng `OBX-14` làm fallback cho chỉ số tương ứng; nếu nhiều chỉ số được gom thì Tool lấy giá trị `OBX-14` muộn nhất trong các chỉ số cuối cùng để làm `thời điểm đo` gói |
| Có cả `OBR-7` và `OBX-14` | Ưu tiên `OBR-7` để tránh tách cùng một lần đo thành nhiều mốc thời gian khác nhau; `OBX-14` chỉ dùng cho log raw hoặc fallback khi `OBR-7` trống |
| Không có cả `OBR-7` và `OBX-14` | Xem là lỗi dữ liệu, không gửi HIS |

### 9.3. Mapping nhóm chỉ số từ bản tin mẫu

| `OBX-3` mẫu | Tên nghiệp vụ trên HIS | Đơn vị kỳ vọng | Ghi chú |
| --- | --- | --- | --- |
| `007001^VITAL PR(spo2)` | Mạch / PR | `/min` | Chỉ số mạch lấy từ nhóm SpO2 |
| `007000^VITAL SpO2` | SpO2 | `%` | Độ bão hòa oxy máu ngoại vi |
| `072007^VITAL rPR(spo2)` | Nhịp mạch tham chiếu | `/min` | Chưa mặc định gửi `SAKURA` ở Phase I nếu chưa có xác nhận bổ sung |
| `009000^NIBP SYS` | Huyết áp tâm thu | `mmHg` | Thuộc nhóm `Huyết áp`, nằm trong phạm vi Phase I |
| `009001^NIBP DIAS` | Huyết áp tâm trương | `mmHg` | Thuộc nhóm `Huyết áp`, nằm trong phạm vi Phase I |
| `009002^NIBP MEAN` | MAP / Huyết áp trung bình | `mmHg` | Chưa mặc định gửi `SAKURA` ở Phase I nếu chưa có xác nhận bổ sung |

### 9.4. Mapping chỉ số dự kiến cần hỗ trợ ở HIS

| Tên nghiệp vụ HIS | Nguồn HL7 | Trạng thái |
| --- | --- | --- |
| Mạch | `007001^VITAL PR(spo2)` | Nằm trong phạm vi Phase I |
| SpO2 | `007000^VITAL SpO2` | Nằm trong phạm vi Phase I |
| Nhịp thở | `[Cần điền mã OBX-3 thực tế]` | Nằm trong phạm vi Phase I nhưng cần chốt mã thật |
| Nhiệt độ | `[Cần điền mã OBX-3 thực tế]` | Nằm trong phạm vi Phase I nếu thiết bị hỗ trợ |
| Huyết áp tâm thu | `009000^NIBP SYS` | Nằm trong phạm vi Phase I |
| Huyết áp tâm trương | `009001^NIBP DIAS` | Nằm trong phạm vi Phase I |
| MAP | `009002^NIBP MEAN` | Chưa mặc định gửi `SAKURA` ở Phase I |
| PI | `[Cần điền mã OBX-3 thực tế]` | Chưa mặc định gửi `SAKURA` ở Phase I |

### 9.5. Bảng mapping chỉ số chi tiết để điền tiếp

| STT | Chỉ số HIS | Mã `OBX-3` | Tên HL7 / mô tả | `OBX-5` kiểu dữ liệu | `OBX-6` đơn vị | Có bắt buộc trong Phase I không | Rule lấy giá trị cuối cùng trong chu kỳ | Ghi chú |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Mạch | `007001^VITAL PR(spo2)` | `VITAL PR(spo2)` | `NM` | `/min` | Có | Lấy giá trị hợp lệ cuối cùng theo `thời điểm đo` chuẩn | Gửi `SAKURA` ở Phase I |
| 2 | SpO2 | `007000^VITAL SpO2` | `VITAL SpO2` | `NM` | `%` | Có | Lấy giá trị hợp lệ cuối cùng theo `thời điểm đo` chuẩn | Gửi `SAKURA` ở Phase I |
| 3 | Nhịp thở | `[Cần điền]` | `[Cần điền]` | `[Cần điền]` | `[Cần điền]` | Có | Lấy giá trị hợp lệ cuối cùng theo `thời điểm đo` chuẩn | Gửi `SAKURA` ở Phase I |
| 4 | Nhiệt độ | `[Cần điền]` | `[Cần điền]` | `[Cần điền]` | `[Cần điền]` | Có nếu thiết bị hỗ trợ | Lấy giá trị hợp lệ cuối cùng theo `thời điểm đo` chuẩn | Gửi `SAKURA` ở Phase I khi đã xác nhận |
| 5 | Huyết áp tâm thu | `009000^NIBP SYS` | `NIBP SYS` | `NM` | `mmHg` | Có | Lấy giá trị hợp lệ cuối cùng theo `thời điểm đo` chuẩn | Gửi `SAKURA` ở Phase I |
| 6 | Huyết áp tâm trương | `009001^NIBP DIAS` | `NIBP DIAS` | `NM` | `mmHg` | Có | Lấy giá trị hợp lệ cuối cùng theo `thời điểm đo` chuẩn | Gửi `SAKURA` ở Phase I |
| 7 | MAP | `009002^NIBP MEAN` | `NIBP MEAN` | `NM` | `mmHg` | Không mặc định | Lấy giá trị hợp lệ cuối cùng theo `thời điểm đo` chuẩn | Chỉ log hoặc chờ xác nhận thêm |
| 8 | PI | `[Cần điền]` | `[Cần điền]` | `[Cần điền]` | `[Cần điền]` | Không mặc định | Lấy giá trị hợp lệ cuối cùng theo `thời điểm đo` chuẩn | Chỉ log hoặc chờ xác nhận thêm |

Ghi chú:

- Chỉ map những mã `OBX-3` đã được chốt ở tài liệu mapping chi tiết.
- Nếu nhận mã `OBX-3` chưa map thì Tool ghi log kỹ thuật, `[Cần xác nhận]` có đưa vào bảng lỗi hay không.
- Trong Phase I, không tự đẩy `MAP`, `PI`, `rPR` sang `SAKURA` nếu chưa có xác nhận bổ sung.

## 10. Rule chu kỳ chi tiết

### 10.1. Định nghĩa chu kỳ

- Chu kỳ của một Người bệnh bắt đầu từ thời điểm `Find Patient` thành công.
- Phase I: mỗi chu kỳ dài `10 phút`.
- Ví dụ:
  - Chu kỳ 1: `T0` đến trước `T0 + 10 phút`.
  - Chu kỳ 2: từ `T0 + 10 phút` đến trước `T0 + 20 phút`.
- Mỗi lần `Find Patient` thành công đều mở một chu kỳ mới.
- Nếu phát sinh `Find Patient` mới trước khi đủ `10 phút` thì chu kỳ cũ đóng ngay tại thời điểm `Find Patient` mới, kể cả khi quét lại cùng `Mã hồ sơ`.

### 10.2. Điều kiện đóng chu kỳ

| Tình huống | Thời điểm đóng chu kỳ |
| --- | --- |
| Đủ `10 phút` kể từ đầu chu kỳ | Đóng tại mốc đủ `10 phút` |
| Có `Find Patient` mới cho Người bệnh tiếp theo | Đóng tại thời điểm `Find Patient` mới |
| Tool dừng hoặc lỗi hệ thống | `[Cần xác nhận]` cách khôi phục và chốt chu kỳ dang dở |

### 10.3. Rule chọn gói dữ liệu cuối cùng

| Tình huống | Rule |
| --- | --- |
| Chu kỳ chỉ có 1 bản tin đo | Gửi 1 gói dữ liệu của bản tin đó khi chu kỳ đóng |
| Chu kỳ có nhiều bản tin đo | Gom thành 1 gói dữ liệu gửi HIS khi chu kỳ đóng |
| Cùng 1 chỉ số xuất hiện nhiều lần với `thời điểm đo` khác nhau | Chọn giá trị có `thời điểm đo` chuẩn cuối cùng trước thời điểm đóng chu kỳ |
| Cùng 1 chỉ số xuất hiện nhiều lần với cùng `thời điểm đo` chuẩn | Chọn giá trị từ bản tin Tool nhận sau cùng |
| Chỉ số không phải giá trị cuối cùng của chu kỳ | Không gửi HIS, chỉ lưu log kỹ thuật |

### 10.4. Rule xử lý bản ghi đến trễ

| Tình huống | Rule |
| --- | --- |
| Bản ghi đến sau khi chu kỳ đã đóng nhưng `thời điểm đo` thuộc chu kỳ đó | Vẫn nhận vào đúng chu kỳ cũ |
| Bản ghi đến trễ không thay đổi bộ chỉ số cuối cùng của gói dữ liệu | Chỉ lưu log kỹ thuật |
| Bản ghi đến trễ làm thay đổi ít nhất 1 chỉ số cuối cùng hoặc làm đổi `thời điểm đo` gói | Đưa vào diện resend |
| Bản ghi đến trễ nhưng không xác định được `PID-3` hoặc `thời điểm đo` | Ghi lỗi dữ liệu, không gửi HIS |

## 11. Trạng thái dữ liệu và trạng thái giao dịch

| Trạng thái | Điều kiện vào trạng thái | Điều kiện ra khỏi trạng thái |
| --- | --- | --- |
| `Chờ gửi HIS` | Gói dữ liệu cuối cùng hợp lệ của chu kỳ đã được chọn | Gửi HIS thành công hoặc lỗi |
| `Đã gửi HIS` | HIS nhận gói dữ liệu sinh hiệu thành công, kể cả trường hợp HIS tự xử lý trùng bên trong | Nếu có bản ghi đến trễ làm thay đổi gói dữ liệu thì sinh giao dịch resend mới để cập nhật lại HIS |
| `Gửi HIS lỗi` | Gửi HIS không thành công hoặc lỗi dữ liệu cần theo dõi | Resend thành công hoặc đóng lỗi theo vận hành |

## 12. Message lỗi hoặc cảnh báo chính

| Mã | Tình huống | Message / cảnh báo |
| --- | --- | --- |
| MSG-00 | Loại bản tin `Find Patient` không đúng | `[Cần xác nhận] Bản tin tra cứu Người bệnh không đúng định dạng QRY^A19` |
| MSG-01 | `Mã hồ sơ` không tồn tại hoặc đã ra viện | `[Cần xác nhận] Không tìm thấy Người bệnh hợp lệ theo Mã hồ sơ` |
| MSG-02 | `Find Patient` thành công nhưng Monitor hiển thị thiếu trường bắt buộc | `[Cần xác nhận] Thông tin Người bệnh hiển thị chưa đủ để tiếp tục đo` |
| MSG-03 | Không có `PID-3` | `[Cần xác nhận] Không có Mã hồ sơ` |
| MSG-04 | Không có `OBR-7` và `OBX-14` | `[Cần xác nhận] Thiếu thời điểm đo` |
| MSG-05 | Lỗi kết nối gửi HIS sau 3 lần retry ở Phase I | `[Cần xác nhận] Gửi dữ liệu sinh hiệu sang HIS thất bại` |
| MSG-06 | Bản ghi đến trễ làm thay đổi gói dữ liệu cuối cùng | `[Cần xác nhận] Có bản ghi đến trễ cần resend để cập nhật HIS` |

## 13. Dữ liệu hoặc nghiệp vụ phát sinh

| Hạng mục | Mô tả |
| --- | --- |
| Bản ghi sinh hiệu | Lưu tại HIS theo `Mã hồ sơ`, `thời điểm đo`, `Mã máy` và danh sách chỉ số đã map |
| Log kỹ thuật | Lưu tại Tool cho `Find Patient`, parse HL7, rule chu kỳ, gửi HIS, retry, resend |
| Bảng lỗi | Lưu tại Tool cho lỗi gửi HIS, lỗi dữ liệu, dữ liệu trắng |
| Giao dịch resend | Phát sinh khi giao dịch lỗi cần gửi lại hoặc bản ghi đến trễ làm thay đổi gói dữ liệu cuối cùng |

## 13A. Yêu cầu log và truy vết vận hành tối thiểu

| Hạng mục log | Bắt buộc lưu | Mục đích |
| --- | --- | --- |
| `Find Patient` | `Mã hồ sơ`, thời điểm request, `messageControlId`, kết quả tra cứu, lý do lỗi nếu có | Truy vết ghép Người bệnh |
| Nhận bản tin `ORU^R01` | `PID-3`, `OBR-7`, `OBX-14`, danh sách `OBX-3` nhận được, thời điểm Tool nhận | Truy vết dữ liệu thô từ Monitor |
| Gom chu kỳ | `Mã hồ sơ`, mã chu kỳ, thời điểm bắt đầu, thời điểm đóng, các chỉ số cuối cùng được chọn, `thời điểm đo` gói | Giải thích vì sao HIS nhận bộ chỉ số nào |
| Gửi HIS | payload nghiệp vụ đã chuẩn hóa, kết quả phản hồi, số lần retry, lý do lỗi | Đối soát gửi nhận |
| Resend | người thao tác, thời điểm thao tác, mã giao dịch gốc, mã giao dịch resend, lý do resend, kết quả resend | Truy vết vận hành |
| Thay đổi cấu hình `Mã máy` / IP thiết bị | người thao tác, thời điểm thay đổi, giá trị cũ, giá trị mới | Truy vết cấu hình |

## 14. Ghi chú dữ liệu hoặc kỹ thuật

- Tên endpoint, phương thức gọi, cơ chế auth, timeout, idempotency key và contract kỹ thuật thật sẽ tách sang tài liệu API hoặc swagger nếu có.
- Tên trường kỹ thuật trên HIS và Tool chưa được khẳng định trong tài liệu này.
- Phase I thống nhất gửi 1 gói dữ liệu tổng hợp cho mỗi chu kỳ; Tool gom các chỉ số cuối cùng hợp lệ của chu kỳ rồi mới gửi sang HIS.
- Nếu giai đoạn sau phát sinh nhu cầu nhận từng chỉ số riêng lẻ thì phải tách tài liệu nghiệp vụ và chốt lại rule chống trùng theo từng chỉ số.

## 15. Báo cáo, in, xuất liên quan

- Không nằm trong phạm vi của tài liệu detail này.
- Nếu cần báo cáo đối soát số lần resend, số lỗi gửi HIS, số dữ liệu trắng thì tách sang dashboard vận hành. `[Cần xác nhận]`

## 16. Điều kiện chấp nhận

- AC-01: Khi Monitor gửi `ORU^R01`, ISOFHTool nhận và parse được bản tin ở mức tối thiểu.
- AC-02: Sau khi nhận bản tin, ISOFHTool phản hồi lại Monitor bằng `ACK` để Monitor tiếp tục duy trì kết nối và gửi các lượt đo tiếp theo nếu có.
- AC-03: Tool parse được `PID-3`, `OBR-7`, `OBX-3`, `OBX-5`, `OBX-6` từ bản tin HL7 mẫu.
- AC-04: Mỗi lần `Find Patient` thành công, Tool mở một chu kỳ mới; nếu đang có chu kỳ mở thì chu kỳ cũ đóng ngay tại thời điểm `Find Patient` mới.
- AC-05: Tool ưu tiên dùng `OBR-7` làm `thời điểm đo` chuẩn; nếu `OBR-7` trống thì dùng `OBX-14` fallback.
- AC-06: Trong cùng 1 chu kỳ, Tool gom nhiều bản tin `ORU^R01` thành 1 gói dữ liệu duy nhất gửi sang HIS.
- AC-07: Với từng chỉ số trong cùng chu kỳ, Tool chọn giá trị hợp lệ cuối cùng theo `thời điểm đo` chuẩn.
- AC-08: Nếu có nhiều giá trị của cùng 1 chỉ số có cùng `thời điểm đo` chuẩn trong cùng chu kỳ thì Tool chọn giá trị từ bản tin nhận sau cùng.
- AC-09: Tool chỉ gửi `05` nhóm chỉ số sống cơ bản của Phase I: `Mạch`, `Nhiệt độ`, `Huyết áp`, `Nhịp thở`, `SpO2`.
- AC-10: Nếu bản ghi đến trễ thuộc chu kỳ đã đóng thì Tool vẫn nhận theo `OBR-7` hoặc `OBX-14` của cùng `PID-3`.
- AC-11: Nếu bản ghi đến trễ làm thay đổi ít nhất 1 chỉ số cuối cùng hoặc làm đổi `thời điểm đo` gói thì Tool cho phép resend để cập nhật lại HIS.
- AC-12: Dữ liệu trắng không gửi sang HIS, nhưng phải lưu log kỹ thuật và bảng lỗi với `Lý do lỗi = Không có Mã hồ sơ`.
- AC-13: Phase I: lỗi kết nối retry tự động đủ `3 lần`; nếu vẫn lỗi thì đưa vào `Gửi HIS lỗi`.
- AC-14: Phase I: lỗi dữ liệu không retry tự động, phải vào `Gửi HIS lỗi` để theo dõi và resend thủ công.
- AC-15: Nếu sai `Mã máy` nhưng HIS vẫn nhận dữ liệu thì không đưa vào danh sách resend.
- AC-16: Tool phải lưu log tối thiểu cho nhận `ORU^R01`, phản hồi `ACK`, gom chu kỳ, gửi HIS, resend và thay đổi cấu hình `Mã máy` / IP thiết bị.

## 17. Trường hợp ngoại lệ

- EX-01: Bản tin HL7 thiếu `PID-3`.
- EX-02: Bản tin HL7 thiếu `OBR-7` và `OBX-14`.
- EX-03: Nhận mã `OBX-3` chưa có mapping.
- EX-04: Lỗi kết nối HIS khi gửi dữ liệu sau khi đóng chu kỳ.
- EX-05: Bản ghi đến trễ làm thay đổi gói dữ liệu cuối cùng của chu kỳ đã gửi thành công.
- EX-06: Monitor không nhận được `ACK` hoặc Tool không phản hồi `ACK` đúng thời điểm. `[Cần xác nhận]` chi tiết xử lý kỹ thuật |

## 18. Liên kết

- HLR: `2026-05-02_SRS_High level_Tich hop thiet bi PVM-2701 vao HIS.md`
- Nghiệp vụ Phase I: `2026-05-01_NV_Tich hop thiet bi PVM-2701 vao HIS - Phase I.md`
- Nghiệp vụ Phase II: `2026-05-02_NV_Tich hop thiet bi PVM-2701 vao HIS - Phase II.md`
- Q&A chốt nghiệp vụ: `2026-05-02_QA_Chot nghiep vu_Tich hop thiet bi PVM-2701 vao HIS.md`
- Danh sách ghép chỉ số sống: `2026-05-16_SRS_Detail_Danh sach ghep Chi so song SAKURA - Chi so song Monitor_Tich hop thiet bi PVM-2701 vao HIS.md`
- Danh sách thiết bị Monitor: `2026-05-16_SRS_Detail_Khai bao danh sach thiet bi Monitor_Tich hop thiet bi PVM-2701 vao HIS.md`
- Find Patient: `2026-05-16_SRS_Detail_Nhan yeu cau gui thong tin NB va tra thong tin cho Monitor_Tich hop thiet bi PVM-2701 vao HIS.md`

## 19. Điểm cần xác nhận trước khi triển khai

- Tên endpoint thật, method thật, cơ chế auth thật của API cập nhật sinh hiệu HIS.
- Xác nhận lại mapping kỹ thuật cuối cùng giữa cấu trúc `QRY^A19` từ Monitor và `ADR^A19` phản hồi cho Monitor nếu thiết bị thực tế có khác bản tin mẫu.
- Danh sách đầy đủ mã `OBX-3` cần map cho `Nhịp thở`, `Nhiệt độ`, `PI` và rule bắt buộc hoặc không bắt buộc theo từng chỉ số.
- Xác nhận lại `MAP` có nằm trong phạm vi gửi `SAKURA` của Phase I hay không.
- Tên trường kỹ thuật thật trên HIS để lưu `Mã máy`, `thời điểm đo`, `nguồn dữ liệu`, `mã giao dịch Tool`.
- Cách xử lý với mã `OBX-3` chưa map: chỉ log hay đưa vào bảng lỗi.
- Thời gian giữ log kỹ thuật và bảng lỗi trên ISOFHTool.
- Quy tắc API hoặc DB phía HIS khi nhận resend: update đè, upsert hay version hóa dữ liệu đã lưu. Hạng mục này do DEV chốt ở tài liệu kỹ thuật tích hợp.

## 20. Checklist BA trước khi chốt

- [x] Đã mô tả rõ phạm vi detail cho API, mapping HL7 và rule chu kỳ.
- [x] Đã tách business rule và validate dữ liệu.
- [x] Đã có luồng thành công và ngoại lệ chính.
- [x] Đã phản ánh xử lý bản ghi đến trễ, dữ liệu trắng, retry, resend.
- [x] Đã chỉ rõ phần nào là fact và phần nào còn cần xác nhận.
- [x] Tài liệu này đã đủ làm detail cho luồng nhận sinh hiệu khi đi cùng các note detail liên quan về `Find Patient`, `Danh sách ghép chỉ số sống` và `Danh sách thiết bị Monitor`.
