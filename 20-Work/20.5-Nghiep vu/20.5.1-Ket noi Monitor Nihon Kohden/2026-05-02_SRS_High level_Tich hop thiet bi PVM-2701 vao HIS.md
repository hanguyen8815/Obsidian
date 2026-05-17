# [HLR] Tích hợp thiết bị PVM-2701 vào HIS

| Thuộc tính | Nội dung |
| --- | --- |
| Trạng thái | YellowDraft |
| Người tạo | Hà Nguyễn |
| Ngày tạo | 02/05/2026 |
| Người review | Hà Nguyễn |
| Ngày review | 04/05/2026 |
| Người phê duyệt | Hà Nguyễn |
| Phân hệ | Sinh hiệu / Tích hợp thiết bị / Hồ sơ điều trị |
| Ticket / CR / Jira | [Cần xác nhận] |
| Cơ sở y tế áp dụng | Pilot tại MEDI Plus, định hướng dùng chung cho nhiều Cơ sở y tế |
| Mức áp dụng | Tích hợp + Cấu hình |
| Tài liệu detail liên quan | [Cần bổ sung sau] |

## Khi nào dùng template này

- Tài liệu này dùng để chốt mục tiêu, phạm vi, quy trình tổng quan, tác động và hướng xử lý ở mức sản phẩm cho bài toán tích hợp thiết bị PVM-2701 vào HIS.
- Tài liệu này không đi vào chi tiết màn hình, trường dữ liệu, API, kiểm tra dữ liệu nhỏ hoặc thông báo lỗi theo từng thao tác.

## 1. Tóm tắt nhanh

- Vấn đề hiện tại:
  - Quá trình đo sinh hiệu trên Monitor và ghi nhận vào HIS còn phụ thuộc nhập tay hoặc đối chiếu thủ công.
  - Dễ gây nhầm Người bệnh.
  - Khó truy vết thiết bị.
  - Mất thời gian khi xử lý lỗi gửi dữ liệu.
- Mục tiêu:
  - Kết nối Monitor PVM-2701 với HIS qua ISOFHTool.
  - Tra cứu đúng Người bệnh theo `Mã hồ sơ`.
  - Nhận dữ liệu sinh hiệu tự động.
  - Áp rule gom dữ liệu theo chu kỳ đo.
  - Ghi nhận dữ liệu vào HIS.
- Yêu cầu tiền điều kiện của thiết bị:
  - Monitor PVM-2701 phải hỗ trợ `QI-202P Interface`.
  - Monitor phải hỗ trợ gửi và nhận dữ liệu HL7 phục vụ luồng `Find Patient` và gửi dữ liệu sinh hiệu; tối thiểu gồm `QRY^A19`, `ADR^A19`, `ORU^R01`, `ACK`.
  - Monitor phải có thao tác `Find Patient` và `New Patient` để ghép đúng ngữ cảnh Người bệnh trước khi đo.
  - Monitor phải hiển thị tối thiểu `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính`, và `Tuổi` nếu thiết bị hỗ trợ hiển thị.
  - Thiết bị trong mô hình triển khai phải kết nối được qua IP với ISOFHTool và được đồng bộ giờ với Tool, HIS theo `GMT+7` trước khi chạy thật.
- Kết quả mong muốn:
  - Nhân viên y tế ghép đúng Người bệnh trên Monitor.
  - ISOFHTool nhận được dữ liệu đo.
  - HIS lưu được đúng dữ liệu sinh hiệu theo `Mã hồ sơ`.
  - Có log kỹ thuật.
  - Có theo dõi giao dịch gửi lỗi.
  - Có cơ chế resend khi lỗi kết nối.
  - Ở Phase II có Dashboard trên `SAKURA` để Điều dưỡng hoặc Bác sĩ theo dõi tổng quan Người bệnh thuộc phạm vi phụ trách.
- Giá trị mang lại:
  - Giảm nhập tay.
  - Giảm sai sót ghép nhầm Người bệnh.
  - Tăng khả năng truy vết.
  - Tăng đối soát vận hành.
  - Hỗ trợ phát hiện sớm tình trạng bất thường ở mức theo dõi tổng quan.
  - Tạo nền để mở rộng cho nhiều Cơ sở y tế.
- Quy trình dự phòng:
  - Khi HIS hoặc Tool không tra cứu được Người bệnh, Cơ sở y tế vận hành theo quy trình tay.
  - Đo trên Monitor rồi nhập tay vào HIS.

### Chỉ số thành công nếu có

| Chỉ số | Giá trị mục tiêu |
| --- | --- |
| Tỷ lệ ghép đúng Người bệnh trên Monitor theo quy trình vận hành | 100% |
| Tỷ lệ dữ liệu sinh hiệu lưu đúng vào HIS trong giai đoạn pilot | 95% |
| SLA xử lý lỗi gửi HIS khi đã có đủ thông tin | 15 - 30 phút |

## 2. Phạm vi

### 2.1. Trong phạm vi

- Kết nối Monitor PVM-2701 với HIS thông qua 1 ISOFHTool trung gian.
- Hỗ trợ quy trình Monitor gửi yêu cầu tìm Người bệnh theo `Mã hồ sơ`.
  - Tại bước này Monitor gửi bản tin HL7 `QRY^A19` đến ISOFHTool.
  - Tool chuyển tiếp sang HIS để tra cứu.
  - HIS phản hồi lại Monitor theo bản tin HL7 `ADR^A19`.
  - HIS chỉ trả đúng thông tin của lượt hoặc đợt điều trị có trạng thái `< Đã ra viện (100)`.
  - `Find Patient` là nút bấm bắt buộc trên Monitor để ghép đúng ngữ cảnh Người bệnh trước khi đo.
- Khi `Find Patient` thành công, Monitor phải hiển thị đầy đủ thông tin hành chính Người bệnh theo dữ liệu HIS trả về.
  - Tối thiểu gồm `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính`.
  - Hiển thị thêm `Tuổi` nếu thiết bị hỗ trợ.
- Hỗ trợ quy trình Monitor gửi dữ liệu sinh hiệu qua bản tin HL7 `ORU^R01`, Tool nhận dữ liệu, phản hồi bản tin HL7 `ACK` cho Monitor, áp rule chu kỳ và gửi sang HIS.
- Chu kỳ ghi nhận sinh hiệu được tính từ thời điểm `Find Patient`.
  - Phase I chia chu kỳ theo từng đợt `10 phút` tính từ thời điểm `Find Patient`; sau mỗi `10 phút` Tool đóng 1 đợt dữ liệu và chuẩn bị đợt kế tiếp.
  - Nếu phát sinh `Find Patient` mới khi chu kỳ `10 phút` hiện tại chưa kết thúc thì Tool đóng sớm đợt dữ liệu đang mở tại thời điểm `Find Patient` mới.
  - Trong cùng 1 chu kỳ, Tool gom nhiều bản tin `ORU^R01` thành 1 gói dữ liệu duy nhất để gửi sang HIS khi chu kỳ đóng.
    - Với từng chỉ số trong cùng 1 chu kỳ, Tool chọn giá trị hợp lệ cuối cùng theo `thời điểm đo` trong bản tin HL7; ưu tiên `OBR-7`, nếu `OBR-7` trống thì fallback `OBX-14`.
    - `thời điểm đo` của gói dữ liệu gửi HIS là `thời điểm đo` muộn nhất trong các chỉ số cuối cùng đã được chọn của chu kỳ.
- HIS chống trùng theo `Mã hồ sơ + thời điểm đo sinh hiệu`.
- Ghi log kỹ thuật, quản lý trạng thái giao dịch gửi HIS, retry hoặc resend theo từng phase và theo từng loại lỗi gửi HIS.
- Tách riêng màn hình nghiệp vụ xem dữ liệu sinh hiệu và màn hình vận hành kỹ thuật xem log, lỗi API, lỗi mapping, danh sách resend.
- Sử dụng `Danh mục Mã máy` hiện có trên HIS; HIS chỉ lưu `Mã máy` kèm bản ghi sinh hiệu.
- ISOFHTool cho phép cấu hình kết nối IP tới Monitor và nhập tay `Mã máy` tương ứng với thiết bị.
- Phase I:
  - Triển khai pilot với 1 máy tính đóng vai trò máy trung tâm và một số Monitor trong phạm vi giới hạn.
  - Pilot giai đoạn đầu tại MEDI Plus Nam Định với `05` thiết bị.
    - `01` thiết bị model `NIHON KOHDEN SVM-7260`.
    - `04` thiết bị model `NIHON KOHDEN PVM-4761`.
    - `PVM-4761` đã thử kết nối được theo cùng tài liệu của `PVM-2701`.
  - Một máy tính trung tâm trong pilot dự kiến phục vụ `05` thiết bị và được định hướng nhỏ hơn `10` Monitor; cần xác nhận lại giới hạn tải thực tế với MEDI Plus.
  - Phase I tập trung kết nối `05` chỉ số sống cơ bản.
    - `Mạch`.
    - `Nhiệt độ`.
      - Cần kiểm tra lại thiết bị pilot có hỗ trợ hay không.
    - `Huyết áp`.
    - `Nhịp thở`.
    - `SpO2`.
  - Chưa bắt buộc HIS trả chu kỳ ghi nhận sinh hiệu ngay trong API `Find Patient`.
  - Tool dùng chu kỳ mặc định `10 phút`.
  - Bắt buộc có màn hình lỗi và chức năng resend thủ công theo từng giao dịch lỗi trên ISOFHTool.
  - Dùng lại API `SAKURA` hiện có cho ISOFHTool.
    - API lấy thông tin Người bệnh: đã có.
    - API cập nhật `05` chỉ số sống cơ bản `Mạch`, `Nhiệt độ`, `Huyết áp`, `Nhịp thở`, `SpO2`: đã có.
    - Trong Phase I, phía BE không phải làm thêm API mới.
  - ISOFHTool cần xây dựng các hạng mục chính trong Phase I.
    - Danh sách ghép `Chỉ số sống SAKURA - Chỉ số sống Monitor`.
    - Danh mục cấu hình kết nối Monitor gồm `Mã máy`, `IP`, `PORT` của Monitor.
    - Luồng nhận request `QRY^A19` từ Monitor, gọi API `nbDotDieuTriId` của `SAKURA` để lấy thông tin Người bệnh, chỉ lấy các trường hợp có trạng thái `< Đã ra viện (100)`, sau đó phản hồi `ADR^A19` để Monitor hiển thị thông tin.
    - Luồng nhận bản tin `ORU^R01` từ Monitor và gửi phản hồi `ACK`.
    - Luồng tổng hợp dữ liệu HL7 thành JSON và gọi API cập nhật sinh hiệu vào `SAKURA`.
    - Quản lý log và resend khi có lỗi phát sinh.
- Phase II:
  - Bổ sung 1 thiết lập chung để cấu hình thời gian chu kỳ mặc định.
    - Nếu có khai báo chu kỳ riêng theo từng Người bệnh thì ưu tiên lấy theo khai báo của Người bệnh.
    - Nếu không có khai báo riêng thì lấy theo thiết lập chung.
  - Mở rộng thu thập thêm các dữ liệu khác ngoài `05` chỉ số sống cơ bản của Phase I.
  - Mở rộng cơ chế retry.
  - Theo dõi giao dịch gửi HIS lỗi.
  - Bổ sung màn hình vận hành để xử lý resend.
  - Xây dựng 1 Dashboard trên `SAKURA` để Điều dưỡng hoặc Bác sĩ theo dõi tình trạng tổng quan của các Người bệnh mình phụ trách.
  - Hỗ trợ người dùng xem nhanh có vấn đề bất thường hay không.

### 2.2. Ngoài phạm vi

- Dashboard phân tích lâm sàng chuyên sâu, hỗ trợ quyết định điều trị tự động hoặc cảnh báo lâm sàng nâng cao.
- Cảnh báo tự động cho tình huống quá thời gian không có dữ liệu ở giai đoạn đầu.
- Tự động ra y lệnh, tự động đổi phác đồ từ dữ liệu sinh hiệu.
- Tích hợp nhiều dòng Monitor khác nếu chưa khảo sát riêng.
- Kiến trúc nhiều server, dự phòng nhiều lớp hoặc mở rộng collector theo vùng mạng.
- Gọi API HIS để lấy danh sách `Mã máy` cho người dùng chọn trực tiếp trên Tool ở giai đoạn đầu.
- Chi tiết field, màn hình, API, thông báo lỗi và mapping kỹ thuật mức detail.
- Hành vi hiển thị lỗi chi tiết trên chính màn hình Monitor khi `Find Patient` không thành công hoặc Monitor vẫn giữ Người bệnh cũ; phần này thuộc phạm vi thiết bị và tài liệu hướng dẫn vận hành.

## 3. Hiện trạng đã biết

- Người dùng thao tác trên Monitor theo quy trình:
  - Kiểm tra có đang hiển thị Người bệnh cũ hay không.
  - Nếu có thì nhấn vào `Tên Người bệnh`.
  - Chọn `Admit`.
  - Chọn `New Patient`.
  - Quét barcode `Mã hồ sơ`.
  - Chọn `Find Patient`.
  - Đối chiếu đúng thông tin Người bệnh.
  - Chỉ thực hiện đo sinh hiệu sau khi đã đối chiếu đúng.
