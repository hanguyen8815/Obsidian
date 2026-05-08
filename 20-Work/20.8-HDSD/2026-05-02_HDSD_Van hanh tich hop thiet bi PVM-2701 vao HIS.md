# Hướng dẫn vận hành tích hợp thiết bị PVM-2701 vào HIS

## Mục tiêu

- Hướng dẫn Cơ sở y tế, CNTT và Triển khai vận hành tích hợp Monitor PVM-2701 với HIS qua ISOFHTool.
- Giảm gián đoạn khi phát sinh lỗi tra cứu Người bệnh, lỗi kết nối hoặc sai cấu hình thiết bị.
- Chuẩn hóa cách theo dõi lỗi, resend và bàn giao vận hành sau pilot.

## Bối cảnh

- Monitor PVM-2701 kết nối với HIS thông qua ISOFHTool.
- Người dùng quét `Mã hồ sơ` trên Monitor, chọn `Find Patient`, đo sinh hiệu và để Tool gửi dữ liệu sang HIS.
- Chu kỳ ghi nhận sinh hiệu được tính từ thời điểm `Find Patient`.
- Trong cùng 1 chu kỳ, Tool chọn bản ghi nhận được cuối cùng để gửi sang HIS.
- HIS chống trùng theo `Mã hồ sơ + thời điểm đo sinh hiệu`.
- Nếu HIS hoặc Tool không tra cứu được Người bệnh thì Cơ sở y tế vận hành theo quy trình tay chuẩn tại MEDI Plus.

## Phạm vi

- Bao gồm:
  - Vận hành hằng ngày cho Điều dưỡng, CNTT, Triển khai.
  - Kiểm tra trước go-live.
  - Xử lý các tình huống ngoại lệ đã chốt ở mức pilot.
  - Theo dõi log, lỗi gửi HIS và resend.
  - Checklist bàn giao theo dõi cho CNTT Cơ sở y tế.
- Không bao gồm:
  - Cấu hình kỹ thuật chi tiết mức API hoặc payload.
  - Sửa lỗi phần cứng hoặc firmware của Monitor.
  - Cảnh báo nâng cao khi quá thời gian không có dữ liệu.

## Trạng thái

- Trạng thái hiện tại: Draft để phục vụ pilot.
- Người liên quan:
  - Điều dưỡng / Nhân viên y tế
  - Bác sĩ
  - CNTT Cơ sở y tế
  - Triển khai ISOFH
  - DEV / QA
  - Đơn vị hỗ trợ thiết bị
- Mốc thời gian:
  - Ngày tạo: 2026-05-02
  - Áp dụng: Giai đoạn pilot tại MEDI Plus

## Nội dung chính

### 1. Mục tiêu vận hành

- Đảm bảo Nhân viên y tế ghép đúng Người bệnh trước khi đo.
- Đảm bảo ISOFHTool nhận được dữ liệu từ Monitor và gửi đúng sang HIS.
- Đảm bảo lỗi vận hành được phát hiện sớm, có người theo dõi và có hướng xử lý rõ.
- Đảm bảo khi tích hợp lỗi vẫn có quy trình tay để không ảnh hưởng chăm sóc Người bệnh.

### 2. Vai trò và trách nhiệm

| Nhóm | Trách nhiệm chính |
| --- | --- |
| Điều dưỡng / Nhân viên y tế | Quét `Mã hồ sơ`, đối chiếu đúng Người bệnh trên Monitor, thực hiện đo, thông báo khi có bất thường |
| Bác sĩ | Xem dữ liệu sinh hiệu trên HIS khi cần |
| CNTT Cơ sở y tế | Kiểm tra kết nối, theo dõi lỗi hằng ngày, phối hợp resend, kiểm tra mapping `Mã máy` |
| Triển khai ISOFH | Cấu hình ban đầu, theo dõi lỗi trong thời gian đầu, hỗ trợ xử lý và bàn giao cho CNTT |
| DEV / QA | Hỗ trợ phân tích nguyên nhân gốc khi có lỗi kỹ thuật hoặc dữ liệu bất thường |
| Đơn vị hỗ trợ thiết bị | Hỗ trợ khi lỗi thuộc phạm vi Monitor hoặc kết nối phía thiết bị |

### 3. Checklist trước go-live

#### 3.1. Checklist kỹ thuật

