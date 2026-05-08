---
loai_note: use-case
vai_tro: BA
trang_thai: draft
ngay_tao: 2026-05-02
tags:
  - ba
  - use-case
  - ket-noi-thiet-bi
  - pvm-2701
  - his
  - phase-ii
---

# Use case - Tích hợp thiết bị PVM-2701 vào HIS - Phase II

## Mục tiêu

- Mở rộng từ mô hình pilot của Phase I sang mô hình vận hành ổn định hơn.
- Cho phép cấu hình chu kỳ ghi nhận sinh hiệu theo từng Người bệnh hoặc từng giai đoạn điều trị.
- Bổ sung truy vết thiết bị đo ngay trên HIS.
- Cung cấp màn hình theo dõi dữ liệu sinh hiệu nhận từ thiết bị và trạng thái kết nối.
- Tăng khả năng quản trị, đối soát và xử lý lỗi khi số lượng thiết bị tăng.

## Phạm vi

- Trong phạm vi Phase II:
- HIS cho phép cấu hình chu kỳ ghi nhận sinh hiệu theo từng Người bệnh hoặc theo rule áp dụng.
- API HIS trả thêm thông tin chu kỳ ghi nhận sinh hiệu cho Tool.
- Tool nhận cấu hình chu kỳ từ HIS và áp dụng khi chọn bản ghi gửi sang HIS.
- HIS bổ sung trường `Mã máy` tại dữ liệu sinh hiệu để truy vết thiết bị đo.
- Bổ sung danh mục thiết bị đo để quản lý mã máy, model, serial, IP, vị trí dùng.
- Bổ sung màn hình theo dõi trạng thái kết nối thiết bị và trạng thái nhận dữ liệu.
- Bổ sung màn hình tra cứu dữ liệu nhận từ Tool, bản ghi lỗi, bản ghi chờ gửi lại và lịch sử gửi lại.
- Mở rộng cấu hình retry, resend, cảnh báo kết nối lỗi và rule đồng bộ lại.
- Ngoài phạm vi Phase II:
- Phân tích dữ liệu chuyên sâu hoặc cảnh báo lâm sàng nâng cao.
- Tự động ra y lệnh hoặc tự động thay đổi phác đồ từ dữ liệu sinh hiệu.
- Tích hợp nhiều dòng Monitor khác nếu chưa khảo sát riêng.

## Hiện trạng đã biết

- Phase I đã chốt mô hình Tool trung gian kết nối Monitor với HIS.
- Tool đang nhận dữ liệu từ Monitor, lọc theo rule thời gian và gửi bản ghi đủ điều kiện sang HIS.
- HIS đang chống trùng theo `Mã hồ sơ + thời điểm đo`.
- Phase I mới ưu tiên truy vết thiết bị trên Tool, chưa đưa `Mã máy` xuống bảng nghiệp vụ sinh hiệu của HIS.
- Nhu cầu thực tế là chu kỳ theo dõi sinh hiệu có thể thay đổi theo tình trạng Người bệnh.
- Ví dụ:
- Trong lúc mổ: 10 phút/lần.
- Sau mổ: 30 phút/lần.
- Sau 18 giờ: 1 giờ/lần.

## Giả định cần xác minh

- Tool hiện tại có thể nhận cấu hình chu kỳ từ HIS mà không phải thay đổi lớn kiến trúc.
- Dữ liệu từ Monitor có đủ thông tin thời điểm đo để Tool áp đúng rule chu kỳ.
- HIS có thể xác định được chu kỳ đang áp dụng cho từng Người bệnh tại từng thời điểm.
- Mỗi bản ghi sinh hiệu có thể gắn được với đúng thiết bị đo để lưu `Mã máy`.
- Có đủ dữ liệu đầu vào để xác định trạng thái kết nối thiết bị như `last seen`, `last measurement`, `last push status`.

## Tác nhân

- Tác nhân chính:
- Điều dưỡng hoặc Nhân viên y tế theo dõi sinh hiệu.
- Tác nhân liên quan:
- Bác sĩ xem dữ liệu sinh hiệu đã ghi nhận.
- Nhân viên CNTT hoặc người quản trị theo dõi kết nối thiết bị.
- Đội triển khai, hỗ trợ vận hành và xử lý lỗi tích hợp.
- Quản lý khoa/phòng hoặc đầu mối vận hành khi cần theo dõi tình trạng kết nối.
- Monitor PVM-2701.
- Tool trung gian.
- HIS.

