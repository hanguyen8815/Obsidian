---
loai_note: use-case
vai_tro: BA
trang_thai: draft
ngay_tao: 2026-05-01
tags:
  - ba
  - use-case
  - ket-noi-thiet-bi
  - pvm-2701
  - his
---

# Use case - Tích hợp thiết bị PVM-2701 vào HIS - Phase I

## Mục tiêu

- Tự động lấy thông tin Người bệnh từ HIS lên Monitor để giảm nhập tay.
- Tự động ghi nhận dữ liệu sinh hiệu từ Monitor vào HIS.
- Giảm sai sót khi ghép nhầm Người bệnh.
- Có cơ chế theo dõi lỗi, gửi lại dữ liệu và truy vết khi phát sinh sự cố.
- Triển khai pilot trên phạm vi giới hạn để đánh giá trước khi mở rộng.

## Phạm vi

- Trong phạm vi Phase I:
- Kết nối Monitor PVM-2701 với HIS thông qua 1 Tool trung gian.
- Tool cài trên 1 máy tính làm server trung tâm.
- Tool nhận yêu cầu tìm Người bệnh từ Monitor và gọi API HIS để trả thông tin Người bệnh về Monitor.
- Tool nhận dữ liệu sinh hiệu từ Monitor, áp rule lọc thời gian tại Tool, sau đó gửi dữ liệu sang HIS.
- Tool ghi log kỹ thuật, theo dõi trạng thái bản ghi, hỗ trợ tự động gửi lại và gửi lại bằng tay.
- Ngoài phạm vi Phase I:
- Dashboard theo dõi chỉ số sống.
- Cấu hình nâng cao trên giao diện quản trị.
- Mở rộng nhiều server hoặc kiến trúc dự phòng.
- Hiển thị Mã máy trực tiếp trên bảng nghiệp vụ sinh hiệu của HIS.
- Các rule theo dõi nâng cao theo từng giai đoạn điều trị.

## Hiện trạng đã biết

- Người dùng thao tác trên Monitor theo quy trình:
- Kiểm tra Người bệnh đang hiển thị trên Monitor.
- Nếu đang có Người bệnh cũ thì chọn đổi sang Người bệnh mới.
- Quét barcode Mã hồ sơ.
- Chọn `Find Patient` để Monitor lấy thông tin Người bệnh từ HIS.
- Xác nhận đúng Người bệnh.
- Thực hiện đo sinh hiệu.
- Trong HIS hiện tại, `Mã hồ sơ` dùng cho tích hợp này là mã tương ứng duy nhất với `1 mã vào viện / 1 lượt khám / 1 đợt điều trị`.
- HIS đã có cơ chế chống trùng: cùng `1 Mã hồ sơ` và `1 thời điểm đo` chỉ ghi nhận `1 bản ghi sinh hiệu`.
- Phase I được triển khai theo mô hình pilot với 1 server trung tâm và một số Monitor trong phạm vi giới hạn.

## Giả định cần xác minh

- Monitor PVM-2701 hỗ trợ gửi yêu cầu tìm Người bệnh và gửi dữ liệu đo để Tool có thể tiếp nhận.
- Tool có thể nhận được thời điểm đo từ Monitor để làm mốc lọc và gửi sang HIS.
- Hạ tầng mạng tại Cơ sở y tế cho phép server trung tâm kết nối ổn định tới các Monitor trong phạm vi pilot.
- Các chỉ số tích hợp trong Phase I đều có trong bản tin thực tế của Monitor.
- Tool có thể nhận diện được thiết bị tối thiểu theo IP, tên máy, serial hoặc mã cấu hình để phục vụ truy vết.

## Tác nhân

- Tác nhân chính: Điều dưỡng hoặc Nhân viên y tế thực hiện đo sinh hiệu.
- Tác nhân liên quan:
- Monitor PVM-2701.
- Tool trung gian.
- HIS.
- Nhân viên CNTT của Cơ sở y tế.
- Đội triển khai HIS.
- Đơn vị hỗ trợ thiết bị khi cần.

## Điều kiện đầu vào

- Người bệnh đã có `Mã hồ sơ` hợp lệ trong HIS.
- Monitor sẵn sàng đo.
- Tool đang hoạt động và kết nối được với HIS.
- Monitor kết nối được tới Tool trong cùng phạm vi mạng đã cấu hình.