- `Find Patient` là chức năng bắt buộc trên Monitor để ghép đúng Người bệnh trước khi đo sinh hiệu.
- Quy trình ghép Người bệnh trên Monitor dùng `Mã hồ sơ` và chức năng `Find Patient`.
- Tại thời điểm chọn `Find Patient`:
  - Monitor gửi bản tin HL7 `QRY^A19` đến ISOFHTool.
  - ISOFHTool chuyển tiếp yêu cầu sang HIS.
  - ISOFHTool parse bản tin HL7 `ADR^A19` theo thông tin HIS trả về để phản hồi lại Monitor.
- `Mã hồ sơ` trên HIS đã được định nghĩa là duy nhất cho 1 lượt khám hoặc 1 lượt điều trị, tương ứng 1 lần `Patient Visit` trong phạm vi tích hợp này.
- HIS chỉ trả đúng thông tin Người bệnh theo `Mã hồ sơ` của lượt hoặc đợt điều trị có trạng thái `< Đã ra viện (100)`.
  - Nếu không có `Mã hồ sơ` phù hợp thì không trả thông tin.
  - Nếu `Mã hồ sơ` đã ra viện thì không trả thông tin để tránh đo vào hồ sơ cũ.
- Một Người bệnh không có hơn 1 `Mã hồ sơ` hợp lệ cùng lúc trong phạm vi tích hợp này.
- Khi Người bệnh chuyển khoa, chuyển giường hoặc đổi Bác sĩ điều trị thì `Mã hồ sơ` không đổi.
- HIS hiện đang chống trùng theo `Mã hồ sơ + thời điểm đo sinh hiệu`.
- Chu kỳ ghi nhận sinh hiệu được tính từ thời điểm `Find Patient`.
  - Phase I chia chu kỳ theo từng đợt `10 phút` tính từ thời điểm `Find Patient`; hết `10 phút` thì đóng 1 đợt dữ liệu và chuẩn bị đợt tiếp theo.
  - Nếu phát sinh `Find Patient` mới khi đợt `10 phút` hiện tại chưa kết thúc thì đợt cũ được đóng tại thời điểm `Find Patient` mới.
  - Nếu trong 1 chu kỳ chỉ có 1 bản tin đo thì gói dữ liệu của bản tin đó được gửi khi chu kỳ được đóng.
- Trên HIS đã có `Danh mục Mã máy`; phạm vi hiện tại là sử dụng danh mục sẵn có này để quản lý `Mã máy`, không tạo mới danh mục nghiệp vụ khác.
- Trên ISOFHTool, kết nối tới Monitor được cấu hình theo IP và có trường nhập `Mã máy` tương ứng với thiết bị để định danh khi gửi sang HIS.
- Nếu sai `Mã máy`:
  - HIS vẫn nhận và lưu dữ liệu sinh hiệu.
  - Phần chưa map đúng `Mã máy` được xử lý theo checklist cấu hình và đối soát vận hành.
  - Không yêu cầu resend dữ liệu.
  - Chưa cần sửa hồi cứu trên HIS ở giai đoạn triển khai ban đầu.
- Nếu không `Find Patient` được Người bệnh:
  - Điều dưỡng có thể chuyển `New Patient` trên Monitor để đo dữ liệu trắng.
  - Dữ liệu này không gắn Người bệnh trên HIS.
  - Xử lý theo quy trình tay.
- Dữ liệu trắng vẫn đi vào ISOFHTool nhưng chỉ lưu log kỹ thuật và bảng lỗi với `Lý do lỗi = Không có Mã hồ sơ`; không gửi sang HIS.
- Khi ISOFHTool nhận được bản tin HL7 `ORU^R01` từ Monitor thì phải phản hồi lại bản tin HL7 `ACK` để Monitor duy trì kết nối và tiếp tục gửi các bản tin tiếp theo nếu có.
- Giai đoạn đầu tại MEDI Plus Nam Định dự kiến triển khai `05` thiết bị.
  - `01` thiết bị model `NIHON KOHDEN SVM-7260`.
  - `04` thiết bị model `NIHON KOHDEN PVM-4761`.
  - `PVM-4761` đã thử kết nối được theo cùng tài liệu của `PVM-2701`.
- Một máy tính trung tâm trong pilot dự kiến phục vụ `05` thiết bị và được định hướng nhỏ hơn `10` Monitor; cần xác nhận lại với MEDI Plus.
- Phase I tập trung kết nối `05` chỉ số sống cơ bản.
  - `Mạch`.
  - `Nhiệt độ`.
    - Cần kiểm tra lại thiết bị pilot có hỗ trợ hay không.
  - `Huyết áp`.
  - `Nhịp thở`.
  - `SpO2`.
- Phase II sẽ mở rộng thu thập thêm các dữ liệu khác và bổ sung sau.
- Bản ghi đến trễ được xử lý theo `thời điểm đo` trong bản tin HL7.
  - Ưu tiên `OBR-7`.
  - Nếu `OBR-7` trống thì fallback `OBX-14`.
  - Tool so theo cùng `Mã hồ sơ` tại `PID-3`.
  - Vẫn nhận vào chu kỳ đã đóng và cho phép resend nếu cần.
- Giai đoạn đầu chưa đặt giới hạn thời gian hiệu lực cho một lần ghép Người bệnh trên Monitor vì có tình huống Người bệnh phải theo dõi Monitor thời gian dài.
- Giai đoạn đầu chưa xử lý cảnh báo tự động khi quá thời gian không có dữ liệu; phần này sẽ đánh giá sau triển khai.

## 4. Fact / Assumption / Constraint / Open question

| Loại | Nội dung | Nguồn / Người xác nhận | Trạng thái |
| --- | --- | --- | --- |
| Fact | Monitor PVM-2701 được định hướng kết nối với HIS thông qua 1 ISOFHTool trung gian. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | Quy trình ghép Người bệnh trên Monitor dùng `Mã hồ sơ` và chức năng `Find Patient`. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | Khi ISOFHTool nhận được bản tin HL7 `ORU^R01` từ Monitor thì phải phản hồi lại bản tin HL7 `ACK` để Monitor duy trì kết nối và tiếp tục gửi các bản tin tiếp theo nếu có. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | `Find Patient` là nút bấm bắt buộc trên Monitor khi thực hiện ghép ngữ cảnh Người bệnh trước khi đo. | Hà Nguyễn xác nhận 03/05/2026 | Đã rõ |
| Fact | Tại bước `Find Patient`, Monitor gửi bản tin HL7 `QRY^A19` đến ISOFHTool; ISOFHTool chuyển tiếp sang HIS để tra cứu và parse bản tin HL7 `ADR^A19` theo dữ liệu HIS trả về để phản hồi lại Monitor. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | Khi `Find Patient` thành công, Monitor phải hiển thị đầy đủ thông tin hành chính Người bệnh theo dữ liệu HIS trả về; tối thiểu gồm `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính`, và `Tuổi` nếu thiết bị hỗ trợ hiển thị. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | `Mã hồ sơ` tương ứng duy nhất cho 1 lượt khám hoặc 1 lượt điều trị trong phạm vi tích hợp. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Fact | HIS chỉ trả đúng thông tin theo `Mã hồ sơ` của lượt hoặc đợt điều trị có trạng thái `< Đã ra viện (100)`; nếu không có hoặc `Mã hồ sơ` đã ra viện thì không trả thông tin để tránh đo vào hồ sơ cũ. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | Một Người bệnh không có hơn 1 `Mã hồ sơ` hợp lệ cùng lúc trong phạm vi tích hợp này. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Fact | Khi chuyển khoa, chuyển giường hoặc đổi Bác sĩ điều trị thì `Mã hồ sơ` không đổi. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Fact | Chu kỳ ghi nhận sinh hiệu được tính từ thời điểm `Find Patient`. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Fact | Phase I chia chu kỳ theo từng đợt `10 phút` tính từ thời điểm `Find Patient`; hết `10 phút` thì đóng 1 đợt dữ liệu và chuẩn bị đợt kế tiếp. | Hà Nguyễn xác nhận 03/05/2026 | Đã rõ |
| Fact | Nếu phát sinh `Find Patient` mới khi chu kỳ hiện tại chưa kết thúc thì Tool đóng sớm đợt dữ liệu đang mở tại thời điểm `Find Patient` mới. | Hà Nguyễn xác nhận 03/05/2026 | Đã rõ |
| Fact | Trong cùng 1 chu kỳ, Tool gom nhiều bản tin `ORU^R01` thành 1 gói dữ liệu duy nhất để gửi sang HIS. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | Với từng chỉ số trong cùng 1 chu kỳ, Tool chọn giá trị hợp lệ cuối cùng theo `thời điểm đo` trong bản tin HL7; ưu tiên `OBR-7`, nếu `OBR-7` trống thì fallback `OBX-14`. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | Nếu trong 1 chu kỳ chỉ có 1 bản tin đo thì gói dữ liệu của bản tin đó được gửi khi chu kỳ được đóng. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | HIS chống trùng theo `Mã hồ sơ + thời điểm đo sinh hiệu`. | Note Phase I, Phase II, Q&A | Đã rõ |
| Fact | Mốc thời gian chuẩn để chống trùng là thời gian đo sinh hiệu, không phải thời gian HIS nhận bản tin. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Fact | Nếu HIS báo trùng dữ liệu thì trạng thái giao dịch tại Tool vẫn là `Đã gửi HIS`; HIS tự xử lý phần trùng. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Fact | Nếu sai `Mã máy`, HIS vẫn nhận và lưu dữ liệu; không yêu cầu resend, cần đối soát và mapping lại sau cấu hình. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Fact | `Lý do lỗi` trước mắt chỉ cần lưu tại ISOFHTool. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Fact | Phase I pilot chưa bắt buộc HIS trả chu kỳ ghi nhận sinh hiệu ngay trong API `Find Patient`; Tool dùng chu kỳ mặc định `10 phút`. | Hà Nguyễn xác nhận 03/05/2026 | Đã rõ |
| Fact | Phase I bắt buộc có màn hình lỗi và chức năng resend thủ công theo từng giao dịch lỗi trên ISOFHTool. | Hà Nguyễn xác nhận 03/05/2026 | Đã rõ |
| Fact | Phase II bổ sung thiết lập chung để cấu hình chu kỳ mặc định; nếu Người bệnh có khai báo chu kỳ riêng thì ưu tiên lấy theo khai báo của Người bệnh, nếu không có thì lấy theo thiết lập chung. | Hà Nguyễn xác nhận 03/05/2026 | Đã rõ |
| Fact | Phase II bổ sung Dashboard trên `SAKURA` để Điều dưỡng hoặc Bác sĩ theo dõi tổng quan các Người bệnh thuộc phạm vi phụ trách và nhận biết nhanh dấu hiệu bất thường theo rule hiển thị. | Hà Nguyễn cập nhật 06/05/2026 | Đã rõ |
| Fact | Với lỗi dữ liệu gửi HIS, Phase I ghi log lại để theo dõi và resend sau; Phase II hỗ trợ tự động retry gửi lại 3 lần hoặc theo số lần trong thiết lập, nếu quá ngưỡng thì tiếp tục log để theo dõi. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Fact | Resend theo lô không phải tiêu chí bắt buộc để nghiệm thu Phase I; nếu chưa đủ effort thì chuyển sang Phase II. | Hà Nguyễn xác nhận 03/05/2026 | Đã rõ |
| Fact | Dữ liệu sinh hiệu nghiệp vụ được xem trên HIS; log kỹ thuật và lỗi vận hành được xem ở màn hình kỹ thuật riêng. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Fact | Dữ liệu trắng vẫn đi vào ISOFHTool nhưng chỉ lưu log kỹ thuật và bảng lỗi với `Lý do lỗi = Không có Mã hồ sơ`; không gửi sang HIS. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | Bản ghi đến trễ được xử lý theo `thời điểm đo` trong bản tin HL7; ưu tiên `OBR-7`, nếu `OBR-7` trống thì fallback `OBX-14`; Tool so theo cùng `Mã hồ sơ` ở `PID-3`, vẫn nhận vào chu kỳ đã đóng và cho phép resend nếu cần. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | Phase I chưa làm user-level permission trên ISOFHTool; ai truy cập được ISOFHTool trên máy server dùng riêng thì đều xem được theo phạm vi vận hành hiện tại. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | Giai đoạn đầu tại MEDI Plus Nam Định dự kiến triển khai `05` thiết bị gồm `01` thiết bị `NIHON KOHDEN SVM-7260` và `04` thiết bị `NIHON KOHDEN PVM-4761`; `PVM-4761` đã thử kết nối được theo cùng tài liệu của `PVM-2701`. | Hà Nguyễn xác nhận 04/05/2026 | Đã rõ |
| Fact | Phase I tập trung kết nối `05` chỉ số sống cơ bản gồm `Mạch`, `Nhiệt độ`, `Huyết áp`, `Nhịp thở`, `SpO2`; Phase II sẽ mở rộng thêm các dữ liệu khác. | Hà Nguyễn cập nhật 16/05/2026 | Đã rõ |
| Fact | Phase I dùng lại API `SAKURA` hiện có để ISOFHTool lấy thông tin Người bệnh và cập nhật `05` chỉ số sống cơ bản; phía BE không phải làm thêm API mới trong phase này. | Hà Nguyễn cập nhật 16/05/2026 | Đã rõ |
| Fact | Bắt buộc đồng bộ giờ giữa Monitor, Tool và HIS theo GMT+7 trước khi chạy thật. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Fact | Kênh cảnh báo vận hành hiện tại là log nội bộ; chưa triển khai cảnh báo ngoài màn hình, email hay Zalo ở giai đoạn đầu. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Fact | Nếu không `Find Patient` được Người bệnh thì Điều dưỡng có thể chuyển `New Patient` trên Monitor để đo dữ liệu trắng; dữ liệu này không gắn Người bệnh trên HIS và xử lý theo quy trình tay. | Hà Nguyễn xác nhận 03/05/2026 | Đã rõ |
| Constraint | Giai đoạn đầu phải ưu tiên mô hình có thể pilot nhanh, hạn chế thay đổi lớn về hạ tầng. | Note Phase I | Đã rõ |
| Constraint | Không được làm mất dữ liệu đã nhận từ Monitor khi HIS lỗi tạm thời. | Note Phase I, Phase II | Đã rõ |
| Constraint | Dữ liệu Người bệnh và dữ liệu sinh hiệu phải được ghi nhận đầy đủ, đúng ngữ cảnh `Mã hồ sơ`. | Hà Nguyễn xác nhận 02/05/2026 | Đã rõ |
| Assumption | Một máy tính trung tâm trong pilot dự kiến phục vụ `05` thiết bị và được định hướng nhỏ hơn `10` Monitor; cần xác nhận lại với MEDI Plus. | Hà Nguyễn xác nhận 04/05/2026 | Cần xác nhận |
| Assumption | Khi máy trung tâm dừng hoạt động, Cơ sở y tế sẽ vận hành song song ghi giấy; cần xác nhận thêm khả năng lưu dữ liệu của Monitor để hoàn thiện phương án dự phòng. | Hà Nguyễn xác nhận 02/05/2026 | Cần xác nhận |
| Fact | Tài liệu này đủ rõ để tách tiếp xuống detail level cho API, mapping, rule chu kỳ, rule resend, phân quyền và vận hành pilot. | Tổng hợp từ mục 15 và mục 16 | Đã rõ |
| Open question | Ticket / CR / Jira liên quan là gì. | BA / PO | Mở |
| Open question | Thiết bị pilot có hỗ trợ chỉ số `Nhiệt độ` để đưa vào phạm vi `05` chỉ số Phase I hay không. | Hà Nguyễn cập nhật 16/05/2026 | Mở |
| Open question | Thời gian giữ log kỹ thuật và bảng lỗi của dữ liệu trắng hoặc bản ghi lỗi trên ISOFHTool là bao lâu. | BA / Triển khai / CNTT | Mở |