- [ ] ISOFHTool đã được cài đặt trên máy trung tâm.
- [ ] Máy trung tâm hoạt động ổn định, có nguồn điện và mạng ổn định.
- [ ] Monitor kết nối được tới Tool theo IP đã cấu hình.
- [ ] `Mã máy` đã được cấu hình đúng trên Tool cho từng thiết bị.
- [ ] HIS đã có `Danh mục Mã máy` tương ứng.
- [ ] Monitor, Tool và HIS đã đồng bộ thời gian theo GMT+7.
- [ ] Điều dưỡng kiểm tra `Find Patient` thành công và hiển thị đúng thông tin NB trên Monitor với dữ liệu test.
- [ ] Đã đo thử và xác nhận thông tin Sinh hiệu trên màn hình Khám, màn hình Sinh hiệu trên HIS lưu được đúng dữ liệu sinh hiệu đã đo trên Monitor
- [ ] Đã kiểm tra màn hình log kỹ thuật và màn hình lỗi gửi HIS.
- [ ] Đã kiểm tra chức năng resend.

#### 3.2. Checklist nghiệp vụ

- [ ] Điều dưỡng được hướng dẫn quy trình quét `Mã hồ sơ` và đối chiếu Người bệnh trước khi đo.
- [ ] Điều dưỡng được hướng dẫn quy trình nhập tay khi tích hợp lỗi.
- [ ] CNTT biết cách kiểm tra log, lỗi gửi HIS và mapping `Mã máy`.
- [ ] Triển khai và CNTT thống nhất đầu mối xử lý sự cố trong tuần đầu.
- [ ] Đơn vị hỗ trợ thiết bị đã được thông báo đầu mối liên hệ khi lỗi phía Monitor.

### 4. Quy trình vận hành hằng ngày

#### 4.1. Quy trình cho Điều dưỡng / Nhân viên y tế

1. Kiểm tra Monitor đang ở đúng ngữ cảnh Người bệnh cần đo.
2. Quét barcode `Mã hồ sơ`.
3. Chọn `Find Patient` trên Monitor.
4. Đối chiếu thông tin Người bệnh hiển thị trên Monitor với Người bệnh thực tế.
5. Chỉ thực hiện đo khi đã xác nhận đúng Người bệnh.
6. Sau khi đo, hoàn tất thao tác đo theo quy trình bình thường.
7. Nếu có bất thường ở bước tra cứu hoặc hiển thị Người bệnh, dừng dùng luồng tự động và chuyển sang quy trình tay.

#### 4.1.1. Điểm kiểm tra dữ liệu theo bối cảnh vận hành

- Ngoại trú:
- Khi mapping đúng `Mã hồ sơ` Người bệnh, dữ liệu sinh hiệu đã đo từ Monitor sẽ được tích hợp tự động sang HIS theo luồng tích hợp bình thường.
  - Điều dưỡng đã tìm đúng Người bệnh và đo xong dữ liệu sinh hiệu trên Monitor thì không chịu trách nhiệm phải vào HIS để kiểm tra lại dữ liệu HIS đã nhận hay chưa.
  - Ở bước tiếp theo, khi Người bệnh vào Phòng khám, Bác sĩ hoặc người dùng tại màn hình khám sẽ xem dữ liệu sinh hiệu đã được tự động tích hợp từ Monitor sang HIS.
  - Nếu Bác sĩ vào khám mà không thấy dữ liệu sinh hiệu của Người bệnh trên màn hình khám thì phản ánh cho CNTT hoặc Triển khai để kiểm tra.
- Nội trú:
  - Chưa triển khai trong giai đoạn hiện tại.
  - Khi triển khai sau, dữ liệu sẽ được kiểm tra trên Dashboard hoặc trên App Nhân viên y tế đi buồng theo phương án triển khai nội trú.

#### 4.1.2. Quy trình tay chuẩn tại MEDI Plus

1. Điều dưỡng đo sinh hiệu trên Monitor theo quy trình bình thường.
2. Điều dưỡng ghi thông tin sinh hiệu vào form giấy theo quy trình đang áp dụng tại MEDI Plus.
3. Khi Người bệnh vào Phòng khám, Thư ký y khoa tại Phòng khám nhập lại dữ liệu sinh hiệu vào HIS.
4. Nếu nhập tay cùng `Mã hồ sơ` và cùng `thời gian đo sinh hiệu` đã có trên HIS thì HIS không cho phép tạo trùng.
5. Nếu nhập tay với thời gian khác và sau đó ISOFHTool resend thành công, hệ thống có thể tồn tại 2 bản ghi: 1 bản ghi do nhập tay và 1 bản ghi do tích hợp tự động hoặc resend.
6. Giai đoạn pilot chấp nhận khả năng tồn tại 2 bản ghi trong tình huống trên; hệ thống ghi nhận `Người tạo` khác nhau để phục vụ truy vết.