## Trigger

- Nhân viên y tế quét barcode `Mã hồ sơ` của Người bệnh và chọn `Find Patient` trên Monitor.

## Luồng chính

1. Nhân viên y tế kiểm tra trên Monitor đang có thông tin Người bệnh cũ hay không.
2. Nếu có, Nhân viên y tế thao tác chuyển Monitor sang ngữ cảnh Người bệnh mới.
3. Nhân viên y tế quét barcode `Mã hồ sơ` của Người bệnh.
4. Nhân viên y tế chọn chức năng `Find Patient` trên Monitor.
5. Monitor gửi yêu cầu tìm Người bệnh sang Tool.
6. Tool gọi API HIS để lấy thông tin Người bệnh theo `Mã hồ sơ`.
7. HIS trả thông tin Người bệnh cho Tool.
8. Tool phản hồi lại Monitor để Monitor hiển thị thông tin Người bệnh.
9. Nhân viên y tế kiểm tra và xác nhận đúng Người bệnh trước khi đo.
10. Nhân viên y tế thực hiện đo sinh hiệu cho Người bệnh.
11. Monitor gửi dữ liệu sinh hiệu sang Tool sau khi đo xong.
12. Tool tiếp nhận dữ liệu, lưu log và áp rule lọc thời gian của Phase I.
13. Nếu bản ghi đủ điều kiện gửi, Tool gọi API HIS để ghi nhận sinh hiệu.
14. HIS xử lý dữ liệu và trả kết quả thành công hoặc lỗi.
15. Tool cập nhật trạng thái bản ghi tương ứng.
16. Nhân viên y tế tiếp tục thao tác cho Người bệnh tiếp theo.

## Ngoại lệ

1. Không tìm thấy Người bệnh theo `Mã hồ sơ`:
   Monitor không hiển thị được thông tin Người bệnh. Nhân viên y tế kiểm tra lại barcode hoặc thông tin Người bệnh trên HIS.
2. Tool không kết nối được HIS khi tìm Người bệnh:
   Tool ghi log lỗi. Monitor không nhận được kết quả tìm Người bệnh. Chưa được đo cho đến khi xác nhận lại đúng Người bệnh.
3. Tool nhận dữ liệu sinh hiệu nhưng HIS đang lỗi:
   Tool ghi nhận lỗi đẩy HIS. Bản ghi được đưa vào danh sách chờ gửi lại. Tool tự động gửi lại theo cấu hình.
4. Dữ liệu không đủ điều kiện gửi vì rule thời gian:
   Tool không gửi sang HIS nhưng vẫn lưu log để tra cứu.
5. HIS từ chối do trùng bản ghi:
   Tool cập nhật trạng thái tương ứng và không gửi lặp không kiểm soát.

## Điều kiện đầu ra

- Nếu thành công:
- Thông tin Người bệnh hiển thị đúng trên Monitor.
- Dữ liệu sinh hiệu được ghi nhận vào HIS.
- Nếu lỗi:
- Có log để truy vết.
- Có trạng thái bản ghi rõ ràng.
- Có thể tự động gửi lại hoặc gửi lại bằng tay.

## Business rules

- BR1: `Mã hồ sơ` dùng cho tích hợp là mã định danh duy nhất của đúng 1 lượt điều trị đang đo.
- BR2: Chỉ được đo và ghi nhận dữ liệu khi Monitor đã lấy đúng thông tin Người bệnh từ HIS.
- BR3: Tool là nơi áp rule thời gian trong Phase I để giảm số lượng bản ghi gửi sang HIS.
- BR4: Tool phải quản lý trạng thái bản ghi theo từng bước xử lý.
- BR5: HIS chống trùng theo `Mã hồ sơ + thời điểm đo`.
- BR6: Tool phải quản lý được bản ghi đã gửi thành công, bản ghi lỗi và bản ghi chờ gửi lại.
- BR7: Bản ghi bị loại do rule thời gian không gửi sang HIS nhưng vẫn phải lưu log kỹ thuật.
- BR8: Khi HIS lỗi tạm thời, Tool không được làm mất dữ liệu đã nhận từ Monitor.
- BR9: Chỉ tích hợp các chỉ số đã được xác nhận có trong bản tin thực tế của Monitor.
- BR10: Việc gửi lại dữ liệu không được làm phát sinh trùng bản ghi trên HIS.