## 5. Nhóm dùng và bên liên quan

| Nhóm | Vai trò trong quy trình | Mức tham gia | Bị tác động |
| --- | --- | --- | --- |
| Điều dưỡng | Quét `Mã hồ sơ`, đối chiếu Người bệnh, đo sinh hiệu, xem dữ liệu sinh hiệu nghiệp vụ, theo dõi Dashboard tổng quan trên `SAKURA` theo phạm vi phụ trách | Chính | Có |
| Bác sĩ | Xem dữ liệu sinh hiệu đã ghi nhận trong HIS, theo dõi Dashboard tổng quan trên `SAKURA` theo phạm vi phụ trách | Phụ | Có |
| Quản lý Cơ sở y tế | Xem dữ liệu sinh hiệu nghiệp vụ theo quyền được phân | Phụ | Có |
| CNTT Cơ sở y tế | Cấu hình Tool, theo dõi log kỹ thuật, theo dõi lỗi gửi HIS, phối hợp resend và mapping `Mã máy` | Chính ở vận hành | Có |
| Triển khai ISOFH / Hỗ trợ | Hỗ trợ go-live, cấu hình ban đầu, theo dõi lỗi hằng ngày thời gian đầu, phối hợp resend | Chính ở giai đoạn đầu | Có |
| DEV / QA | Xây dựng, kiểm tra API, log, xử lý lỗi kỹ thuật, hỗ trợ điều tra nguyên nhân gốc khi dữ liệu lỗi | Phụ | Có |
| Đơn vị hỗ trợ thiết bị | Xác nhận khả năng giao tiếp Monitor và xử lý lỗi phía thiết bị | Phụ | Có |

## 6. Phân loại yêu cầu theo định hướng sản phẩm

| Hạng mục | Phân loại | Lý do | Ảnh hưởng khách hàng khác |
| --- | --- | --- | --- |
| Ghép Người bệnh trên Monitor qua `Mã hồ sơ` và `Find Patient` | Tích hợp | Phụ thuộc giao tiếp giữa Monitor, Tool và HIS | Có |
| Tool nhận dữ liệu sinh hiệu, quản lý trạng thái, retry, resend | Tích hợp | Là lớp trung gian xử lý kết nối và vận hành | Có |
| Chống trùng theo `Mã hồ sơ + thời điểm đo sinh hiệu` | Lõi dùng chung | Là rule nền để tránh lặp dữ liệu trong HIS | Có |
| Chu kỳ ghi nhận sinh hiệu | Lõi dùng chung + Cấu hình + Tích hợp | Phase I dùng chu kỳ mặc định `10 phút`; từ Phase II hỗ trợ thiết lập chung và ưu tiên khai báo riêng theo từng Người bệnh | Có |
| Cấu hình kết nối IP thiết bị và `Mã máy` trên ISOFHTool | Cấu hình | Mỗi Cơ sở y tế cần khai báo theo thiết bị thực tế | Có |
| Lưu `Mã máy` kèm bản ghi sinh hiệu trên HIS | Lõi dùng chung | Nhu cầu truy vết thiết bị có giá trị cho nhiều Cơ sở y tế | Có |
| Màn hình nghiệp vụ và màn hình vận hành kỹ thuật tách riêng | Lõi dùng chung | Tách đúng nhóm người dùng và tránh lộ log kỹ thuật cho người dùng nghiệp vụ | Có |
| Dashboard tổng quan Người bệnh phụ trách trên `SAKURA` | Lõi dùng chung + Cấu hình | Hỗ trợ Điều dưỡng hoặc Bác sĩ theo dõi nhanh danh sách Người bệnh thuộc phạm vi phụ trách và nhận biết bất thường theo rule hiển thị; chi tiết cách xác định bất thường và phạm vi phụ trách cần cấu hình hoặc chốt ở tài liệu detail | Có |
| Retry / resend cho giao dịch gửi HIS lỗi | Lõi dùng chung + Cấu hình | Phase I bắt buộc resend thủ công theo từng giao dịch lỗi; Phase II thực hiện retry và resend theo `Thiết lập chung` trên HIS | Có |
| Kiến trúc 1 máy trung tâm cho pilot | Đặc thù | Mang tính giai đoạn, có thể thay đổi khi mở rộng | Có |

## 7. Quy trình tổng quan / Use case tổng quan

### 7.1. Danh sách use case

| Mã | Tên use case | Trigger | Người thao tác chính | Kết quả đầu ra |
| --- | --- | --- | --- | --- |
| UC-01 | Cấu hình kết nối Monitor và `Mã máy` trên ISOFHTool | Cấu hình ban đầu hoặc cập nhật vận hành pilot | CNTT / Triển khai ISOFH | Tool kết nối đúng Monitor, nhận dữ liệu đúng kênh và gắn đúng `Mã máy` khi gửi HIS |
| UC-02 | Chuyển Monitor sang ngữ cảnh Người bệnh mới | Monitor đang hiển thị Người bệnh cũ trước khi quét barcode `Mã hồ sơ` của Người bệnh mới | Điều dưỡng / Nhân viên y tế | Monitor sẵn sàng ghép đúng Người bệnh mới trước khi đo |
| UC-03 | Tìm và ghép đúng Người bệnh bằng `Mã hồ sơ` | Quét barcode `Mã hồ sơ` và chọn `Find Patient` | Điều dưỡng / Nhân viên y tế | Monitor hiển thị đúng thông tin hành chính tối thiểu của Người bệnh hợp lệ để đối chiếu trước khi đo |
| UC-04 | Từ chối ghép Người bệnh không hợp lệ hoặc hiển thị thiếu thông tin | `Mã hồ sơ` không tồn tại, đã ra viện, hoặc `Find Patient` thành công nhưng Monitor không hiển thị đủ thông tin tối thiểu | HIS / ISOFHTool / Monitor | Không đo theo luồng tích hợp, không ghi dữ liệu sang HIS và chuyển sang hướng xử lý vận hành phù hợp |
| UC-05 | Nhận dữ liệu đo sinh hiệu và phản hồi `ACK` cho Monitor | Hoàn tất đo sinh hiệu trên Monitor sau khi đã ghép Người bệnh | ISOFHTool | Tool nhận bản tin đo, phản hồi `ACK`, lưu log kỹ thuật và chuẩn bị xử lý theo chu kỳ |
| UC-06 | Gom dữ liệu theo chu kỳ Phase I và gửi gói cuối sang HIS | Chu kỳ `10 phút` kết thúc hoặc phát sinh `Find Patient` mới | ISOFHTool | HIS nhận 1 gói dữ liệu cuối cùng của chu kỳ theo đúng `Mã hồ sơ`, `thời điểm đo` và `Mã máy` |
| UC-07 | Xử lý bản ghi đến trễ của chu kỳ đã đóng | Tool nhận bản ghi có `thời điểm đo` thuộc chu kỳ đã đóng | ISOFHTool | Bản ghi được gán đúng chu kỳ đã đóng và cho phép resend nếu làm thay đổi gói dữ liệu cuối cùng |
| UC-08 | Theo dõi log kỹ thuật và giao dịch gửi HIS lỗi | Phát sinh lỗi gửi HIS, dữ liệu trắng hoặc cần đối soát kỹ thuật | CNTT / Triển khai ISOFH / DEV / QA | Có log kỹ thuật, danh sách lỗi và `Lý do lỗi` để tra cứu |
| UC-09 | Retry và resend thủ công giao dịch gửi HIS lỗi | Gói dữ liệu gửi HIS lỗi do lỗi kết nối hoặc lỗi dữ liệu | CNTT / Triển khai ISOFH | Giao dịch được retry hoặc resend theo rule Phase I và cập nhật lại trạng thái xử lý |
| UC-10 | Ghi nhận dữ liệu trắng khi không `Find Patient` được Người bệnh | Không tra cứu được Người bệnh nhưng vẫn cần đo theo quy trình tay | Điều dưỡng / ISOFHTool | Dữ liệu trắng chỉ lưu ở Tool với `Lý do lỗi = Không có Mã hồ sơ`, không gửi sang HIS |
| UC-11 | Xem dữ liệu sinh hiệu nghiệp vụ trên HIS | Người dùng mở màn hình nghiệp vụ sau khi dữ liệu đã gửi thành công | Điều dưỡng / Bác sĩ / Quản lý Cơ sở y tế | Dữ liệu sinh hiệu hiển thị đúng theo `Mã hồ sơ` và quyền được phân |
| UC-12 | Xem Dashboard tổng quan Người bệnh phụ trách trên `SAKURA` | Người dùng mở Dashboard theo dõi sau khi dữ liệu sinh hiệu đã được ghi nhận hoặc tổng hợp trạng thái | Điều dưỡng / Bác sĩ | Hiển thị danh sách Người bệnh thuộc phạm vi phụ trách, trạng thái tổng quan, thời điểm đo gần nhất và dấu hiệu bất thường theo rule hiển thị |

### 7.2. Luồng chính

#### Lược đồ workflow

```text
[Điều dưỡng kiểm tra Monitor có đang hiển thị NB cũ hay không]
                |
        +-------+-------+
        |               |
        v               v
[Có NB cũ]         [Chưa có NB cũ]
        |               |
        v               v
[Nhấn Tên NB -> Admit -> New Patient] [Chuyển sang Bước 4]
                |               |
                +-------+-------+
                        |
                        v
[Bước 4. Quét Barcode Mã hồ sơ]
                |
                v
[Chọn Find Patient trên Monitor]
                |
                v
[Monitor gửi yêu cầu Find Patient tới Tool]
                |
                v
[Tool gọi HIS tra cứu Người bệnh]
                |
                v
[HIS trả thông tin Người bệnh]
                |
                v
[Monitor hiển thị thông tin NB để đối chiếu]
                |
                v
[Điều dưỡng xác nhận đúng NB và đo sinh hiệu]
                |
                v
[Monitor gửi ORU^R01 tới Tool]
                |
                v
[Tool phản hồi ACK cho Monitor]
                |
                v
[Tool lưu log, gom dữ liệu trong chu kỳ tính từ Find Patient]
                |
        +-------+-------+
        |               |
        v               v
[Chưa đủ điều kiện đóng]   [Chu kỳ đóng]
[gói dữ liệu của chu kỳ]      |
        |               v
        v      [Gán trạng thái Chờ gửi HIS]
[Lưu log kỹ thuật]            |
                              v
                  [Tool gửi dữ liệu sang HIS kèm Mã máy]
                              |
                      +-------+-------+
                      |               |
                      v               v
            [HIS nhận thành công] [Lỗi kết nối / lỗi gửi]
                      |               |
                      v               v
    [Cập nhật trạng thái Đã gửi HIS] [Cập nhật trạng thái Gửi HIS lỗi]
                                              |
                                              v
                               [Đưa vào danh sách lỗi gửi HIS]
                                              |
                                              v
                            [CNTT / Triển khai theo dõi và resend]
```

