# Biên bản họp tiến độ kết nối Monitor và App Nhân viên y tế

## Thông tin chung

- Thời gian: 14h00 - 14h45, 15/05/2026
- Hình thức: Họp nội bộ qua cuộc gọi nhóm theo transcript
- Thành phần tham gia:
  - Hà Nguyễn
  - Nam Mai Ngọc
  - Tuấn Kim Đình
- Người chủ trì: Hà Nguyễn
- Người ghi biên bản: AI tổng hợp từ transcript cuộc họp

## Mục tiêu buổi họp

- Rà soát tiến độ 2 việc chính đang theo dõi trong tháng 05/2026.
- Việc 1: `Kết nối Monitor`. Toàn bộ danh sách đầu việc kỹ thuật chi tiết đang nêu trong note này thuộc phạm vi công việc này.
- Việc 2: `Kết nối App Nhân viên y tế` tích hợp `faceID` và `CCCD` để phục vụ tiếp đón.
- Chốt phạm vi tối thiểu cần hoàn thành để kịp test và chuẩn bị triển khai tại `Medi Nam Định` vào đầu tháng 06/2026.
- Phân công đầu việc, deadline và cách theo dõi ticket.

## Chương trình trao đổi

- Kiểm tra tình trạng xử lý 2 đầu việc lớn đang bị chậm.
- Chốt phạm vi và các đầu việc thuộc nhánh `Kết nối Monitor`.
- Chốt hướng làm phần mapping chỉ số sống từ `Monitor` sang `Sakura`.
- Chốt cấu hình thiết bị `Monitor` và luồng giao tiếp `HL7`.
- Nhắc việc tách riêng đầu việc cho nhánh `App Nhân viên y tế` tích hợp `faceID` và `CCCD` để tiếp đón.
- Chốt kế hoạch test, fix bug, tạo ticket và chuẩn bị triển khai.

## Hai việc chính cần theo dõi

- Việc 1: `Kết nối Monitor`
  - Phạm vi: nhận bản tin từ `Monitor`, tìm Người bệnh, phản hồi `ADR A19`, nhận dữ liệu chỉ số sống, phản hồi `ACK`, map dữ liệu, gửi sinh hiệu lên `Sakura`, lưu log và hỗ trợ đẩy lại khi lỗi.
  - Ghi chú: toàn bộ các đầu việc kỹ thuật chi tiết hiện có trong phần `Kế hoạch tiếp theo` đang thuộc nhánh công việc này, trừ đầu việc liên quan `Nhân viên y tế`.
- Việc 2: `Kết nối App Nhân viên y tế` tích hợp `faceID` và `CCCD` để tiếp đón
  - Phạm vi đang biết: kết nối phần `App Nhân viên y tế` để hỗ trợ tiếp đón bằng `faceID` và `CCCD`.
  - Trạng thái hiện tại: Dự án `App NVYT` có PM dự án là anh `Vũ Đình Tùng`. Tuấn liên hệ anh Tùng để log ticket vào Dự án `App NVYT`. Nam xử lý lỗi tích hợp `faceID` quét `CCCD` trên `App Nhân viên y tế`.

## Nội dung chính

- Công việc 1: `Kết nối Monitor`
  - Nhánh công việc này hiện đang chậm so với mục tiêu tháng 05/2026, cần dồn ưu tiên để hoàn tất trong tuần kế tiếp.
  - Phần backend sẽ tận dụng `API` sẵn có của `Sakura`, không làm thêm backend riêng nếu không bắt buộc.
  - Sẽ bổ sung màn hình cấu hình mapping chỉ số sống giữa dữ liệu từ `Monitor` và các trường dữ liệu trên `Sakura`.
  - Mapping chỉ số phải cho phép thêm mới động trong quá trình sử dụng, không giới hạn cố định theo danh sách ban đầu.
  - Trước mắt ưu tiên các chỉ số tối thiểu đang cần trên màn hình khám:
    - Mạch
    - Nhiệt độ
    - Huyết áp
    - Nhịp thở
    - SpO2
  - `Chiều cao` và `Cân nặng` hiện chưa chắc lấy trực tiếp từ thiết bị đang có. Tạm thời chấp nhận nhập bổ sung, sau khi triển khai thử sẽ đánh giá lại cách tối ưu.
  - Cần bổ sung cấu hình danh sách thiết bị `Monitor`, tối thiểu gồm `IP` và `Port`.
  - Luồng tìm kiếm Người bệnh từ `Monitor` sẽ gọi `API` HIS để lấy thông tin và phản hồi lại bằng bản tin `ADR A19`.
  - Mỗi bản tin nhận từ `Monitor` phải phản hồi `ACK` để thiết bị tiếp tục gửi dữ liệu.
  - Sau khi nhận xong dữ liệu đo, hệ thống sẽ tổng hợp các giá trị cuối, map sang dữ liệu HIS rồi gửi lên `Sakura`.
  - Cần có log trạng thái gửi dữ liệu, màn hình theo dõi thành công hoặc thất bại và chức năng đẩy lại khi lỗi.
- Công việc 2: `Kết nối App Nhân viên y tế` tích hợp `faceID` và `CCCD` để tiếp đón
  - Cần theo dõi thành một nhánh công việc riêng để tránh gộp chung vào các task của `Monitor`.
  - PM dự án là anh `Vũ Đình Tùng`.
  - Tuấn liên hệ anh Tùng để log ticket vào Dự án `App NVYT`.

## Điểm đã thống nhất