#### 4.2. Quy trình cho CNTT / Triển khai

1. Kiểm tra trạng thái hoạt động của máy trung tâm và ISOFHTool vào đầu ca hoặc đầu ngày.
2. Kiểm tra log kỹ thuật và danh sách lỗi gửi HIS.
3. Kiểm tra kết nối thiết bị nếu có phản ánh từ Phòng khám hoặc Bác sĩ rằng chưa thấy dữ liệu sinh hiệu của Người bệnh trên màn hình khám.
4. Kiểm tra cấu hình IP và `Mã máy` khi có thay đổi thiết bị hoặc nghi ngờ sai mapping.
5. Thực hiện resend cho các bản ghi ở trạng thái `Gửi HIS lỗi` do lỗi kết nối hoặc lỗi kỹ thuật gửi HIS. (Triển khia ISOFH sẽ thực hiện thời gian đầu (Có thể trực tiếp hoặc remote), sau đó theo dõi 1 tuần không có lỗi sẽ bàn giao lại cho Phòng CNTT. Trường hợp Phòng CNTT tiếp nhận vận hành nhưng không xử lý được thì Phòng CNTT báo Hotline ISOFH để hỗ trợ xử lý).
6. Ghi nhận sự cố, nguyên nhân, cách xử lý và kết quả xử lý vào note vận hành hoặc đầu mối hỗ trợ tương ứng.

### 5. Theo dõi hằng ngày trong giai đoạn pilot

- Tuần đầu sau go-live:
  - Triển khai ISOFH và CNTT cùng theo dõi hằng ngày.
  - Ưu tiên rà danh sách lỗi gửi HIS, log kỹ thuật, tình huống nhập tay phát sinh.
- Sau khoảng 1 tuần nếu hệ thống ổn định:
  - Có thể bàn giao theo dõi thường ngày cho CNTT Cơ sở y tế.
  - Triển khai ISOFH chuyển sang hỗ trợ khi có sự cố hoặc khi cần điều chỉnh.

## Việc cần làm

- [ ] Xác nhận số lượng Monitor thực tế trong pilot tại MEDI Plus.
- [ ] Xác nhận Monitor có khả năng lưu dữ liệu khi máy trung tâm dừng hoạt động hay không.
- [ ] Hoàn thiện checklist triển khai và checklist bàn giao cho CNTT.
- [ ] Viết tài liệu HDSD ngắn cho Điều dưỡng về thao tác `Find Patient` và nhập tay dự phòng.
- [ ] Viết tài liệu detail cho màn hình log kỹ thuật và màn hình resend.

## Business rules hoặc lưu ý thực thi

- `Mã hồ sơ` là duy nhất cho 1 lượt khám hoặc 1 lượt điều trị trong phạm vi tích hợp này.
- Một Người bệnh không có hơn 1 `Mã hồ sơ` hợp lệ cùng lúc trong phạm vi tích hợp này.
- Khi chuyển khoa, chuyển giường hoặc đổi Bác sĩ điều trị thì `Mã hồ sơ` không đổi.
- Chu kỳ ghi nhận sinh hiệu được tính từ thời điểm `Find Patient`.
- Trong cùng 1 chu kỳ, Tool chọn bản ghi nhận được cuối cùng để gửi sang HIS.
- HIS chống trùng theo `Mã hồ sơ + thời điểm đo sinh hiệu`.
- Nếu HIS báo trùng nhưng đã nhận được dữ liệu thì Tool vẫn ghi nhận trạng thái `Đã gửi HIS`.
- Sai `Mã máy` không chặn HIS nhận dữ liệu nếu HIS vẫn nhận được bản ghi; trường hợp này cần rà lại cấu hình mapping, không cần resend.
- Retry tự động chỉ áp dụng cho lỗi kết nối hoặc lỗi kỹ thuật khi gửi HIS.
- Thiết kế ban đầu mặc định retry 3 lần.
- `Lý do lỗi` trước mắt chỉ cần lưu ở ISOFHTool.
- Giai đoạn đầu chưa triển khai cảnh báo tự động khi quá thời gian không có dữ liệu.
- Giai đoạn pilot chấp nhận tình huống vừa có bản ghi nhập tay vừa có bản ghi resend hoặc tích hợp tự động nếu khác `thời gian đo sinh hiệu`; hệ thống truy vết qua `Người tạo`.