#### Workflow tổng quan

| Bước | Tác nhân / Hệ thống | Hành động | Kết quả |
| --- | --- | --- | --- |
| 1 | Điều dưỡng | Kiểm tra trên Monitor có đang hiển thị thông tin Người bệnh đang đo hay không | Xác định cần chuyển ngữ cảnh sang Người bệnh mới hay không |
| 2 | Điều dưỡng / Monitor | Nếu đang có thông tin Người bệnh cũ trên Monitor thì nhấn vào `Tên Người bệnh`, chọn `Admit`, sau đó chọn `New Patient` | Monitor chuyển sang ngữ cảnh tiếp nhận Người bệnh mới |
| 3 | Điều dưỡng | Nếu chưa có thông tin Người bệnh cũ trên Monitor thì chuyển sang Bước 4 | Giữ nguyên ngữ cảnh để quét `Mã hồ sơ` |
| 4 | Điều dưỡng | Dùng đầu đọc Barcode quét Barcode `Mã hồ sơ` của Người bệnh | Monitor nhận `Mã hồ sơ` cần tra cứu |
| 5 | Điều dưỡng | Chọn `Find Patient` trên Monitor | Monitor phát sinh yêu cầu tra cứu Người bệnh |
| 6 | Monitor | Gửi bản tin HL7 `QRY^A19` sang ISOFHTool | Tool nhận yêu cầu `Find Patient` |
| 7 | ISOFHTool | Chuyển tiếp yêu cầu tra cứu sang HIS theo `Mã hồ sơ` | HIS xử lý yêu cầu tra cứu |
| 8 | HIS | Trả thông tin hành chính Người bệnh cho ISOFHTool | Tool nhận kết quả tra cứu |
| 9 | ISOFHTool -> Monitor | Parse bản tin HL7 `ADR^A19` từ dữ liệu HIS trả về và gửi lại Monitor | Monitor hiển thị đầy đủ thông tin hành chính theo dữ liệu HIS; tối thiểu `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính`, và `Tuổi` nếu thiết bị hỗ trợ |
| 10 | Điều dưỡng | Đối chiếu đúng Người bệnh và thực hiện đo sinh hiệu | Monitor phát sinh dữ liệu sinh hiệu |
| 11 | Monitor | Gửi dữ liệu sinh hiệu sang ISOFHTool bằng bản tin HL7 `ORU^R01` | Tool nhận bản ghi đo |
| 11a | ISOFHTool -> Monitor | Gửi phản hồi bản tin HL7 `ACK` đã nhận dữ liệu | Monitor duy trì kết nối và tiếp tục gửi các lượt đo tiếp theo nếu có |
| 12 | ISOFHTool | Lưu log, gom các bản tin trong chu kỳ tính từ thời điểm `Find Patient`, theo dõi cửa sổ `10 phút` và xác định thời điểm đóng chu kỳ | Phase I dùng chu kỳ mặc định `10 phút`; chu kỳ đóng khi đủ `10 phút` hoặc khi phát sinh `Find Patient` mới |
| 13a | ISOFHTool | Nếu chu kỳ chưa đóng thì tiếp tục nhận dữ liệu, gom các chỉ số theo rule và chỉ lưu log kỹ thuật cho dữ liệu trung gian | Gói dữ liệu chưa gửi sang HIS |
| 13b | ISOFHTool | Khi chu kỳ đóng, Tool tạo gói dữ liệu cuối cùng của chu kỳ; nếu có dữ liệu thì gán trạng thái `Chờ gửi HIS` và gửi sang HIS kèm `Mã máy` | HIS nhận gói dữ liệu sinh hiệu của chu kỳ |
| 14a | HIS | Nhận thành công gói dữ liệu sinh hiệu | Tool cập nhật trạng thái `Đã gửi HIS` |
| 14b | ISOFHTool / HIS | Nếu lỗi kết nối, lỗi kỹ thuật hoặc lỗi dữ liệu khi gửi sang HIS | Tool cập nhật trạng thái `Gửi HIS lỗi`, lưu `Lý do lỗi` tại Tool và đưa vào danh sách lỗi gửi HIS |
| 15 | CNTT / Triển khai ISOFH | Theo dõi danh sách lỗi gửi HIS, kiểm tra log và thực hiện resend khi cần | Bản ghi lỗi được đối soát và xử lý |
| 16 | `SAKURA` / Điều dưỡng / Bác sĩ | Tổng hợp dữ liệu sinh hiệu đã ghi nhận và hiển thị Dashboard theo phạm vi Người bệnh phụ trách | Người dùng theo dõi được tổng quan, thấy dấu hiệu bất thường theo rule hiển thị và quyết định mở hồ sơ chi tiết khi cần |

1. Kiểm tra xem trên Monitor có thông tin Người bệnh đang đo hay không.
2. Nếu đang có thông tin Người bệnh cũ trên Monitor thì nhấn vào `Tên Người bệnh`. Hệ thống hiển thị popup, chọn `Admit`, sau đó chọn `New Patient` để chuyển sang ngữ cảnh Người bệnh mới.
3. Nếu chưa có thông tin Người bệnh cũ trên Monitor thì chuyển sang Bước 4.
4. Dùng đầu đọc Barcode quét Barcode `Mã hồ sơ` của Người bệnh.
5. Chọn `Find Patient` để lấy thông tin Người bệnh tương ứng từ HIS.
6. Monitor gửi yêu cầu sang ISOFHTool.
   - Tool gọi HIS để lấy thông tin Người bệnh theo `Mã hồ sơ`.
7. HIS trả thông tin hành chính Người bệnh cho Tool.
   - Tool parse bản tin HL7 `ADR^A19` theo dữ liệu HIS trả về.
   - Tool chuyển kết quả về Monitor để đối chiếu.
   - Khi `Find Patient` thành công, Monitor phải hiển thị đầy đủ thông tin hành chính theo dữ liệu nhận được từ HIS.
     - Tối thiểu gồm `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính`.
     - Hiển thị thêm `Tuổi` nếu thiết bị hỗ trợ.
8. Nhân viên y tế xác nhận đúng Người bệnh trước khi đo.
9. Nhân viên y tế thực hiện đo sinh hiệu; Monitor gửi dữ liệu sang ISOFHTool bằng bản tin HL7 `ORU^R01` sau khi đo xong.
10. Sau khi nhận bản tin `ORU^R01`, ISOFHTool gửi phản hồi bản tin HL7 `ACK` để Monitor duy trì kết nối và tiếp tục gửi các lượt đo tiếp theo nếu có.
11. Tool lưu log kỹ thuật và gom dữ liệu theo chu kỳ tính từ thời điểm `Find Patient`.
    - Ở Phase I, chu kỳ mặc định là `10 phút`.
    - Chu kỳ được chia theo từng đợt `Find Patient + 10 phút`, `+20 phút`, `+30 phút`...
    - Nếu phát sinh `Find Patient` mới khi chưa hết `10 phút` thì chu kỳ đang mở được đóng ngay tại thời điểm `Find Patient` mới.
    - Từ Phase II, chu kỳ mặc định lấy theo thiết lập chung.
      - Nếu Người bệnh có khai báo riêng thì ưu tiên theo khai báo riêng.
12. Trong thời gian chu kỳ còn mở, Tool tiếp tục nhận nhiều bản tin `ORU^R01`, lưu log kỹ thuật và gom dữ liệu theo từng chỉ số; các dữ liệu trung gian chưa gửi sang HIS.
13. Khi chu kỳ được đóng, nếu chu kỳ có dữ liệu thì Tool tạo 1 gói dữ liệu cuối cùng của chu kỳ để gửi sang HIS.
    - Với từng chỉ số, Tool chọn giá trị hợp lệ cuối cùng theo `thời điểm đo` trong bản tin HL7.
      - Ưu tiên `OBR-7`.
      - Nếu `OBR-7` trống thì fallback `OBX-14`.
    - Nếu chu kỳ chỉ có 1 bản tin đo thì gửi gói dữ liệu của bản tin đó khi đóng chu kỳ.
    - Gói dữ liệu đủ điều kiện được gán trạng thái `Chờ gửi HIS` và gửi sang HIS kèm `Mã máy`.
14. Nếu HIS nhận được dữ liệu thì Tool cập nhật trạng thái `Đã gửi HIS`.
15. Nếu lỗi kết nối, lỗi kỹ thuật hoặc lỗi dữ liệu khi gửi thì Tool cập nhật trạng thái `Gửi HIS lỗi`, lưu `Lý do lỗi` tại Tool và đưa vào danh sách lỗi gửi HIS.
16. CNTT và Triển khai ISOFH theo dõi lỗi gửi HIS, kiểm tra log và thực hiện resend khi cần.
17. Ở Phase II, `SAKURA` tổng hợp dữ liệu sinh hiệu đã ghi nhận để hiển thị Dashboard theo phạm vi Người bệnh phụ trách của Điều dưỡng hoặc Bác sĩ.
    - Dashboard chỉ phục vụ theo dõi tổng quan.
    - Không thay thế xem chi tiết hồ sơ điều trị.

### 7.3. Luồng ngoại lệ

- Không tìm thấy Người bệnh theo `Mã hồ sơ`:
  - Bao gồm trường hợp `Mã hồ sơ` không tồn tại.
  - Bao gồm trường hợp `Mã hồ sơ` thuộc lượt hoặc đợt điều trị đã ở trạng thái `Đã ra viện (100)` trở lên.
  - Phần hiển thị chi tiết trên Monitor thuộc phạm vi thiết bị.
  - Cơ sở y tế cần có hướng dẫn vận hành.
    - Quét lại `Mã hồ sơ`.
    - Bấm `Find Patient` lại.
    - Nếu vẫn lỗi thì reset Monitor và liên hệ đơn vị thiết bị.
- `Find Patient` thành công nhưng Monitor không hiển thị đủ thông tin hành chính tối thiểu gồm `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính`:
  - Không được thực hiện đo.
  - Không được ghi nhận dữ liệu sang HIS.
  - Nhân viên y tế cần thực hiện lại `Find Patient`.
  - Nếu vẫn không đủ thông tin thì chuyển sang quy trình tay và liên hệ hỗ trợ kỹ thuật.
- HIS lỗi hoặc Tool không gọi được HIS ở bước tra cứu Người bệnh:
  - Cơ sở y tế vận hành theo quy trình tay.
  - Điều dưỡng có thể chuyển `New Patient` trên Monitor để đo dữ liệu trắng.
  - Sau đó nhập tay thông số vào HIS.
  - Dữ liệu trắng vẫn đi vào ISOFHTool nhưng chỉ lưu log kỹ thuật và bảng lỗi với `Lý do lỗi = Không có Mã hồ sơ`.
  - Không gửi dữ liệu trắng sang HIS.
- Điều dưỡng phát hiện Monitor hiển thị sai Người bệnh sau khi quét mã:
  - Tạm chuyển sang quy trình tay để tránh chờ.
  - Đồng thời điều tra nguyên nhân gốc giữa dữ liệu HIS, thao tác người dùng và thiết bị để xử lý triệt để.
- Monitor vẫn hiển thị Người bệnh cũ dù đã quét đúng mã:
  - Đây là tình huống phía thiết bị.
  - Hướng xử lý trước mắt:
    - Quét lại.
    - `Find Patient` lại.
    - Thử lại vài lần.
    - Nếu không được thì reset Monitor và liên hệ hãng.
  - Nội dung này cần đưa vào tài liệu hướng dẫn sử dụng và vận hành.
- Chỉ số hoặc bản tin trung gian không được chọn vào gói dữ liệu cuối cùng của chu kỳ:
  - ISOFHTool chỉ lưu log kỹ thuật.
  - Không gửi sang HIS.
  - Chưa cần màn hình tra cứu cho người dùng ở giai đoạn đầu.
- Bản ghi đến trễ có `thời điểm đo` thuộc chu kỳ đã đóng:
  - Tool xử lý theo `thời điểm đo` trong bản tin HL7.
    - Ưu tiên `OBR-7`.
    - Nếu `OBR-7` trống thì fallback `OBX-14`.
  - Tool so theo cùng `Mã hồ sơ` tại `PID-3`.
  - Vẫn nhận vào chu kỳ đã đóng.
  - Nếu bản ghi đến trễ làm thay đổi gói dữ liệu cuối cùng hợp lệ của chu kỳ thì Tool phải cho phép resend để cập nhật lại HIS.
- Chu kỳ đủ `10 phút` hoặc phát sinh `Find Patient` mới:
  - Tool đóng chu kỳ hiện tại.
  - Nếu chu kỳ có dữ liệu thì tạo gói dữ liệu cuối cùng của chu kỳ để gửi sang HIS.
- HIS báo trùng dữ liệu:
  - Tool vẫn cập nhật trạng thái `Đã gửi HIS`.
  - HIS tự xử lý phần chống trùng theo rule hiện có.
- Sai `Mã máy`:
  - HIS vẫn nhận và lưu dữ liệu sinh hiệu.
  - Vấn đề cần theo dõi là mapping `Mã máy`.
  - Không cần resend dữ liệu.
  - Cần kiểm tra lại cấu hình `Mã máy` trong giai đoạn cấu hình và test đầu vào.