## Điều kiện đầu vào

- Phase I đã vận hành ổn định ở mức cơ bản.
- Monitor, Tool và HIS đang kết nối được với nhau.
- Người bệnh đã có `Mã hồ sơ` hợp lệ.
- Danh mục thiết bị đo đã được khai báo tối thiểu trên hệ thống.

## Trigger

- Người dùng cấu hình chu kỳ ghi nhận sinh hiệu cho Người bệnh hoặc Tool gọi API HIS để lấy cấu hình khi ghép Người bệnh trên Monitor.

## Luồng chính

1. Người dùng thực hiện ghép Người bệnh trên Monitor như Phase I.
2. Tool gọi API HIS để lấy thông tin Người bệnh.
3. HIS trả về thông tin Người bệnh kèm cấu hình chu kỳ ghi nhận sinh hiệu đang áp dụng.
4. Tool lưu chu kỳ ghi nhận sinh hiệu của Người bệnh đó để dùng trong quá trình xử lý bản ghi.
5. Nhân viên y tế đo sinh hiệu cho Người bệnh.
6. Monitor gửi bản tin sinh hiệu sang Tool.
7. Tool nhận bản tin, đối chiếu chu kỳ đang áp dụng, chọn bản ghi đủ điều kiện gửi sang HIS.
8. Tool gửi dữ liệu sinh hiệu sang HIS kèm `Mã máy` hoặc thông tin định danh thiết bị.
9. HIS ghi nhận dữ liệu sinh hiệu, lưu `Mã máy` và các thông tin truy vết liên quan.
10. HIS cập nhật màn hình sinh hiệu và các màn hình theo dõi kết nối.
11. Nếu bản ghi lỗi, Tool và HIS ghi nhận trạng thái để người quản trị theo dõi và gửi lại khi cần.

## Luồng cấu hình chu kỳ theo dõi

1. Người dùng được phân quyền mở hồ sơ hoặc màn hình theo dõi sinh hiệu của Người bệnh.
2. Người dùng nhập hoặc chọn `Thời gian ghi nhận sinh hiệu` theo phút.
3. Nếu không nhập riêng cho Người bệnh, hệ thống áp dụng giá trị mặc định theo cấu hình chung hoặc theo khoa/phòng nếu có.
4. HIS lưu giá trị chu kỳ đang áp dụng.
5. Khi Tool gọi API lấy thông tin Người bệnh, HIS trả thêm trường chu kỳ này.
6. Tool dùng giá trị đó để quyết định bản ghi nào được gửi sang HIS.

## Luồng theo dõi kết nối thiết bị

1. Tool định kỳ cập nhật trạng thái kết nối thiết bị về HIS hoặc HIS chủ động lấy trạng thái từ Tool.
2. HIS hiển thị danh sách thiết bị, trạng thái kết nối, thời điểm nhận dữ liệu gần nhất, trạng thái gửi gần nhất.
3. Nếu quá thời gian quy định không nhận được dữ liệu từ thiết bị đang được đánh dấu theo dõi, hệ thống chuyển trạng thái và cảnh báo cho người quản trị.
4. Người quản trị tra cứu danh sách thiết bị lỗi, bản ghi lỗi và thực hiện gửi lại nếu cần.

## Ngoại lệ

1. HIS không trả về chu kỳ ghi nhận:
   Tool dùng giá trị mặc định đã cấu hình chung.
2. Tool nhận bản tin nhưng không map được `Mã máy`:
   Bản ghi có thể chuyển vào danh sách chờ xử lý hoặc vẫn nhận nhưng gắn trạng thái thiếu thông tin thiết bị, tùy rule chốt.
3. Tool nhận dữ liệu cũ sau thời gian gián đoạn:
   Tool áp lại rule chu kỳ và rule chống trùng trước khi gửi HIS.
4. Thiết bị mất kết nối kéo dài:
   HIS chuyển trạng thái thiết bị và hiển thị cảnh báo để theo dõi.
5. Người dùng đổi chu kỳ theo dõi trong lúc đang kết nối:
   Hệ thống phải chốt rõ rule có hiệu lực từ thời điểm lưu mới, không hồi tố cho bản ghi cũ.

## Điều kiện đầu ra