## Hướng xử lý tình huống vận hành

### 1. `Find Patient` không tìm thấy Người bệnh

- Người thực hiện: Điều dưỡng / Nhân viên y tế.
- Cách xử lý:
  1. Quét lại `Mã hồ sơ`.
  2. Bấm `Find Patient` lại.
  3. Nếu vẫn lỗi, thông báo CNTT hoặc Triển khai kiểm tra.
  4. Nếu chưa xử lý ngay được, chuyển sang `Quy trình tay chuẩn tại MEDI Plus` ở mục `4.1.2`.
- Lưu ý:
  - Phần hiển thị chi tiết trên Monitor thuộc phạm vi thiết bị.
  - Nếu lặp lại nhiều lần, cần liên hệ đơn vị hỗ trợ thiết bị.

### 2. HIS lỗi hoặc Tool không tra cứu được Người bệnh

- Người thực hiện: Điều dưỡng, CNTT, Triển khai.
- Cách xử lý:
  1. Điều dưỡng chuyển sang `Quy trình tay chuẩn tại MEDI Plus` ở mục `4.1.2` ngay để không làm chậm chăm sóc Người bệnh.
  2. CNTT hoặc Triển khai kiểm tra trạng thái Tool, mạng và kết nối HIS.
  3. Sau khi hệ thống ổn định, kiểm tra lại log và xác nhận nguyên nhân.

### 3. Monitor hiển thị sai Người bệnh sau khi quét mã

- Người thực hiện: Điều dưỡng, CNTT, Triển khai.
- Cách xử lý:
  1. Không tiếp tục đo theo luồng tự động.
  2. Chuyển sang `Quy trình tay chuẩn tại MEDI Plus` ở mục `4.1.2` để tránh chờ.
  3. Kiểm tra lại dữ liệu `Mã hồ sơ`, thao tác người dùng và phản hồi từ Monitor.
  4. Nếu nguyên nhân thuộc thiết bị, liên hệ đơn vị hỗ trợ thiết bị.

### 4. Monitor vẫn hiển thị Người bệnh cũ dù đã quét đúng mã

- Người thực hiện: Điều dưỡng, CNTT, Triển khai.
- Cách xử lý:
  1. Quét lại `Mã hồ sơ`.
  2. Bấm `Find Patient` lại.
  3. Thử lại vài lần.
  4. Nếu vẫn không được, reset Monitor.
  5. Nếu còn lặp lại, liên hệ đơn vị hỗ trợ thiết bị để xử lý triệt để.
- Lưu ý:
  - Đây là tình huống thuộc phạm vi thiết bị, không phải logic nghiệp vụ HIS. 

### 5. Bác sĩ vào khám nhưng không thấy dữ liệu sinh hiệu của Người bệnh trên màn hình khám

- Người phát hiện: Bác sĩ hoặc người dùng tại màn hình khám.
- Người kiểm tra: CNTT, Triển khai.
- Cách kiểm tra:
  1. Kiểm tra Tool có nhận được dữ liệu từ Monitor không.
  2. Kiểm tra bản ghi có đang là bản ghi cuối cùng trong chu kỳ hay chưa.
  3. Kiểm tra log gửi HIS và danh sách lỗi gửi HIS.
  4. Kiểm tra kết nối HIS và trạng thái retry.
  5. Nếu cần, thực hiện resend.

### 6. Sai `Mã máy`

- Người thực hiện: CNTT, Triển khai.
- Cách xử lý:
  1. Kiểm tra cấu hình `Mã máy` trên Tool.
  2. Đối chiếu với `Danh mục Mã máy` trên HIS.
  3. Sửa lại cấu hình nếu sai.
  4. Đo thử và xác nhận dữ liệu mới đã gắn đúng `Mã máy`.
- Lưu ý:
  - Nếu HIS vẫn nhận được dữ liệu thì không cần resend bản ghi cũ chỉ vì sai `Mã máy`.

### 7. Lỗi gửi HIS

- Người thực hiện: CNTT, Triển khai.
- Cách xử lý:
  1. Kiểm tra log kỹ thuật và `Lý do lỗi` trên Tool.
  2. Xác định lỗi kết nối hay lỗi kỹ thuật khác.
  3. Đợi Tool retry tự động theo số lần mặc định nếu đang trong quá trình retry.
  4. Nếu bản ghi đã vào trạng thái `Gửi HIS lỗi`, thực hiện resend bằng tay.
  5. Nếu resend nhiều lần vẫn lỗi, phối hợp DEV để phân tích nguyên nhân gốc.