- Lỗi kết nối khi gửi HIS:
  - Phase I retry tự động `3 lần`.
  - Nếu vẫn lỗi sau ngưỡng retry thì đưa sang trạng thái `Gửi HIS lỗi` để CNTT hoặc Triển khai xử lý resend thủ công theo từng giao dịch lỗi.
  - Phase II retry theo `Thiết lập chung`.
  - Nếu quá ngưỡng thì tiếp tục đưa vào diện resend.
- Lỗi dữ liệu khi gửi HIS:
  - Phase I không retry tự động.
  - Ghi log lại tại Tool.
  - Cập nhật trạng thái `Gửi HIS lỗi`.
  - Cho phép resend thủ công theo từng giao dịch lỗi.
  - Phase II hỗ trợ tự động retry `3 lần` hoặc theo số lần trong thiết lập.
  - Nếu quá ngưỡng retry thì tiếp tục ghi log để theo dõi và xử lý resend.
- Không nhận được dữ liệu trong thời gian dài:
  - Giai đoạn đầu chưa triển khai cảnh báo tự động cho tình huống này.
  - Sẽ đánh giá tiếp sau pilot.

## 8. Quy tắc nghiệp vụ mức tổng quan

- `Mã hồ sơ` dùng cho tích hợp phải xác định duy nhất đúng 1 lượt khám hoặc 1 lượt điều trị trong HIS.
- HIS chỉ trả về thông tin Người bệnh khi `Mã hồ sơ` thuộc đúng 1 lượt hoặc đợt điều trị có trạng thái `< Đã ra viện (100)`; nếu không có hoặc `Mã hồ sơ` đã ra viện thì không trả thông tin để tránh đo vào hồ sơ cũ.
- Một Người bệnh không có hơn 1 `Mã hồ sơ` hợp lệ cùng lúc trong phạm vi tích hợp này.
- Khi chuyển khoa, chuyển giường hoặc đổi Bác sĩ điều trị thì `Mã hồ sơ` không đổi.
- Chỉ được ghi nhận dữ liệu sinh hiệu khi Nhân viên y tế đã đối chiếu đúng Người bệnh trên Monitor theo quy trình vận hành.
- Giai đoạn đầu, điều kiện ghép đúng Người bệnh được kiểm soát chủ yếu bằng quy trình người dùng: quét đúng `Mã hồ sơ`, đối chiếu đúng thông tin Người bệnh trên Monitor rồi mới đo.
- `Find Patient` là thao tác bắt buộc trên Monitor để ghép đúng ngữ cảnh Người bệnh trước khi đo.
- Tại bước `Find Patient`, Monitor gửi bản tin HL7 `QRY^A19` đến ISOFHTool; ISOFHTool chuyển tiếp sang HIS để tra cứu và parse bản tin HL7 `ADR^A19` theo dữ liệu HIS trả về để phản hồi lại Monitor.
- Khi ISOFHTool nhận được bản tin HL7 `ORU^R01` từ Monitor thì phải phản hồi lại bản tin HL7 `ACK` để Monitor duy trì kết nối và tiếp tục gửi các lượt đo tiếp theo nếu có.
- Khi `Find Patient` thành công, Monitor phải hiển thị đầy đủ thông tin hành chính theo dữ liệu HIS trả về; tối thiểu gồm `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính`; nếu thiết bị hỗ trợ thì hiển thị thêm `Tuổi`. Nếu không hiển thị đủ thông tin tối thiểu thì không được đo và không được ghi nhận dữ liệu sang HIS.
- Phase III định hướng bổ sung đối chiếu hệ thống: nếu tên hoặc ngày sinh hiển thị trên Monitor không khớp với HIS tại thời điểm đó thì đưa vào danh sách cảnh báo và hiển thị trên dashboard.
- Dashboard Phase II trên `SAKURA` chỉ hiển thị dữ liệu theo dõi tổng quan của Người bệnh thuộc phạm vi phụ trách theo quyền được phân; không thay thế màn hình chi tiết sinh hiệu, không thay thế đánh giá lâm sàng và không thay thế quy trình ký hoặc khóa hồ sơ.
- Tiêu chí hiển thị bất thường trên Dashboard phải đi theo rule nghiệp vụ hoặc cấu hình được chốt riêng; không hard-code theo một khoa hoặc một Cơ sở y tế nếu chưa xác minh dùng chung.
- ISOFHTool là điểm tiếp nhận dữ liệu từ Monitor và quản lý trạng thái xử lý của từng bản ghi gửi HIS.
- HIS tiếp tục chống trùng theo `Mã hồ sơ + thời điểm đo sinh hiệu`.
- Mốc thời gian chuẩn để chống trùng là `thời điểm đo sinh hiệu`, không dùng thời điểm Tool nhận bản tin.
- Chu kỳ ghi nhận sinh hiệu được tính từ thời điểm `Find Patient`.
  - Phase I dùng chu kỳ mặc định `10 phút`.
  - Phase I tập trung kết nối `05` chỉ số sống cơ bản gồm `Mạch`, `Nhiệt độ`, `Huyết áp`, `Nhịp thở`, `SpO2`.
    - Riêng chỉ số `Nhiệt độ` cần kiểm tra lại thiết bị pilot có hỗ trợ hay không.
  - Ở Phase I, chu kỳ được chia theo từng đợt `10 phút` tính từ thời điểm `Find Patient`, ví dụ `T0 -> T0+10 phút`, `T0+10 phút -> T0+20 phút`, `T0+20 phút -> T0+30 phút`.
  - Nếu phát sinh `Find Patient` mới khi chu kỳ hiện tại chưa kết thúc thì chu kỳ cũ được đóng tại thời điểm `Find Patient` mới.
  - Từ Phase II, chu kỳ mặc định lấy theo `Thiết lập chung`.
    - Nếu Người bệnh có khai báo chu kỳ riêng thì ưu tiên lấy theo khai báo của Người bệnh.
    - Nếu không có thì lấy theo `Thiết lập chung`.
  - Từ Phase II, mở rộng thu thập thêm các dữ liệu khác ngoài `05` chỉ số sống cơ bản của Phase I.
  - Trong cùng 1 chu kỳ, Tool gom nhiều bản tin `ORU^R01` thành 1 gói dữ liệu duy nhất để gửi sang HIS khi chu kỳ đóng.
    - Nếu chu kỳ chỉ có 1 bản tin đo thì gửi gói dữ liệu của bản tin đó khi chu kỳ được đóng.
    - Với từng chỉ số trong cùng chu kỳ, Tool chọn giá trị hợp lệ cuối cùng theo `thời điểm đo`; ưu tiên `OBR-7`, nếu `OBR-7` trống thì fallback `OBX-14`.
    - Nếu có nhiều giá trị của cùng 1 chỉ số có cùng thời điểm đo trong cùng chu kỳ, Tool ưu tiên giá trị từ bản tin Tool nhận được sau cùng.
    - Chỉ số hoặc bản tin trung gian không được chọn vào gói dữ liệu cuối cùng của chu kỳ không gửi sang HIS nhưng vẫn phải có log kỹ thuật để kiểm tra khi cần.
- Bản ghi đến trễ được xử lý theo `thời điểm đo` trong bản tin HL7, ưu tiên `OBR-7`, nếu `OBR-7` trống thì fallback `OBX-14`, không dùng thời điểm Tool nhận bản tin để quyết định chu kỳ.
- Tool so bản ghi đến trễ theo cùng `Mã hồ sơ` tại `PID-3`; nếu `thời điểm đo` thuộc chu kỳ đã đóng thì vẫn nhận vào đúng chu kỳ đã đóng đó.
- Nếu bản ghi đến trễ làm thay đổi gói dữ liệu cuối cùng hợp lệ của chu kỳ thì Tool phải cho phép resend để cập nhật lại dữ liệu trên HIS.
- Nếu không `Find Patient` được Người bệnh thì Điều dưỡng có thể chuyển `New Patient` trên Monitor để đo dữ liệu trắng; dữ liệu này không gắn Người bệnh trên HIS và xử lý theo quy trình tay.
- Dữ liệu trắng vẫn đi vào ISOFHTool nhưng chỉ lưu log kỹ thuật và bảng lỗi với `Lý do lỗi = Không có Mã hồ sơ`; không gửi sang HIS.
- Phase I dùng lại API `SAKURA` hiện có để ISOFHTool lấy thông tin Người bệnh và cập nhật `05` chỉ số sống cơ bản; phía BE không phải làm thêm API mới trong phase này.
- Giai đoạn đầu chưa giới hạn thời gian hiệu lực của một lần ghép Người bệnh trên Monitor.
- Nếu HIS đã nhận được dữ liệu nhưng tự xử lý trùng bên trong HIS thì trạng thái giao dịch ở Tool vẫn là `Đã gửi HIS`.
- Sai `Mã máy` không được xem là lỗi gửi HIS nếu HIS vẫn nhận được dữ liệu; trường hợp này cần đối soát mapping chứ không cần resend dữ liệu, và giai đoạn triển khai ban đầu chưa yêu cầu sửa hồi cứu trên HIS.
- Với lỗi dữ liệu gửi HIS, Phase I không retry tự động; Tool ghi log, cập nhật trạng thái `Gửi HIS lỗi` và hỗ trợ resend thủ công theo từng giao dịch lỗi.
- Với lỗi dữ liệu gửi HIS, Phase II hỗ trợ tự động retry gửi lại `3 lần` hoặc theo số lần trong thiết lập; nếu quá ngưỡng retry thì tiếp tục ghi log, đưa vào diện theo dõi và cho phép resend.
- Với lỗi kết nối gửi HIS, Phase I mặc định retry tự động `3 lần` trước khi đưa giao dịch sang diện theo dõi resend.
- Với lỗi kết nối gửi HIS, Phase II retry theo `Thiết lập chung`; nếu quá ngưỡng thì đưa giao dịch sang diện theo dõi resend.
- Phase I bắt buộc hỗ trợ resend thủ công theo từng giao dịch lỗi.
- Resend theo lô không phải tiêu chí bắt buộc để nghiệm thu Phase I; nếu chưa đủ effort thì chuyển sang Phase II.
- `Lý do lỗi` trước mắt chỉ lưu tại ISOFHTool.
- Giai đoạn đầu chưa cần cảnh báo tự động cho tình huống quá thời gian không có dữ liệu hoặc đang theo dõi lâu mà chưa nhận dữ liệu.
- Mọi thao tác resend, thay đổi cấu hình, thay đổi `Mã máy` trên Tool và xử lý lỗi gửi HIS phải có log kỹ thuật để kiểm tra.
- Phase I chưa làm user-level permission trên ISOFHTool; đây là giới hạn của pilot. Trong giai đoạn này, Tool vận hành trên máy server dùng riêng; ai vào được ISOFHTool trên máy này thì đều xem được màn hình kỹ thuật theo phạm vi vận hành hiện tại.

### Rule có thể cấu hình

| Rule | Có thể cấu hình không | Mức cấu hình | Ghi chú |
| --- | --- | --- | --- |
| Chu kỳ ghi nhận sinh hiệu | Có | Phase I dùng mặc định `10 phút`; Phase II cấu hình qua `Thiết lập chung` và ưu tiên khai báo riêng theo từng Người bệnh nếu có | Tool không cần lấy chu kỳ qua API `Find Patient` ở Phase I |
| Số lần retry khi lỗi gửi HIS | Có định hướng | Phase I cố định theo rule đã chốt; Phase II thực hiện theo `Thiết lập chung` trên HIS | Phase I: lỗi kết nối retry tự động `3 lần`, lỗi dữ liệu không retry tự động |
| IP kết nối tới Monitor | Có | Theo từng thiết bị | Cần cập nhật lại nếu đổi IP thiết bị |
| `Mã máy` gắn với thiết bị trên Tool | Có | Theo từng thiết bị | Giai đoạn đầu nhập tay trên Tool |
| Phân quyền xem dữ liệu nghiệp vụ trên HIS | Có | Theo vai trò người dùng | HIS tạo quyền để phân cho người dùng |
| Phân quyền xem log kỹ thuật | Có | Theo vai trò kỹ thuật | Dành cho DEV, QA, Triển khai, CNTT |
| Phân quyền trên ISOFHTool | Chưa yêu cầu ở Phase I | Phase I vận hành bằng máy dùng riêng cho CNTT / Triển khai; phân quyền kỹ thuật chi tiết đánh giá ở phase sau | Áp dụng cho màn hình lỗi, log, resend và cấu hình Tool trong giai đoạn đầu |

### Rule retry / resend theo phase