- Nếu thành công:
- HIS ghi nhận dữ liệu sinh hiệu theo đúng chu kỳ áp dụng.
- HIS lưu được `Mã máy` để truy vết thiết bị.
- Người quản trị theo dõi được trạng thái thiết bị và trạng thái bản ghi.
- Nếu lỗi:
- Có log và trạng thái để tra cứu nguyên nhân.
- Có thể xử lý resend, retry và đối soát bản ghi lỗi.

## Business rules

- BR1: Chu kỳ ghi nhận sinh hiệu là rule nghiệp vụ có thể cấu hình, không hard-code theo từng Cơ sở y tế.
- BR2: Chu kỳ ghi nhận có thể có giá trị mặc định chung và có thể được ghi đè cho từng Người bệnh.
- BR3: Khi có cấu hình riêng trên Người bệnh, Tool phải ưu tiên dùng cấu hình riêng thay cho cấu hình mặc định.
- BR4: Rule lọc theo thời gian phải dựa trên `thời điểm đo` của thiết bị, không dựa trên thời điểm Tool nhận được bản tin.
- BR5: Trong cùng một chu kỳ, phải chốt rõ lấy bản ghi đầu tiên hay bản ghi cuối cùng.
- BR6: `Mã máy` là dữ liệu bắt buộc để truy vết thiết bị đối với bản ghi sinh hiệu nhận từ Tool, trừ trường hợp được chấp nhận ngoại lệ có kiểm soát.
- BR7: HIS tiếp tục chống trùng theo `Mã hồ sơ + thời điểm đo`; nếu cần, Tool vẫn giữ khóa kỹ thuật riêng để quản lý resend.
- BR8: Nếu quá thời gian theo dõi mà không còn dữ liệu từ thiết bị, trạng thái kết nối của Người bệnh hoặc thiết bị phải được cập nhật theo rule cấu hình.
- BR9: Mọi thao tác sửa cấu hình chu kỳ, gửi lại dữ liệu, đổi trạng thái hoặc xử lý lỗi phải có log.
- BR10: Dữ liệu hiển thị trên dashboard chỉ là dữ liệu theo dõi vận hành, không thay thế cho rule ký, khóa hồ sơ và truy vết trong hồ sơ điều trị.

## Phân quyền

- Điều dưỡng hoặc Nhân viên y tế:
- Được đo sinh hiệu, xem dữ liệu của Người bệnh thuộc phạm vi được giao.
- Người dùng chuyên môn được phân quyền:
- Được cập nhật chu kỳ ghi nhận sinh hiệu cho Người bệnh.
- Nhân viên CNTT hoặc quản trị:
- Được xem dashboard vận hành, log thiết bị, log bản ghi lỗi, thực hiện gửi lại dữ liệu lỗi.

## Audit log / thông báo

- Cần ghi log gì:
- Ai thay đổi chu kỳ ghi nhận.
- Giá trị cũ, giá trị mới, thời điểm thay đổi.
- Thiết bị nào gửi bản ghi.
- Bản ghi nào bị loại do rule thời gian.
- Bản ghi nào gửi lỗi, gửi lại, gửi thành công.
- Thiết bị nào bị đánh dấu mất kết nối và khi nào hồi phục.
- Cần thông báo cho ai:
- Nhân viên CNTT hoặc người quản trị khi thiết bị lỗi kết nối, khi bản ghi gửi lỗi kéo dài hoặc khi vượt ngưỡng không nhận được dữ liệu.

## Dữ liệu liên quan

- Nguồn dữ liệu:
- Monitor PVM-2701.
- Tool trung gian.
- HIS.
- Trường dữ liệu chính:
- Mã hồ sơ.
- Thời gian ghi nhận sinh hiệu (phút).
- Trạng thái kết nối thiết bị đo sinh hiệu.
- Mã máy.
- Tên máy.
- Model.
- Serial.
- IP.
- Thời điểm đo.
- Nhiệt độ.
- Mạch hoặc nhịp tim.
- Huyết áp tâm thu.
- Huyết áp tâm trương.
- Huyết áp trung bình nếu Monitor có gửi.
- Nhịp thở.
- SpO2.

## Trạng thái cần theo dõi

- Trạng thái kết nối thiết bị:
- Chưa ghép Người bệnh.
- Đã ghép Người bệnh.
- Đang chờ dữ liệu đo.
- Đang nhận dữ liệu.
- Mất kết nối.
- Lỗi gửi HIS.
- Hoạt động bình thường.
- Trạng thái bản ghi sinh hiệu:
- Mới nhận.
- Không đủ điều kiện gửi theo rule thời gian.
- Chờ gửi HIS.
- Đã gửi thành công.
- Gửi lỗi.
- Chờ gửi lại.
- Đã gửi lại.