## Phân quyền

- Ai được thực hiện:
- Điều dưỡng hoặc Nhân viên y tế được phép quét mã, tìm Người bệnh và đo sinh hiệu trên Monitor.
- Tool tự động tiếp nhận và gửi dữ liệu sang HIS.
- Ai được xem / duyệt / hủy:
- Nhân viên CNTT hoặc người được phân quyền được xem log, theo dõi lỗi và thực hiện gửi lại bản ghi lỗi trên Tool.
- HIS xử lý chống trùng theo rule hệ thống, không yêu cầu người dùng duyệt tay trong Phase I.

## Audit log / thông báo

- Cần ghi log gì:
- Thời điểm nhận yêu cầu tìm Người bệnh.
- Mã hồ sơ tra cứu.
- Kết quả HIS trả về.
- Thời điểm nhận bản tin sinh hiệu.
- Dữ liệu parse được.
- Kết quả lọc theo rule thời gian.
- Trạng thái đẩy HIS.
- Nội dung lỗi nếu có.
- Số lần gửi lại.
- Thông tin nhận diện thiết bị gồm tên máy hoặc mã máy, model, IP hoặc serial nếu có.
- Cần thông báo cho ai:
- Nhân viên CNTT hoặc người được phân quyền khi có danh sách bản ghi lỗi cần xử lý.

## Dữ liệu liên quan

- Nguồn dữ liệu:
- Monitor PVM-2701.
- HIS.
- Tool trung gian.
- Trường dữ liệu chính:
- Mã hồ sơ.
- Thời điểm đo.
- Nhiệt độ.
- Mạch hoặc nhịp tim.
- Huyết áp tâm thu.
- Huyết áp tâm trương.
- Huyết áp trung bình nếu Monitor có gửi.
- Nhịp thở.
- SpO2.
- Thông tin nhận diện thiết bị nếu có.

## Trạng thái xử lý bản ghi tại Tool

- Mới nhận.
- Không đủ điều kiện gửi.
- Chờ gửi HIS.
- Đã gửi thành công.
- Gửi lỗi.
- Chờ gửi lại.
- Đã gửi lại.

## Acceptance criteria

- AC1: Khi quét đúng barcode `Mã hồ sơ`, Monitor lấy được đúng thông tin Người bệnh từ HIS.
- AC2: Nhân viên y tế có thể xác nhận đúng Người bệnh trên Monitor trước khi đo.
- AC3: Sau khi đo xong, Tool nhận được dữ liệu sinh hiệu từ Monitor.
- AC4: Tool áp được rule lọc thời gian trước khi gửi dữ liệu sang HIS.
- AC5: HIS nhận và ghi nhận được dữ liệu sinh hiệu hợp lệ từ Tool.
- AC6: Khi HIS lỗi tạm thời, bản ghi không bị mất và được đưa vào danh sách chờ gửi lại.
- AC7: Tool hỗ trợ tự động gửi lại và gửi lại bằng tay.
- AC8: Gửi lại không làm phát sinh bản ghi trùng trên HIS.
- AC9: Có thể tra cứu log theo thời gian, Người bệnh, trạng thái gửi và thiết bị.
- AC10: Các chỉ số ghi nhận trên HIS khớp với dữ liệu đo từ Monitor trong ca kiểm thử.

## Rủi ro / open questions

- Rủi ro:
- Server trung tâm là điểm lỗi đơn trong Phase I.
- Hạ tầng mạng không ổn định có thể làm chậm hoặc lỗi gửi dữ liệu.
- Rule lọc thời gian tại Tool có thể chưa tối ưu trong giai đoạn đầu pilot.
- Bản tin thực tế từ Monitor có thể khác giả định hiện tại và cần điều chỉnh mapping.
- Open questions:
- Tool có nhận được message ID hoặc sequence từ Monitor để tăng độ chắc cho quản lý resend hay không.
- Rule lọc thời gian sẽ lấy bản ghi đầu hay bản ghi cuối trong một chu kỳ.
- Có lưu toàn bộ dữ liệu thô tại Tool để phục vụ đối soát sau go-live hay không.
- Khi mở rộng số lượng Monitor, có cần tách thêm server hoặc collector theo vùng mạng hay không.