| Tình huống | Phase I | Phase II |
| --- | --- | --- |
| Lỗi kết nối gửi HIS | Retry tự động `3 lần`; nếu vẫn lỗi thì cập nhật `Gửi HIS lỗi` và cho resend thủ công theo từng giao dịch lỗi | Retry theo `Thiết lập chung`; nếu quá ngưỡng thì cập nhật `Gửi HIS lỗi` và cho resend |
| Lỗi dữ liệu gửi HIS | Không retry tự động; cập nhật `Gửi HIS lỗi`, lưu `Lý do lỗi` và cho resend thủ công theo từng giao dịch lỗi | Retry tự động `3 lần` hoặc theo `Thiết lập chung`; nếu quá ngưỡng thì cập nhật `Gửi HIS lỗi` và cho resend |
| Bản ghi đến trễ thuộc chu kỳ đã đóng | Vẫn nhận vào đúng chu kỳ theo `thời điểm đo` trong bản tin HL7; ưu tiên `OBR-7`, nếu `OBR-7` trống thì fallback `OBX-14`; nếu làm thay đổi gói dữ liệu cuối cùng thì cho resend | Giữ cùng nguyên tắc Phase I; retry hoặc resend thực hiện theo thiết lập và cơ chế của Phase II |

## 9. Cấu hình cần có

| Nhóm cấu hình                      | Mô tả                                                                       | Bắt buộc | Áp dụng theo Cơ sở y tế | Ghi chú                                                                                    |
| ---------------------------------- | --------------------------------------------------------------------------- | -------- | ----------------------- | ------------------------------------------------------------------------------------------ |
| Danh mục Mã máy trên HIS           | Quản lý `Mã máy` dùng để lưu truy vết với bản ghi sinh hiệu                 | Có       | Có                      | - Trên HIS đã có danh mục mã máy<br>- HIS chỉ làm thêm lưu `Mã máy` cùng bản ghi sinh hiệu |
| Cấu hình kết nối Monitor trên Tool | Cấu hình IP và tham số kết nối để Tool lắng nghe dữ liệu từ Monitor         | Có       | Có                      | Nếu thiết bị đổi IP thì phải cập nhật lại cấu hình kết nối                                 |
| Cấu hình `Mã máy` trên Tool        | Gắn thiết bị thực tế với `Mã máy` trên HIS                                  | Có       | Có                      | Giai đoạn đầu nhập tay `Mã máy`, chưa cần gọi API lấy danh mục                             |
| Danh sách ghép chỉ số sống         | Mapping giữa chỉ số sống của `SAKURA` và chỉ số gửi từ Monitor              | Có       | Có                      | Phase I trước mắt tập trung `05` chỉ số cơ bản; mapping chi tiết chốt ở tài liệu detail    |
| Cấu hình chu kỳ ghi nhận           | Phase I dùng mặc định `10 phút`; Phase II cấu hình qua `Thiết lập chung`, và nếu có khai báo riêng theo từng Người bệnh thì ưu tiên lấy theo khai báo riêng | Có | Có | Không cần lấy chu kỳ qua API `Find Patient` ở Phase I |
| Cấu hình retry / resend            | Số lần retry, thao tác resend, quy tắc đổi trạng thái | Có       | Có                      | Phase I bắt buộc có resend thủ công theo từng giao dịch lỗi; resend theo lô không bắt buộc để nghiệm thu Phase I và có thể chuyển sang Phase II |
| Phân quyền trên HIS                | Quyền xem dữ liệu sinh hiệu, quyền xem màn hình nghiệp vụ, quyền cấu hình chu kỳ mặc định hoặc khai báo chu kỳ riêng theo từng Người bệnh ở Phase II | Có | Có | HIS tạo quyền để phân cho người dùng phù hợp |
| Cấu hình Dashboard trên `SAKURA`  | Phạm vi Người bệnh phụ trách, chỉ số tổng quan, quy tắc gắn cờ bất thường, cách sắp xếp ưu tiên hiển thị | Có định hướng | Có | Chi tiết rule hiển thị và nguồn xác định Người bệnh phụ trách cần chốt ở tài liệu detail |
| Phân quyền màn hình kỹ thuật       | Quyền xem log, lỗi API, lỗi mapping, danh sách resend                       | Có       | Có                      | Dành cho DEV, QA, Triển khai, CNTT                                                         |
| Thiết lập đồng bộ thời gian        | Đồng bộ Monitor, Tool, HIS theo GMT+7                                       | Có       | Có                      | Bắt buộc đưa vào checklist triển khai                                                      |

## 10. Dữ liệu bị tác động

| Nhóm dữ liệu | Tác động | Mô tả |
| --- | --- | --- |
| Hồ sơ Người bệnh / Lượt khám / Lượt điều trị | Chỉ đọc | HIS được tra cứu để xác định đúng Người bệnh theo `Mã hồ sơ`; chỉ trả thông tin khi lượt hoặc đợt điều trị có trạng thái `< Đã ra viện (100)` |
| Dữ liệu sinh hiệu | Tạo mới | HIS nhận gói dữ liệu sinh hiệu hợp lệ từ Tool |
| Dữ liệu sinh hiệu | Chống trùng / bỏ qua / resend | HIS chống trùng theo `Mã hồ sơ + thời điểm đo sinh hiệu`; Tool bỏ qua các chỉ số hoặc bản tin trung gian không được chọn vào gói dữ liệu cuối cùng của chu kỳ; bản ghi đến trễ có thể làm phát sinh resend |
| Dữ liệu phân công hoặc phạm vi phụ trách Người bệnh | Chỉ đọc / tổng hợp | Dashboard trên `SAKURA` dùng dữ liệu phân công hoặc phạm vi được phép xem để xác định danh sách Người bệnh cần theo dõi; nguồn xác định cụ thể cần chốt ở tài liệu detail |
| Dữ liệu sinh hiệu chi tiết | Tạo mới | Phase I trước mắt lưu `05` nhóm chỉ số sống cơ bản:<br>- Mạch<br>- Nhiệt độ<br>  - Cần kiểm tra lại thiết bị pilot có hỗ trợ hay không<br>- Huyết áp<br>- Nhịp thở<br>- SpO2<br>- Phase II mở rộng thêm các chỉ số khác và chốt mapping chi tiết ở tài liệu detail |
| Dữ liệu tổng hợp hiển thị Dashboard | Tạo mới / tổng hợp | `SAKURA` tổng hợp thời điểm đo gần nhất, trạng thái nhận dữ liệu, chỉ số cần theo dõi và cờ bất thường để hiển thị mức tổng quan; chi tiết trường hiển thị cần tách xuống tài liệu detail |
| `Mã máy` | Tạo mới / lưu kèm | HIS chỉ lưu `Mã máy` kèm bản ghi sinh hiệu để truy vết thiết bị; giai đoạn triển khai ban đầu chưa yêu cầu sửa hồi cứu nếu mapping sai |
| Giao dịch gửi HIS | Tạo mới / cập nhật | Quản lý trạng thái `Chờ gửi HIS`, `Đã gửi HIS`, `Gửi HIS lỗi` |
| `Lý do lỗi` | Tạo mới | Chỉ lưu tại ISOFHTool cho các lỗi gửi HIS và dữ liệu trắng với giá trị ví dụ `Không có Mã hồ sơ` |
| Cấu hình kết nối thiết bị | Tạo mới / cập nhật | Lưu cấu hình IP thiết bị và `Mã máy` tương ứng trên Tool |
| Log vận hành và audit | Tạo mới | Lưu log tìm Người bệnh, nhận dữ liệu, dữ liệu trắng, lọc theo chu kỳ, xử lý bản ghi đến trễ, gửi HIS, retry, resend, thay đổi cấu hình Tool |

## 11. Tác động đến phân hệ liên quan

| Phân hệ | Tác động | Mức độ | Ghi chú |
| --- | --- | --- | --- |
| Sinh hiệu | Nhận dữ liệu từ thiết bị, lưu `Mã máy`, áp rule chu kỳ, hiển thị dữ liệu sinh hiệu nghiệp vụ | Cao | Là phân hệ chịu tác động trực tiếp |
| Hồ sơ điều trị | Hiển thị dữ liệu sinh hiệu theo đúng `Mã hồ sơ` hoặc lượt điều trị | Vừa | Cần chi tiết ở tài liệu detail |
| `SAKURA` / Dashboard theo dõi tổng quan | Hiển thị danh sách Người bệnh phụ trách, trạng thái tổng quan, thời điểm đo gần nhất và dấu hiệu bất thường theo rule hiển thị | Cao | Chỉ áp dụng từ Phase II; không thay thế màn hình chi tiết sinh hiệu |
| Danh mục dùng chung | Dùng `Danh mục Mã máy` hiện có để phục vụ truy vết thiết bị | Vừa | Không tạo mới danh mục nghiệp vụ khác |
| Quản trị hệ thống | Bổ sung quyền xem dữ liệu nghiệp vụ, quyền cấu hình chu kỳ mặc định hoặc khai báo chu kỳ riêng theo từng Người bệnh ở Phase II, màn hình vận hành kỹ thuật | Cao | Cần tách rõ vai trò xem dữ liệu và xem log |
| Tài liệu hướng dẫn sử dụng / vận hành | Bổ sung hướng dẫn xử lý tình huống `Find Patient` lỗi, sai Người bệnh, sai `Mã máy`, chuyển sang nhập tay | Cao | Là đầu ra bắt buộc cho pilot |
| Báo cáo vận hành | Có thể bổ sung thống kê lỗi gửi HIS, thời gian xử lý lỗi, số lần resend | Vừa | Mức chi tiết báo cáo sẽ chốt sau |

## 12. Tác động đến tích hợp

| Hệ thống | Chiều dữ liệu | Trigger | Dữ liệu chính | Ghi chú |
| --- | --- | --- | --- | --- |
| Monitor PVM-2701 -> ISOFHTool | Gửi | Kiểm tra ngữ cảnh Người bệnh trên Monitor, chuyển `New Patient` nếu đang có Người bệnh cũ, quét `Mã hồ sơ`, chọn `Find Patient`, hoàn tất đo | Yêu cầu tìm Người bệnh `QRY^A19`, dữ liệu sinh hiệu `ORU^R01`, thông tin nhận diện thiết bị | Tool lắng nghe kết nối từ Monitor theo `IP`, `PORT`; định dạng bản tin detail sẽ tách tài liệu sau |
| ISOFHTool -> Monitor | Gửi | Phản hồi sau tra cứu Người bệnh hoặc sau khi nhận bản tin đo | `ADR^A19`, `ACK` | `ACK` được gửi sau khi nhận `ORU^R01` để Monitor duy trì kết nối và tiếp tục gửi dữ liệu |
| ISOFHTool -> HIS | Gửi / Nhận | Tìm Người bệnh, gửi gói dữ liệu sinh hiệu, retry, resend | `Mã hồ sơ`, thông tin Người bệnh, dữ liệu sinh hiệu, `Mã máy`, trạng thái giao dịch | Phase I dùng lại API `SAKURA` hiện có; chưa cần HIS trả chu kỳ qua API `Find Patient`; nếu bản ghi đến trễ làm thay đổi gói dữ liệu cuối cùng của chu kỳ thì Tool cho phép resend |
| HIS -> ISOFHTool | Nhận | Tool gọi tra cứu `Mã hồ sơ` qua API `nbDotDieuTriId` | Thông tin Người bệnh | HIS chỉ trả thông tin khi `Mã hồ sơ` thuộc lượt hoặc đợt điều trị có trạng thái `< Đã ra viện (100)` |
| ISOFHTool <-> HIS | Hai chiều | Gửi gói dữ liệu sinh hiệu, nhận phản hồi xử lý, theo dõi lỗi gửi, resend | Trạng thái gửi HIS, `Lý do lỗi`, log gửi nhận | Sai `Mã máy` không mặc định là lỗi resend nếu HIS vẫn nhận dữ liệu; dữ liệu trắng không gửi sang HIS |
| HIS / `SAKURA` | Nội bộ hệ thống / hiển thị | Người dùng mở Dashboard theo dõi tổng quan | Danh sách Người bệnh phụ trách, thời điểm đo gần nhất, trạng thái tổng quan, cờ bất thường theo rule hiển thị | Chưa xác định ở mức high level Dashboard đọc trực tiếp từ HIS hay qua lớp tổng hợp riêng; cần chốt trong tài liệu detail |

### Bản tin HL7 mẫu tham chiếu từ Monitor

```hl7
MSH|^~\&|NIHON KOHDEN|NIHON KOHDEN|CLIENT APP|CLIENT FACILITY|20260422184400||ORU^R01^ORU_R01|20260422000007|P|2.4|||NE|AL||ASCII||ASCII
PID|||26031200119||MAI^NGOC^^^^^L^A||19910427|M
PV1||I|^^BED-001^10.0.100.98:1
ORC|RE
OBR|1|||VITAL|||20260422184400||||||||||||||||||A
OBX|1|NM|007001^VITAL PR(spo2)|1|77|/min|||||F|||20260422184400|||
OBX|2|NM|007000^VITAL SpO2|1|99|%|||||F|||20260422184400|||
OBX|3|NM|072007^VITAL rPR(spo2)|1|77|/min|||||F|||20260422184400|||
OBR|2|||NIBP|||20260422184337||||||||||||||||||A
OBX|1|NM|009000^NIBP SYS|1|133|mmHg|||||F|||20260422184337|||
OBX|2|NM|009001^NIBP DIAS|1|77|mmHg|||||F|||20260422184337|||
OBX|3|NM|009002^NIBP MEAN|1|95|mmHg|||||F|||20260422184337|||
```

### Điểm cần khai thác từ bản tin HL7 mẫu