- Cuộc họp đang theo dõi 2 việc chính riêng biệt: `Kết nối Monitor` và `Kết nối App Nhân viên y tế` tích hợp `faceID` và `CCCD` để tiếp đón.
- Toàn bộ các đầu việc kỹ thuật chi tiết hiện đang liệt kê trong note này chủ yếu thuộc nhánh `Kết nối Monitor`.
- Giảm phạm vi theo hướng làm tối thiểu nhưng chạy được thực tế, sau đó mới tối ưu thêm.
- Dùng lại `API` của `Sakura` cho luồng lấy thông tin Người bệnh.
- Làm cấu hình mapping chỉ số sống theo hướng mở rộng được trong quá trình vận hành.
- Làm cấu hình danh sách thiết bị `Monitor` theo `IP` và `Port`.
- Chuẩn phản hồi tìm kiếm Người bệnh sẽ dùng `ADR A19`.
- Bắt buộc phản hồi `ACK` cho từng bản tin dữ liệu từ `Monitor`.
- Thời gian tự động kết thúc phiên đo thống nhất là `10 phút`. Sau triển khai sẽ đánh giá lại.
- Đầu tháng 06/2026 là mốc mục tiêu để triển khai tại `Medi Nam Định`.

## Quyết định

- Tạo 1 ticket tổng cho nhánh `Kết nối Monitor`.
- Tách 6 subtask kỹ thuật cho phần `Sakura` thuộc nhánh `Kết nối Monitor`.
- Với nhánh `App Nhân viên y tế`, Tuấn liên hệ anh `Vũ Đình Tùng` để log ticket vào Dự án `App NVYT` và Nam xử lý lỗi tích hợp `faceID` quét `CCCD`.
- Tuấn Kim Đình hoàn thiện tài liệu nghiệp vụ chi tiết phần kết nối `Monitor` để team bám theo khi làm và test.

## Điểm chưa rõ hoặc cần xác minh thêm

- Chưa có đủ thiết bị thực tế để kiểm tra hết các loại chỉ số có thể phát sinh từ `Monitor`.
- Thời gian tự động kết thúc phiên đo thống nhất là `10 phút`. Sau triển khai sẽ đánh giá lại tính phù hợp.
- Cần kiểm tra thực tế tại `Medi Nam Định` để quyết định luồng nhập bổ sung `Chiều cao`, `Cân nặng`.
- Nam trao đổi với Minh để học hỏi kinh nghiệm về chuẩn `HL7/MLLP`, tránh thiếu bước phản hồi khiến thiết bị dừng gửi dữ liệu.
- Với nhánh `App Nhân viên y tế`, chưa có chi tiết về luồng tiếp đón, cách dùng `faceID`, cách đọc `CCCD`, rule đối soát danh tính, dữ liệu trả về và phạm vi tích hợp thực tế.

## Kế hoạch tiếp theo

> Lưu ý: Các đầu việc liên quan `Monitor` và `App NVYT` đang được theo dõi chung trong một bảng. Các dòng có nhắc `App NVYT`, `Nhân viên y tế`, `faceID`, `CCCD` thuộc nhánh `App Nhân viên y tế`.

| Việc cần làm | Người phụ trách | Hạn xử lý | Trạng thái |
| --- | --- | --- | --- |
| Hoàn thành cấu hình mapping chỉ số sống giữa `Monitor` và `Sakura` | Nam Mai Ngọc | 18/05/2026 | Mới |
| Hoàn thành cấu hình danh sách thiết bị `Monitor` gồm `IP`, `Port` | Nam Mai Ngọc | 19/05/2026 | Mới |
| Nhận bản tin `QRY^A19` từ `Monitor` và gọi API `nb-dot-dieu-tri-id` của `SAKURA` lấy thông tin Hành chính và gửi `ADR^A19` cho `Monitor` | Nam Mai Ngọc | 22/05/2026 | Mới |
| Bổ sung phản hồi `ACK` cho từng bản tin dữ liệu từ `Monitor` | Nam Mai Ngọc | 21/05/2026 | Mới |
| Tổng hợp dữ liệu đo, map và gửi sinh hiệu lên `Sakura` | Nam Mai Ngọc | 21/05/2026 | Mới |
| Làm log, màn hình theo dõi trạng thái và chức năng đẩy lại khi lỗi | Nam Mai Ngọc | 21/05/2026 | Mới |
| Tạo ticket tổng và các subtask để follow công việc | Tuấn Kim Đình | 16/05/2026 | Mới |
| Liên hệ anh `Vũ Đình Tùng` để log ticket vào Dự án `App NVYT` cho phần `Nhân viên y tế` | Tuấn Kim Đình | 16/05/2026 | Mới |
| Hoàn thiện tài liệu nghiệp vụ chi tiết phần kết nối `Monitor` | Tuấn Kim Đình | 16/05/2026 | Đang làm |
| Fix lỗi `App Nhân viên y tế` tích hợp `faceID` quét `CCCD` | Nam Mai Ngọc | 18/05/2026 | Mới |
| Test tổng thể luồng kết nối và ghi nhận bug để fix | Tuấn Kim Đình, Nam Mai Ngọc | Tuần từ 25/05/2026 đến 29/05/2026 | Mới |
| Chuẩn bị triển khai thử tại `Medi Nam Định` | Cả nhóm | Đầu tháng 06/2026 | Mới |

## Tài liệu và note liên quan

- Transcript cuộc họp: `Meeting in Khối R&D-20260515_140022-Meeting Recording`

## Follow-up

- Cần họp lại: Có, nếu phát sinh vướng mắc trong quá trình làm hoặc test.
- Cần xác nhận thêm với:
  - Anh `Vũ Đình Tùng` về phần `App Nhân viên y tế`
  - Anh Minh nếu cần hỗ trợ thêm về luồng kỹ thuật hoặc chuẩn kết nối