### 8. Máy trung tâm dừng hoạt động

- Người thực hiện: CNTT, Triển khai, Cơ sở y tế.
- Cách xử lý tạm thời:
  1. Chuyển sang `Quy trình tay chuẩn tại MEDI Plus` ở mục `4.1.2`.
  2. CNTT kiểm tra nguồn điện, mạng, trạng thái máy và ISOFHTool.
  3. Xác nhận lại với hãng thiết bị xem Monitor có lưu tạm dữ liệu được không.
- Lưu ý:
  - Đây là điểm cần xác nhận thêm trong pilot.

## Theo dõi lỗi và resend

### 1. Khi nào cần resend

- Bản ghi ở trạng thái `Gửi HIS lỗi` do lỗi kết nối hoặc lỗi kỹ thuật gửi HIS.
- Nếu Cơ sở y tế đã xử lý theo `Quy trình tay chuẩn tại MEDI Plus`, việc resend vẫn được phép thực hiện khi cần; giai đoạn pilot chấp nhận khả năng tồn tại thêm 1 bản ghi resend ngoài bản ghi nhập tay nếu khác `thời gian đo sinh hiệu`.

### 2. Khi nào không cần resend

- HIS đã nhận được dữ liệu nhưng tự xử lý trùng bên trong HIS.
- Sai `Mã máy` nhưng HIS vẫn đã nhận và lưu được bản ghi sinh hiệu.

### 3. Cách theo dõi lỗi hằng ngày

- Kiểm tra danh sách lỗi gửi HIS vào đầu ngày hoặc đầu ca.
- Ưu tiên xử lý các lỗi mới phát sinh trong ngày.
- Đối chiếu lại với phản ánh thực tế từ khoa nếu có trường hợp Người dùng báo không thấy dữ liệu.
- Theo dõi SLA xử lý lỗi mục tiêu: `15 - 30 phút` khi đã có đủ thông tin.

## Checklist bàn giao cho CNTT Cơ sở y tế

- [ ] CNTT biết cách mở và đọc log kỹ thuật.
- [ ] CNTT biết cách mở danh sách lỗi gửi HIS.
- [ ] CNTT biết cách resend bản ghi lỗi.
- [ ] CNTT biết cách kiểm tra và sửa cấu hình IP thiết bị.
- [ ] CNTT biết cách kiểm tra và sửa `Mã máy` trên Tool.
- [ ] CNTT nắm được khi nào phải chuyển sang quy trình tay.
- [ ] CNTT có đầu mối liên hệ Triển khai ISOFH.
- [ ] CNTT có đầu mối liên hệ đơn vị hỗ trợ thiết bị.
- [ ] CNTT đã được nhắc bắt buộc kiểm tra đồng bộ giờ GMT+7 khi có sự cố chống trùng hoặc lệch dữ liệu.

## Rủi ro và điểm cần làm rõ

- Rủi ro:
  - Máy trung tâm là điểm lỗi đơn trong giai đoạn pilot.
  - Sai `Mã máy` vẫn có thể làm dữ liệu vào HIS nhưng truy vết thiết bị bị sai.
  - Nếu Điều dưỡng không đối chiếu đúng Người bệnh trước khi đo thì có thể ghi nhận nhầm dữ liệu.
  - Khi HIS hoặc Tool lỗi ở bước tra cứu, quy trình tay làm tăng thao tác cho khoa.
- Open question:
  - Một máy trung tâm thực tế phục vụ tối đa bao nhiêu Monitor tại MEDI Plus.
  - Monitor có lưu được dữ liệu khi máy trung tâm dừng hoạt động hay không.

## Tài liệu và note liên quan

- [[2026-05-02_SRS_High level_Tich hop thiet bi PVM-2701 vao HIS]]
- [[2026-05-02_QA_Chot nghiep vu_Tich hop thiet bi PVM-2701 vao HIS]]
- [[2026-05-01_NV_Tich hop thiet bi PVM-2701 vao HIS - Phase I]]
- [[2026-05-02_NV_Tich hop thiet bi PVM-2701 vao HIS - Phase II]]
- [[2026-05-02_Decision_Tich hop thiet bi PVM-2701 vao HIS]]

## Nhật ký cập nhật

- 2026-05-02: Tạo note vận hành pilot dựa trên HLR và Q&A đã chốt.