| Thành phần HL7 | Ý nghĩa nghiệp vụ | Ghi chú |
| --- | --- | --- |
| `PID-3` | Mã định danh Người bệnh / `Mã hồ sơ` được Monitor gửi trong bản tin | Cần chốt mapping chi tiết ở tài liệu detail |
| `QRY^A19` | Bản tin tra cứu Người bệnh từ Monitor sang ISOFHTool | Dùng tại bước `Find Patient` |
| `ADR^A19` | Bản tin phản hồi thông tin hành chính từ ISOFHTool về Monitor theo dữ liệu HIS trả về | Cần chốt mapping chi tiết ở tài liệu detail |
| `ACK` | Bản tin xác nhận ISOFHTool đã nhận `ORU^R01` từ Monitor | Dùng để Monitor duy trì kết nối và tiếp tục gửi dữ liệu |
| `OBR-7` | Thời điểm đo của nhóm chỉ số | Là nguồn thời gian tham chiếu cho từng nhóm kết quả trong bản tin |
| `OBX-14` | Thời điểm đo của chỉ số nếu có gửi kèm ở mức OBX | Dùng để xử lý bản ghi đến trễ khi thiết bị gửi thời gian ở mức chỉ số |
| `OBX-3` | Mã chỉ số sinh hiệu | Ví dụ mẫu hiện có các mã PR, SpO2, rPR, SYS, DIAS, MEAN |
| `OBX-5` | Giá trị đo | Gửi sang HIS theo mapping chi tiết từng chỉ số |
| `OBX-6` | Đơn vị đo | Ví dụ `/min`, `%`, `mmHg` |

Ghi chú: Bản tin mẫu trên dùng để chốt hướng đọc dữ liệu từ Monitor. Mapping chi tiết giữa mã `OBX-3` và trường lưu trên HIS, danh sách trường bắt buộc hay có thể rỗng, số lẻ cho phép và quy tắc chuẩn hóa sẽ được mô tả ở tài liệu detail tích hợp.

## 13. Phân quyền và truy vết

| Hành động | Nhóm được làm | Điều kiện | Có cần log / audit không |
| --- | --- | --- | --- |
| Xem dữ liệu sinh hiệu nghiệp vụ trên HIS hoặc Dashboard tổng quan trên `SAKURA` | Điều dưỡng / Bác sĩ / Quản lý Cơ sở y tế / CNTT / Triển khai - Hỗ trợ theo quyền được phân | Theo quyền do HIS cấp hoặc quyền trên `SAKURA`; Dashboard chỉ hiển thị Người bệnh thuộc phạm vi phụ trách hoặc phạm vi được phân | Có |
| Cấu hình chu kỳ mặc định / khai báo chu kỳ riêng theo từng Người bệnh | Nhóm được phân quyền trên HIS | Chỉ áp dụng từ Phase II theo quyền do HIS cấp | Có |
| Xem log kỹ thuật, lỗi API, lỗi mapping, lịch sử resend | DEV / QA / Triển khai / CNTT | Phase I vận hành trên máy server dùng riêng; ai truy cập được ISOFHTool trên máy này thì đều xem được theo phạm vi vận hành hiện tại | Có |
| Cấu hình IP thiết bị và `Mã máy` trên Tool | CNTT / Triển khai ISOFH | Phase I vận hành trên máy server dùng riêng và chưa tách quyền chi tiết trên Tool | Có |
| Resend giao dịch lỗi | CNTT / Triển khai ISOFH | Giao dịch ở trạng thái `Gửi HIS lỗi` do lỗi gửi HIS hoặc bản ghi đến trễ cần cập nhật lại HIS; Phase I thao tác trên máy server dùng riêng | Có |
| Theo dõi lỗi hằng ngày giai đoạn đầu | CNTT / Triển khai ISOFH | Trong thời gian đầu go-live, Triển khai theo dõi hằng ngày; sau khoảng 1 tuần ổn định có thể bàn giao cho CNTT | Có |

## 14. Yêu cầu phi chức năng

- Độ đúng dữ liệu:
  - Ưu tiên cao nhất là ghi nhận đúng `Mã hồ sơ` và đúng dữ liệu sinh hiệu.
  - Sai lệch có thể gây sự cố nghiêm trọng.
- Ổn định vận hành:
  - Tool không được làm mất dữ liệu đã nhận khi HIS lỗi tạm thời.
- Retry / resend:
  - Phase I bắt buộc có màn hình lỗi và chức năng resend thủ công theo từng giao dịch lỗi trên ISOFHTool.
  - Phase I chỉ retry tự động `3 lần` với lỗi kết nối.
  - Lỗi dữ liệu ở Phase I không retry tự động.
  - Phase II hỗ trợ retry theo `Thiết lập chung`.
  - Bản ghi đến trễ thuộc chu kỳ đã đóng vẫn được nhận theo `thời điểm đo` trong bản tin HL7.
    - Ưu tiên `OBR-7`.
    - Nếu `OBR-7` trống thì fallback `OBX-14`.
  - Nếu bản ghi đến trễ làm thay đổi gói dữ liệu cuối cùng thì phải cho resend.
  - Resend theo lô không phải tiêu chí bắt buộc để nghiệm thu Phase I và có thể chuyển sang Phase II.
- Phân quyền:
  - Tách màn hình nghiệp vụ và màn hình kỹ thuật.
  - HIS quản lý quyền dữ liệu nghiệp vụ.
  - Phase I chưa có user-level permission trên ISOFHTool.
  - Đây là giới hạn pilot và được kiểm soát bằng máy server dùng riêng.
- Dashboard tổng quan:
  - Phase II bổ sung Dashboard trên `SAKURA` cho Điều dưỡng hoặc Bác sĩ theo dõi Người bệnh thuộc phạm vi phụ trách.
  - Dashboard ưu tiên hiển thị dữ liệu đủ nhanh để theo dõi tổng quan.
  - Chi tiết ngưỡng bất thường, cơ chế làm tươi dữ liệu và cách xác định phạm vi phụ trách cần chốt ở tài liệu detail.
- Truy vết:
  - Phải có log cho tìm Người bệnh, nhận dữ liệu, dữ liệu trắng, lọc theo chu kỳ, xử lý bản ghi đến trễ, gửi HIS, retry, resend, thay đổi cấu hình Tool và cập nhật trạng thái giao dịch.
- Lưu lỗi:
  - `Lý do lỗi` trước mắt chỉ cần lưu ở ISOFHTool.
  - Bao gồm lỗi gửi HIS và dữ liệu trắng với `Lý do lỗi = Không có Mã hồ sơ`.
- Đồng bộ thời gian:
  - Monitor, Tool và HIS phải dùng cùng thời gian GMT+7.
  - Đây là điều kiện bắt buộc trước khi chạy thật.
- Vận hành dự phòng:
  - Khi HIS hoặc Tool không tra cứu được Người bệnh, Cơ sở y tế phải có quy trình nhập tay để không làm gián đoạn chăm sóc Người bệnh.
- Cảnh báo vận hành:
  - Giai đoạn đầu chỉ cần log nội bộ.
  - Chưa triển khai cảnh báo tự động cho tình huống quá thời gian không có dữ liệu.

### Trạng thái giao dịch gửi HIS

| Trạng thái | Ý nghĩa |
| --- | --- |
| `Chờ gửi HIS` | Gói dữ liệu đã được Tool tiếp nhận, đủ điều kiện và đang chờ gửi sang HIS |
| `Đã gửi HIS` | Tool đã gửi thành công sang HIS. Trường hợp HIS nhận thành công nhưng xử lý trùng bên trong HIS vẫn xem là `Đã gửi HIS` |
| `Gửi HIS lỗi` | Tool không gửi được sang HIS hoặc phát sinh lỗi dữ liệu cần theo dõi, retry hoặc resend tùy theo phase và loại lỗi |

### Trường theo dõi lỗi

| Trường | Ý nghĩa |
| --- | --- |
| `Lý do lỗi` | Lưu chi tiết lỗi tại ISOFHTool, ví dụ: mất kết nối, lỗi API, lỗi dữ liệu gửi, `Không có Mã hồ sơ` |

### Theo dõi lỗi gửi HIS

- Cần có bảng theo dõi riêng cho các transaction gửi HIS lỗi để phục vụ tra cứu, resend và đối soát.
- Phase I bắt buộc có màn hình lỗi và chức năng resend thủ công theo từng giao dịch lỗi trên ISOFHTool để xử lý các transaction gửi HIS lỗi.
- Thao tác resend thủ công do Triển khai ISOFH hoặc CNTT thực hiện khi kiểm tra thấy dữ liệu sinh hiệu chưa lên màn hình Khám bệnh hoặc màn hình Sinh hiệu.
- Số lần resend thủ công tham chiếu ở giai đoạn đầu là khoảng `3 - 5 lần`; nếu vẫn không thành công thì phối hợp DEV để điều tra nguyên nhân.
- Sai `Mã máy` nhưng HIS vẫn nhận được dữ liệu không xem là lỗi resend; cần kiểm tra lại checklist mapping.
- Giai đoạn triển khai ban đầu chưa yêu cầu sửa hồi cứu dữ liệu đã lưu sai `Mã máy` trên HIS.
- Với lỗi dữ liệu gửi HIS, Phase I cập nhật `Gửi HIS lỗi`, log lại để theo dõi và resend; Phase II hỗ trợ retry `3 lần` hoặc theo thiết lập trước khi đưa vào diện theo dõi tiếp.
- Với bản ghi đến trễ thuộc chu kỳ đã đóng, nếu bản ghi làm thay đổi gói dữ liệu cuối cùng hợp lệ của chu kỳ thì Tool phải cho phép resend để cập nhật lại HIS.
- Dữ liệu trắng phải vào log kỹ thuật và bảng lỗi của Tool với `Lý do lỗi = Không có Mã hồ sơ`, không vào danh sách gửi HIS.
- Giai đoạn đầu chưa cần cảnh báo tự động cho tình huống quá thời gian không có dữ liệu.

## 15. Điều kiện chấp nhận mức high level

- [x] Mục tiêu và phạm vi đã rõ.
- [x] Đã chốt `Mã hồ sơ` là định danh duy nhất cho 1 lượt khám hoặc lượt điều trị trong phạm vi tích hợp.
- [x] Đã chốt rule HIS chỉ trả thông tin cho `Mã hồ sơ` của lượt hoặc đợt điều trị có trạng thái `< Đã ra viện (100)`.
- [x] Đã chốt rule chu kỳ tính từ thời điểm `Find Patient`, gom dữ liệu theo chu kỳ và tạo gói dữ liệu cuối cùng để gửi HIS.
- [x] Đã chốt rule xử lý bản ghi đến trễ theo `thời điểm đo` trong bản tin HL7, ưu tiên `OBR-7`, fallback `OBX-14`, và cơ chế resend khi cần.
- [x] Đã chốt nguyên tắc chống trùng theo `Mã hồ sơ + thời điểm đo sinh hiệu`.
- [x] Đã chốt nguyên tắc sai `Mã máy` không chặn HIS lưu dữ liệu và không yêu cầu resend.
- [x] Đã chốt cách xử lý dữ liệu trắng trên ISOFHTool.
- [x] Đã tách màn hình nghiệp vụ và màn hình vận hành kỹ thuật.
- [x] Đã xem xét phân quyền, log kỹ thuật, retry, resend và quy trình tay dự phòng.
- [x] Đã bổ sung định hướng Phase II cho Dashboard theo dõi tổng quan trên `SAKURA` dành cho Điều dưỡng hoặc Bác sĩ.
- [x] Đủ rõ để tách xuống tài liệu detail level.

### UAT bắt buộc cho pilot

Ghi chú: UAT pilot tập trung các case cơ bản. Các case ngoại lệ và case biên sẽ được QA tách thành tài liệu testcase chi tiết.