## Cấu hình cần có

- Cấu hình chu kỳ ghi nhận sinh hiệu mặc định.
- Cấu hình cho phép ghi đè chu kỳ theo từng Người bệnh.
- Cấu hình thời gian xác định mất kết nối thiết bị.
- Cấu hình số lần retry và khoảng chờ giữa các lần retry.
- Cấu hình rule hiển thị cảnh báo lỗi kết nối.
- Danh mục thiết bị đo gồm:
- Mã máy.
- Tên máy.
- Model.
- Serial.
- IP.
- Khoa/phòng.
- Giường hoặc vị trí dùng nếu có.
- Trạng thái hoạt động.
- Mapping giữa thiết bị vật lý và mã quản lý trên HIS.
- Cấu hình quyền ai được sửa chu kỳ, ai được xem log, ai được gửi lại dữ liệu lỗi.

## Tác động đến phân hệ liên quan

- Phân hệ sinh hiệu:
- Bổ sung `Mã máy`, chu kỳ ghi nhận, trạng thái kết nối và khả năng tra cứu tốt hơn.
- Phân hệ hồ sơ điều trị:
- Cần xác định rõ nơi nhập và xem chu kỳ theo dõi sinh hiệu của Người bệnh.
- Phân hệ danh mục:
- Cần thêm danh mục thiết bị đo.
- Phân hệ quản trị hệ thống:
- Cần màn hình cấu hình rule, trạng thái thiết bị và xử lý lỗi.
- Phân hệ báo cáo:
- Có thể phát sinh nhu cầu báo cáo thiết bị lỗi, tỷ lệ nhận dữ liệu thành công, số bản ghi bị loại theo rule.

## Acceptance criteria

- AC1: HIS cho phép khai báo chu kỳ ghi nhận sinh hiệu mặc định và chu kỳ riêng cho từng Người bệnh.
- AC2: API HIS trả được chu kỳ ghi nhận hiện hành cho Tool khi Tool lấy thông tin Người bệnh.
- AC3: Tool áp đúng rule chu kỳ khi chọn bản ghi gửi sang HIS.
- AC4: HIS lưu được `Mã máy` cùng bản ghi sinh hiệu nhận từ Tool.
- AC5: Có danh mục thiết bị đo để quản lý tối thiểu mã máy, model, serial hoặc IP.
- AC6: Có màn hình theo dõi trạng thái thiết bị và trạng thái gửi dữ liệu.
- AC7: Có thể tra cứu được bản ghi lỗi, bản ghi chờ gửi lại và lịch sử gửi lại.
- AC8: Khi quá thời gian không nhận được dữ liệu từ thiết bị, hệ thống cập nhật được trạng thái theo rule đã cấu hình.
- AC9: Khi đổi chu kỳ theo dõi của Người bệnh, Tool áp dụng được giá trị mới từ thời điểm có hiệu lực.
- AC10: Có audit log cho thay đổi cấu hình chu kỳ và thao tác xử lý lỗi.

## Rủi ro / open questions

- Rủi ro:
- Rule chu kỳ quá phức tạp có thể làm khó vận hành nếu không thiết kế màn hình rõ ràng.
- Nếu `Mã máy` không được map ổn định, truy vết trên HIS có thể sai hoặc thiếu.
- Dashboard nếu lấy dữ liệu trực tiếp từ nhiều nguồn có thể chậm hoặc lệch trạng thái.
- Khi số lượng thiết bị tăng nhanh, mô hình 1 server trung tâm có thể không còn phù hợp.
- Open questions:
- Chu kỳ ghi nhận sẽ cấu hình theo từng Người bệnh, theo khoa/phòng hay theo mẫu chăm sóc?
- Trong cùng một chu kỳ, chọn bản ghi đầu hay bản ghi cuối?
- Có cần lưu toàn bộ dữ liệu thô trên HIS hay chỉ lưu ở Tool?
- Trạng thái `Kết nối thiết bị đo sinh hiệu` nên để dạng `TRUE/FALSE` hay mở rộng thành nhiều trạng thái vận hành?
- Dashboard chỉ phục vụ CNTT và triển khai hay cần cho cả Điều dưỡng/Bác sĩ theo dõi?

## Note liên quan

- [[2026-05-01_NV_Tich hop thiet bi PVM-2701 vao HIS - Phase I]]