- Kết nối được ISOFHTool với Monitor.
- Có màn hình hoặc chỗ cấu hình trên ISOFHTool để mapping nhận dữ liệu từ Monitor.
- Phase I có màn hình lỗi và chức năng resend thủ công theo từng giao dịch lỗi trên ISOFHTool.
- Nếu Monitor đang hiển thị Người bệnh cũ thì phải thao tác `Tên Người bệnh` -> `Admit` -> `New Patient` trước khi quét barcode `Mã hồ sơ` của Người bệnh mới.
- Chỉ tìm được Người bệnh trên Monitor khi `Mã hồ sơ` thuộc lượt hoặc đợt điều trị có trạng thái `< Đã ra viện (100)`; Monitor gửi bản tin HL7 `QRY^A19`, ISOFHTool parse bản tin HL7 `ADR^A19` từ dữ liệu HIS trả về và Monitor hiển thị tối thiểu `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính`, và `Tuổi` nếu thiết bị hỗ trợ.
- Nếu `Mã hồ sơ` không tồn tại hoặc đã ở trạng thái `Đã ra viện (100)` thì HIS không trả thông tin Người bệnh.
- ISOFHTool nhận được dữ liệu đo từ thiết bị.
- Sau khi nhận bản tin `ORU^R01`, ISOFHTool phải phản hồi bản tin HL7 `ACK` để Monitor tiếp tục duy trì kết nối và gửi các lượt đo tiếp theo nếu có.
- HIS lưu được đúng dữ liệu đo sinh hiệu theo `Mã hồ sơ` đã đo.
- Phase I trước mắt ghi nhận `05` chỉ số sống cơ bản gồm `Mạch`, `Nhiệt độ`, `Huyết áp`, `Nhịp thở`, `SpO2`.
- Phase I áp dụng chu kỳ mặc định `10 phút` để lọc gói dữ liệu gửi HIS.
- Chu kỳ Phase I được đóng sau mỗi `10 phút` tính từ thời điểm `Find Patient`; nếu phát sinh `Find Patient` mới trước khi hết `10 phút` thì chu kỳ cũ phải được đóng ngay.
- Nếu trong 1 chu kỳ chỉ có 1 bản tin đo thì hệ thống phải gửi gói dữ liệu của bản tin đó khi chu kỳ kết thúc.
- Nếu có bản ghi đến trễ, Tool phải xử lý theo `thời điểm đo` trong bản tin HL7, ưu tiên `OBR-7`, nếu `OBR-7` trống thì fallback `OBX-14`, vẫn nhận vào đúng chu kỳ đã đóng và cho phép resend nếu bản ghi này làm thay đổi gói dữ liệu cuối cùng của chu kỳ.
- Có chức năng kiểm tra log kỹ thuật.
- Có chức năng theo dõi danh sách giao dịch đẩy sang HIS lỗi.
- Có chức năng resend thủ công từng giao dịch lỗi sang HIS.
- Với lỗi dữ liệu gửi HIS ở Phase I, giao dịch lỗi được log lại để theo dõi và hỗ trợ resend sau đó.
- Kiểm tra được thời gian truy xuất dữ liệu khớp với thời gian cấu hình.
- Nếu `Find Patient` thành công nhưng Monitor không hiển thị đủ thông tin hành chính tối thiểu gồm `Mã hồ sơ`, `Họ tên`, `Ngày sinh`, `Giới tính` thì không được đo và không được ghi nhận dữ liệu sang HIS.
- Trong cùng chu kỳ `10 phút` của Phase I, nếu có nhiều bản tin đo thì Tool phải gom thành 1 gói dữ liệu duy nhất gửi sang HIS; với từng chỉ số, chỉ giá trị hợp lệ cuối cùng mới được chọn vào gói dữ liệu đó.
- Với lỗi dữ liệu gửi HIS ở Phase I, bản ghi phải được đưa vào danh sách theo dõi lỗi để người dùng kỹ thuật thực hiện resend sau đó.
- Nếu sai `Mã máy` nhưng HIS vẫn nhận dữ liệu thì bản ghi không vào danh sách resend, chỉ xử lý theo checklist đối soát cấu hình.
- Với lỗi kết nối ở Phase I, hệ thống phải retry tự động đủ `3 lần`; nếu vẫn không gửi được thì bản ghi mới được đưa vào danh sách theo dõi lỗi để CNTT / Triển khai thực hiện resend.
- Nếu không `Find Patient` được Người bệnh thì Điều dưỡng có thể chuyển `New Patient` trên Monitor để đo dữ liệu trắng; dữ liệu này không tự gắn với Người bệnh trên HIS, không gửi sang HIS, nhưng phải lưu tại ISOFHTool dưới dạng log kỹ thuật và bảng lỗi với `Lý do lỗi = Không có Mã hồ sơ`.

### UAT bổ sung cho Phase II

- Có Dashboard trên `SAKURA` để Điều dưỡng hoặc Bác sĩ xem danh sách Người bệnh thuộc phạm vi phụ trách.
- Dashboard hiển thị được tối thiểu `Mã hồ sơ`, thông tin nhận diện Người bệnh, thời điểm đo sinh hiệu gần nhất, trạng thái tổng quan và dấu hiệu bất thường theo rule hiển thị đã chốt.
- Dashboard chỉ hiển thị Người bệnh thuộc phạm vi phụ trách hoặc phạm vi được phân quyền của người dùng đăng nhập.
- Từ Dashboard, người dùng nhận biết được Người bệnh cần chú ý và mở được màn hình chi tiết phù hợp để xem tiếp dữ liệu sinh hiệu.
- Dữ liệu hiển thị trên Dashboard phải bám dữ liệu sinh hiệu đã được HIS ghi nhận; không lấy các bản ghi lỗi hoặc dữ liệu trắng chưa gắn Người bệnh để hiển thị như dữ liệu hợp lệ.

## 16. Hạng mục cần tách xuống detail level

| Hạng mục | Có cần detail không | Tài liệu đích | Ghi chú |
| --- | --- | --- | --- |
| Màn hình theo dõi transaction lỗi và bản ghi lỗi | Có | SRS detail - Dashboard vận hành thiết bị PVM-2701 | Cần mô tả bộ lọc, trạng thái giao dịch gửi HIS, lý do lỗi, thao tác resend |
| Dashboard tổng quan Người bệnh phụ trách trên `SAKURA` | Có | SRS detail - Dashboard tổng quan sinh hiệu trên `SAKURA` | Cần mô tả nguồn xác định Người bệnh phụ trách, bộ chỉ số hiển thị, rule gắn cờ bất thường, cách điều hướng sang màn hình chi tiết |
| Màn hình cấu hình chu kỳ ghi nhận sinh hiệu | Có | SRS detail - Cấu hình chu kỳ ghi nhận sinh hiệu | Cần mô tả `Thiết lập chung`, khai báo riêng theo từng Người bệnh ở Phase II và quyền thao tác |
| API tìm Người bệnh và API gửi dữ liệu sinh hiệu | Có | Tài liệu detail tích hợp / mapping | Cần mô tả payload, mã lỗi, timeout, retry |
| Danh sách ghép `Chỉ số sống SAKURA - Chỉ số sống Monitor` | Có | Tài liệu detail mapping chỉ số sống | Cần mô tả mapping cho `05` chỉ số Phase I, quy tắc chuẩn hóa đơn vị và hướng mở rộng Phase II |
| Mapping `Mã máy` với cấu hình thiết bị trên Tool và Danh mục Mã máy trên HIS | Có | Tài liệu detail mapping `Mã máy` | Cần mô tả cấu hình IP, trường nhập `Mã máy`, quy trình kiểm tra sau cấu hình |
| Rule lọc bản ghi theo chu kỳ | Có | SRS detail - Rule lọc sinh hiệu theo chu kỳ | Cần mô tả rõ gói dữ liệu cuối cùng, chỉ số bị bỏ qua, log kỹ thuật |
| Rule trạng thái giao dịch gửi HIS và resend | Có | SRS detail - Theo dõi transaction lỗi gửi HIS | Cần mô tả lỗi kết nối, số lần retry mặc định, resend thủ công theo từng giao dịch lỗi và phạm vi resend theo lô nếu phát sinh ở phase sau |
| Phân quyền màn hình nghiệp vụ và màn hình kỹ thuật | Có | SRS detail - Phân quyền dashboard và dữ liệu sinh hiệu | Cần mô tả chi tiết vai trò được xem dữ liệu và vai trò được xem log |
| Hướng dẫn vận hành ngoại lệ | Có | Tài liệu HDSD / tài liệu vận hành pilot | Cần có tình huống `Find Patient` lỗi, sai Người bệnh, Monitor giữ Người bệnh cũ, chuyển sang nhập tay |

## 17. Rủi ro và câu hỏi mở

| STT | Loại | Nội dung | Mức độ | Hướng xử lý / Người phụ trách |
| --- | --- | --- | --- | --- |
| 1 | Rủi ro | Mô hình 1 máy trung tâm có thể là điểm lỗi đơn trong giai đoạn pilot. | Cao | Theo dõi trong pilot, đánh giá mở rộng kiến trúc sau khi ổn định. Phụ trách: Đội kỹ thuật / vận hành |
| 2 | Rủi ro | Hạ tầng mạng không ổn định có thể gây chậm hoặc mất kết nối giữa Monitor, Tool và HIS. | Cao | Khảo sát mạng, bổ sung log kỹ thuật để đối soát khi phát sinh lỗi gửi HIS. Phụ trách: CNTT Cơ sở y tế / đội triển khai |
| 3 | Rủi ro | Nếu cấu hình `Mã máy` sai, HIS vẫn nhận dữ liệu nhưng không truy vết đúng thiết bị, dễ gây khó đối soát. | Cao | Checklist kiểm tra mapping `Mã máy` ngay sau cấu hình Tool và test dữ liệu đầu vào. Phụ trách: CNTT / Triển khai / BA |
| 4 | Rủi ro | Khi HIS hoặc Tool lỗi ở bước tra cứu Người bệnh, Cơ sở y tế phải chuyển sang nhập tay, dễ phát sinh thao tác kép. | Vừa | Chuẩn hóa tài liệu vận hành và hướng dẫn người dùng trước go-live. Phụ trách: BA / Triển khai |
| 5 | Rủi ro | Phase I chưa có user-level permission trên ISOFHTool nên khả năng kiểm soát ai xem log kỹ thuật và ai thao tác resend phụ thuộc vào việc quản lý máy server dùng riêng. | Vừa | Kiểm soát vật lý và tài khoản truy cập máy server; đánh giá bổ sung phân quyền chi tiết ở phase sau. Phụ trách: CNTT / Triển khai / PO |
| 6 | Assumption cần xác nhận | Một máy trung tâm trong pilot dự kiến phục vụ `05` thiết bị và được định hướng nhỏ hơn `10` Monitor. | Vừa | Xác nhận lại với MEDI Plus trước triển khai. Phụ trách: Triển khai / CNTT |
| 7 | Assumption cần xác nhận | Khi máy trung tâm dừng hoạt động, cần xác nhận Monitor có khả năng lưu dữ liệu hay không. | Cao | Làm việc với hãng thiết bị và Cơ sở y tế để chốt phương án dự phòng. Phụ trách: Triển khai / Đơn vị thiết bị / CNTT |
| 8 | Câu hỏi mở | Thiết bị pilot có hỗ trợ chỉ số `Nhiệt độ` để đưa vào phạm vi `05` chỉ số sống Phase I hay không. | Vừa | Xác nhận lại với đơn vị thiết bị và MEDI Plus trước khi chốt detail mapping. Phụ trách: BA / Triển khai / Đơn vị thiết bị |
| 9 | Câu hỏi mở | Ticket / CR / Jira liên quan là gì. | Thấp | Bổ sung thông tin hành chính tài liệu. Phụ trách: BA / PO |
| 10 | Câu hỏi mở | Thời gian giữ log kỹ thuật và bảng lỗi của dữ liệu trắng hoặc bản ghi lỗi trên ISOFHTool là bao lâu. | Vừa | Chốt cùng đội triển khai và CNTT trước khi ban hành tài liệu vận hành pilot. Phụ trách: BA / Triển khai / CNTT |
| 11 | Câu hỏi mở | Rule xác định `Người bệnh mình phụ trách` trên Dashboard `SAKURA` sẽ theo Bác sĩ điều trị, Điều dưỡng phụ trách, khoa/phòng, buồng/giường hay ca trực. | Cao | Chốt với nghiệp vụ và đội sản phẩm trước khi xuống detail. Phụ trách: BA / PO / Cơ sở y tế |
| 12 | Câu hỏi mở | Rule gắn cờ `bất thường` trên Dashboard `SAKURA` dùng ngưỡng chung, ngưỡng theo khoa/phòng hay ngưỡng riêng theo Người bệnh. | Cao | Chốt rule hiển thị và cấu hình trước khi thiết kế Dashboard chi tiết. Phụ trách: BA / PO / Chuyên môn |

## 18. Tài liệu tham chiếu

- Yêu cầu đầu vào: `2026-05-01_NV_Tich hop thiet bi PVM-2701 vao HIS - Phase I.md`
- Yêu cầu đầu vào: `2026-05-02_NV_Tich hop thiet bi PVM-2701 vao HIS - Phase II.md`
- Q&A chốt nghiệp vụ: `2026-05-02_QA_Chot nghiep vu_Tich hop thiet bi PVM-2701 vao HIS.md`
- Template high level: `SRS Template High level` - https://confluence.isofh.com.vn/pages/viewpage.action?pageId=226035168
- Tài liệu detail level liên quan: [Cần bổ sung sau]
- Jira / CR: [Cần xác nhận]
- Figma / mockup: [Cần xác nhận]
- Test case / UAT: [Cần bổ sung sau]
- Tài liệu tích hợp / mapping: [Cần bổ sung sau]

## 19. Checklist BA trước khi chốt

- [x] Đã tách rõ nội dung nào là fact, nội dung nào là assumption.
- [x] Đã chỉ ra yêu cầu là dùng chung, cấu hình, tích hợp hay đặc thù.
- [x] Đã đánh giá khả năng giải bằng cấu hình trước khi đề xuất làm riêng.
- [x] Đã xem xét tác động đến phân hệ liên quan, dữ liệu, tích hợp, phân quyền và truy vết.
- [x] Đã đủ rõ để Dev, QA và triển khai hiểu thống nhất ở mức high level.
- [x] Đã xác định phần nào cần tách xuống tài liệu detail level.

## Điểm cần xác nhận trước khi triển khai

- Ticket / CR / Jira liên quan.
- Xác nhận số lượng Monitor thực tế trong pilot tại MEDI Plus.
- Xác nhận thiết bị pilot có hỗ trợ chỉ số `Nhiệt độ` hay không.
- Xác nhận khả năng lưu dữ liệu của Monitor khi máy trung tâm dừng hoạt động.
- Chốt thời gian giữ log kỹ thuật và bảng lỗi trên ISOFHTool.
