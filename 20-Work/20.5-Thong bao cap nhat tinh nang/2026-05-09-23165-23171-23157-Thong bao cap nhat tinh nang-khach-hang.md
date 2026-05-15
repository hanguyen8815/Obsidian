# Thông báo cập nhật tính năng ngày 09/05/2026

- Phiên bản: 23165-23171-23157
- Đối tượng nhận: Khách hàng và Người dùng hệ thống HIS/EMR SAKURA
- Thời gian áp dụng: các đợt phát hành từ 05/05/2026 đến 09/05/2026
- Phạm vi thông báo: bản biên tập theo góc nhìn Người dùng, đã lược bỏ mã công việc và nội dung theo dõi tiến độ nội bộ

Trong đợt cập nhật này, hệ thống tập trung vào các thay đổi ảnh hưởng trực tiếp đến thao tác hằng ngày của Nhân viên Tiếp đón, Bác sĩ, Điều dưỡng, Thu ngân và người dùng khai thác báo cáo. Nội dung dưới đây ưu tiên diễn giải rõ cập nhật gì, ảnh hưởng ra sao và Người dùng cần thao tác thế nào sau khi nâng cấp.

## Điểm chính của đợt cập nhật

- Giảm thao tác lặp tại Tiếp đón, Kiosk, Khám bệnh và Nội trú.
- Hạn chế các tình huống dễ gây sai dữ liệu như chỉ định trùng, dùng trùng thẻ BHYT, chọn nhầm giường hoặc hiển thị sai trạng thái.
- Hoàn thiện thêm nhiều phiếu in, biểu mẫu điện tử, dữ liệu ký số và danh sách đẩy cổng.
- Bổ sung thêm bộ lọc, báo cáo và cách lấy dữ liệu để hỗ trợ đối soát và theo dõi vận hành chính xác hơn.

## 1. Tiếp đón, Kiosk và lịch hẹn

### 1.1. Bổ sung trường Cấp độ bảo mật tại màn hình Tiếp đón

- Cập nhật: Hệ thống bổ sung thêm trường nhập Cấp độ bảo mật tại màn hình Tiếp đón ở đơn vị có cấu hình sử dụng.
- Ảnh hưởng: Nhân viên Tiếp đón có thể ghi nhận ngay mức độ bảo mật của hồ sơ trong lúc tiếp nhận, giúp các bộ phận sau theo dõi và xử lý đúng phạm vi.
- Cách dùng: Khi tạo hoặc cập nhật thông tin tiếp đón, người dùng kiểm tra xem màn hình có hiển thị trường Cấp độ bảo mật hay không. Nếu có, chọn đúng mức theo quy định tại đơn vị trước khi lưu.

### 1.2. Tối ưu luồng lấy số tại Kiosk và hạn chế lấy số trùng

- Cập nhật: Kiosk được tối ưu cách lấy thông tin đợt điều trị, đồng thời bổ sung thiết lập cho phép chặn việc một Người bệnh lấy nhiều số trong cùng một ngày bằng cùng thông tin hoặc cùng mã hồ sơ.
- Ảnh hưởng: Luồng đăng ký khám tại Kiosk ổn định hơn, giảm tình trạng phát sinh nhiều số cho cùng một Người bệnh và giảm sai sót khi điều phối.
- Cách dùng: Với đơn vị bật cấu hình chặn trùng, nếu Người bệnh đã lấy số trong ngày thì hệ thống sẽ không tạo thêm số mới. Nhân viên hỗ trợ nên tra cứu số đã cấp trước khi hướng dẫn lấy lại.

### 1.3. Giữ nguyên danh sách dịch vụ khi cảnh báo thiếu thông tin chỉ định

- Cập nhật: Khi chỉ định dịch vụ từ Tiếp đón mà chưa chọn đủ Phòng khám hoặc Bác sĩ, nếu đóng cảnh báo thì hệ thống không quay ngược về màn hình trước và không xóa danh sách dịch vụ đã chọn.
- Ảnh hưởng: Người dùng không phải làm lại từ đầu khi thiếu một vài thông tin bắt buộc, giảm thời gian thao tác và giảm nguy cơ chỉ định lại nhầm.
- Cách dùng: Sau khi hệ thống cảnh báo, người dùng chỉ cần bổ sung thông tin còn thiếu rồi tiếp tục thực hiện. Danh sách dịch vụ đã chọn sẽ được giữ nguyên.

### 1.4. Chặn dùng trùng thẻ BHYT ở các khoảng thời gian chồng nhau

- Cập nhật: Hệ thống kiểm tra thẻ BHYT đã được dùng ở mã hồ sơ cũ trong khoảng thời gian chồng lấn hay chưa trước khi cho tiếp đón hoặc cập nhật lại thẻ.
- Ảnh hưởng: Giảm nguy cơ phát sinh hai mã hồ sơ dùng cùng một thẻ BHYT trong cùng khoảng điều trị, hỗ trợ đối soát và quyết toán chặt chẽ hơn.
- Cách dùng: Nếu hệ thống báo thẻ đã được sử dụng trước đó, Nhân viên Tiếp đón cần kiểm tra lại mã hồ sơ cũ, thời gian vào viện hoặc ra viện trước khi tiếp tục thao tác.

### 1.5. Tự động ghép dịch vụ cận lâm sàng đã hẹn theo dịch vụ khám đã hẹn

- Cập nhật: Bổ sung thiết lập để khi tiếp đón theo lịch hẹn, hệ thống có thể tự ghép các dịch vụ cận lâm sàng đã hẹn vào đúng dòng dịch vụ khám tương ứng.
- Ảnh hưởng: Danh sách dịch vụ sau khi tiếp đón gọn hơn, đúng nhóm hơn và giảm thao tác ghép tay.
- Cách dùng: Tại đơn vị bật cấu hình này, sau khi chọn kê dịch vụ theo lịch hẹn, người dùng cần kiểm tra lại danh sách dịch vụ được ghép tự động trước khi lưu.

## 2. Khám ngoại trú, chỉ định và kê đơn

### 2.1. Kê thuốc theo thứ và theo buổi chi tiết hơn

- Cập nhật: Luồng kê thuốc theo tuần được bổ sung thêm cột Số lượng theo từng buổi sáng, trưa, chiều, tối.
- Ảnh hưởng: Bác sĩ kê đơn sát hơn với phác đồ dùng thuốc theo ngày và theo buổi, giảm phải ghi chú thủ công.
- Cách dùng: Khi mở popup liều dùng theo tuần, nhập số lượng tương ứng cho từng buổi trong ngày thay vì chỉ ghi tổng liều chung.

### 2.2. Tự động chọn nơi lấy mẫu theo mức ưu tiên của phòng

- Cập nhật: Khi chỉ định dịch vụ có nhiều nơi lấy mẫu, hệ thống tự gợi ý nơi lấy mẫu theo mức ưu tiên đã cấu hình tại phòng.
- Ảnh hưởng: Giảm thao tác chọn tay và giảm nguy cơ chọn sai nơi lấy mẫu khi một dịch vụ có thể thực hiện ở nhiều vị trí.
- Cách dùng: Sau khi chỉ định, người dùng kiểm tra trường Nơi lấy mẫu đã được điền sẵn. Nếu thực tế cần đổi sang vị trí khác, vẫn có thể chỉnh lại trước khi lưu.

### 2.3. Ổn định hơn khi phân phòng và in phiếu chỉ định

- Cập nhật: Sau khi phân phòng và in phiếu chỉ định, màn hình sẽ tự tải lại để hiển thị đúng phòng thực hiện mới nhất, tránh việc lưu lại làm dịch vụ quay về phòng cũ.
- Ảnh hưởng: Giảm sai lệch giữa phòng chung và phòng thực hiện thật, nhất là ở các luồng có tách phân phòng trước khi thực hiện dịch vụ.
- Cách dùng: Sau khi in phiếu chỉ định, người dùng cần chờ màn hình tải lại xong rồi mới sửa tiếp thông tin dịch vụ nếu cần.

### 2.4. Bổ sung tùy chọn ẩn cột giá ở popup chỉ định dịch vụ kỹ thuật

- Cập nhật: Hệ thống hỗ trợ cấu hình ẩn cột giá ở danh sách dịch vụ kỹ thuật, nhưng vẫn giữ phần đơn giá của dịch vụ đang chọn nếu đơn vị cần xem.
- Ảnh hưởng: Hạn chế nhầm lẫn giữa các mức giá trong những khu khám có nhiều loại giá khác nhau.
- Cách dùng: Nếu đơn vị đã bật cấu hình ẩn giá, người dùng chỉ xem thông tin đơn giá tại phần chi tiết dịch vụ đang chọn, không tìm ở danh sách bên trái như trước.

### 2.5. Hạn chế trùng thuốc và hỗ trợ tìm thuốc nhanh hơn

- Cập nhật: Hệ thống xử lý việc bấm lưu nhiều lần khi kê thuốc để tránh sinh trùng thuốc, đồng thời sửa thao tác phím tắt F2 để con trỏ quay lại đúng ô tìm tên thuốc.
- Ảnh hưởng: Giảm nguy cơ kê trùng do thao tác nhanh hoặc do mạng chậm, đồng thời giúp Bác sĩ tìm thuốc liên tục thuận tiện hơn.
- Cách dùng: Người dùng vẫn thao tác như hiện tại. Sau khi bấm lưu, nên chờ hệ thống xử lý xong. Trong popup kê thuốc, có thể dùng lại phím F2 để quay về ô tìm kiếm tên thuốc.

## 3. Nội trú và điều trị ngoại trú

### 3.1. Sao chép tờ điều trị có thêm lựa chọn không sao chép diễn biến

- Cập nhật: Popup sao chép tờ điều trị được bổ sung checkbox Không sao chép diễn biến.
- Ảnh hưởng: Phù hợp hơn với các khoa chỉ muốn sao chép y lệnh hoặc cấu trúc tờ điều trị mà không muốn mang theo diễn biến cũ.
- Cách dùng: Khi sao chép tờ điều trị, nếu muốn tạo tờ mới nhưng không mang nội dung diễn biến từ tờ cũ, người dùng tích chọn Không sao chép diễn biến trước khi xác nhận.

### 3.2. Danh sách giường và thông tin phòng giường chính xác hơn

- Cập nhật: Hệ thống sửa cách kiểm tra số Người bệnh nằm giường theo khoảng thời gian, đồng thời bổ sung hiển thị phòng và giường ở một số danh sách nội trú và dữ liệu xuất danh sách.
- Ảnh hưởng: Người dùng dễ chọn đúng giường còn trống, giảm tình trạng nhìn thấy giường trống nhưng thực tế đã có Người bệnh trong cùng khoảng thời gian.
- Cách dùng: Khi phân giường hoặc tra cứu danh sách nội trú, người dùng kiểm tra thêm các cột Phòng và Giường. Nếu hệ thống cảnh báo giường đã có Người bệnh, cần rà soát lại mã hồ sơ đang nằm tại giường đó.

### 3.3. Phiếu thực hiện và công khai thuốc đầy đủ thông tin hơn

- Cập nhật: Phiếu điều dưỡng được bổ sung thêm ghi chú, chế độ ăn, chế độ chăm sóc, chế độ theo dõi từ các tờ điều trị trong ngày; đồng thời có thể cấu hình hiển thị sẵn 3 dòng trống ở bảng tên thuốc.
- Ảnh hưởng: Điều dưỡng có đủ thông tin tổng hợp hơn khi in và đối chiếu phiếu, hạn chế bỏ sót dữ liệu nằm rải rác ở nhiều tờ điều trị.
- Cách dùng: Khi in phiếu, người dùng kiểm tra lại dữ liệu tổng hợp trong ngày. Nếu đơn vị có cấu hình 3 dòng trống mặc định, phần bảng tên thuốc sẽ hiển thị sẵn để ghi bổ sung khi cần.

### 3.4. Thanh toán cho đối tượng Điều trị ngoại trú linh hoạt hơn

- Cập nhật: Với đơn vị áp dụng đối tượng khám chữa bệnh số 2 là Điều trị ngoại trú, hệ thống hỗ trợ xử lý theo luồng ngoại trú ở các nghiệp vụ thanh toán, cảnh báo tạm ứng, in phiếu có mã QR và một số báo cáo liên quan.
- Ảnh hưởng: Phù hợp hơn với thực tế vận hành ở các đơn vị đang quản lý nhóm này theo hướng ngoại trú thay vì nội trú.
- Cách dùng: Tại các đơn vị đã bật cấu hình, Thu ngân và người dùng liên quan thao tác theo luồng ngoại trú bình thường. Khi in phiếu hoặc xem báo cáo, cần kiểm tra thêm lựa chọn đối tượng Điều trị ngoại trú nếu màn hình có hiển thị.

### 3.5. Cho phép bỏ qua bắt buộc nhập giấy tờ tùy thân tại quầy được cấu hình

- Cập nhật: Hệ thống hỗ trợ khai báo danh sách quầy được phép bỏ qua bước bắt buộc nhập giấy tờ tùy thân.
- Ảnh hưởng: Phù hợp với các tình huống cấp cứu hoặc tiếp nhận nhanh khi chưa đủ giấy tờ tại thời điểm vào viện.
- Cách dùng: Ở quầy đã được cấu hình, người dùng có thể hoàn tất tiếp đón trước và bổ sung giấy tờ sau. Ngoài các quầy này, quy tắc bắt buộc nhập giấy tờ vẫn giữ nguyên.

## 4. Hồ sơ điện tử, biểu mẫu và giấy tờ đẩy cổng

### 4.1. Hiển thị rõ hơn tên người ký và khoa làm việc trên một số phiếu chăm sóc

- Cập nhật: Dữ liệu tên người ký và khoa làm việc được truyền từ Phiếu chăm sóc sang một số phiếu đánh giá và phiếu liên quan; đồng thời hỗ trợ hiển thị tên người ký theo cấu hình.
- Ảnh hưởng: Hồ sơ in ra hoặc hồ sơ điện tử hiển thị rõ người đã ký, giúp dễ kiểm tra và truy vết hơn.
- Cách dùng: Người dùng cần ký phiếu bằng đúng tài khoản đang phụ trách. Sau khi ký, kiểm tra lại phần tên người ký và khoa làm việc trên phiếu nếu đơn vị có sử dụng biểu mẫu liên kết.

### 4.2. Mở rộng dữ liệu và giới hạn nhập trên một số biểu mẫu điện tử

- Cập nhật: Một số biểu mẫu được bổ sung thêm trường dữ liệu, tăng giới hạn ký tự nhập và hoàn thiện liên kết dữ liệu giữa màn hình nghiệp vụ với phiếu in.
- Ảnh hưởng: Người dùng có thể nhập được nội dung chi tiết hơn, giảm tình trạng bị thiếu chỗ nhập hoặc phải ghi chú thủ công ngoài phiếu.
- Cách dùng: Khi điền biểu mẫu, nếu đơn vị đã được cập nhật mẫu mới thì người dùng nhập trực tiếp vào các trường bổ sung trên form thay vì ghi chú riêng bên ngoài.

### 4.3. Danh sách giấy chứng sinh và danh sách đẩy cổng dễ lọc và dễ theo dõi hơn

- Cập nhật: Bổ sung bộ lọc theo cấp ký, sửa cách hiển thị trạng thái giấy tờ tùy thân, thêm trạng thái Thất bại khi đẩy cổng không thành công và cải thiện bộ lọc khoa, ICD ở danh sách đẩy cổng bệnh truyền nhiễm.
- Ảnh hưởng: Người dùng dễ phân loại hồ sơ cần xử lý, dễ theo dõi hồ sơ đẩy cổng lỗi và dễ lọc đúng nhóm giấy tờ theo nghiệp vụ ký duyệt.
- Cách dùng: Tại các màn hình danh sách giấy tờ, người dùng sử dụng thêm các bộ lọc mới như Cấp ký, Khoa, ICD hoặc trạng thái Thất bại để rà soát hồ sơ nhanh hơn.

### 4.4. Thêm lựa chọn Không có giấy tờ tùy thân trên Giấy chứng sinh

- Cập nhật: Giấy chứng sinh được bổ sung checkbox Không có giấy tờ tùy thân.
- Ảnh hưởng: Hỗ trợ thao tác trong các trường hợp thực tế sản phụ chưa có giấy tờ tại thời điểm lập giấy chứng sinh, đồng thời giảm việc bị chặn lưu phiếu trên giao diện.
- Cách dùng: Khi lập giấy chứng sinh, nếu sản phụ chưa có giấy tờ tùy thân thì tích chọn Không có giấy tờ tùy thân. Nếu không tích, quy tắc bắt buộc nhập giấy tờ vẫn giữ như cũ.

### 4.5. Hiển thị lịch sử nhiều mã hồ sơ ở hồ sơ điều trị dài hạn

- Cập nhật: Tại chi tiết hồ sơ điều trị dài hạn, hệ thống hiển thị danh sách các mã hồ sơ liên quan để người dùng chuyển xem từng lần khám hoặc từng đợt điều trị.
- Ảnh hưởng: Dễ tra cứu lại toàn bộ lịch sử của cùng một Người bệnh khi có nhiều mã hồ sơ theo thời gian.
- Cách dùng: Trong phần Hồ sơ bệnh án, người dùng chọn đúng mã hồ sơ cần xem thay vì chỉ theo mặc định một mã như trước.

## 5. Báo cáo, đối soát và khai thác dữ liệu

### 5.1. Bổ sung thêm báo cáo mới và mở rộng bộ lọc ở nhiều báo cáo hiện có

- Cập nhật: Hệ thống bổ sung thêm một số báo cáo mới như báo cáo tổng hợp xuất kho cho Người bệnh, sổ theo dõi khóa tập phục hồi chức năng, đồng thời mở rộng thêm nhiều bộ lọc như tên hàng hóa, tên dịch vụ, khoa, phòng, đối tượng khám chữa bệnh và các lựa chọn chọn nhiều giá trị.
- Ảnh hưởng: Người dùng khai thác báo cáo có thêm công cụ lọc sát nhu cầu thực tế, giảm phải xuất thô rồi lọc lại bằng tay.
- Cách dùng: Khi vào màn hình báo cáo, người dùng rà soát thêm các tiêu chí lọc mới. Một số bộ lọc hỗ trợ chọn nhiều giá trị hoặc nhóm dữ liệu theo khoa/phòng.

### 5.2. Cải thiện độ chính xác của số liệu ở các báo cáo tài chính và vận hành

- Cập nhật: Nhiều báo cáo được sửa cách lấy dữ liệu để khớp hơn với phiếu thu, hóa đơn, tình trạng tạm ứng, thời gian thanh toán hoặc dữ liệu sử dụng giường, hàng hóa, dịch vụ.
- Ảnh hưởng: Số liệu trên báo cáo ổn định và dễ đối chiếu hơn giữa các bộ phận vận hành, thu ngân, kho và kế toán.
- Cách dùng: Sau nâng cấp, người dùng nên kiểm tra lại các báo cáo đang dùng thường xuyên, nhất là các báo cáo đối soát tiền, hóa đơn, xuất kho và đối tượng khám chữa bệnh để nắm rõ cách lấy dữ liệu mới.

### 5.3. Bổ sung lựa chọn và dữ liệu phục vụ nhóm Điều trị ngoại trú trên báo cáo

- Cập nhật: Một số báo cáo bổ sung thêm lựa chọn Đối tượng khám chữa bệnh là Điều trị ngoại trú hoặc cập nhật lại cách phân loại nhóm này.
- Ảnh hưởng: Phù hợp hơn với các đơn vị đang cần tách riêng nhóm Điều trị ngoại trú khi tra cứu doanh thu, thanh toán hoặc dữ liệu vận hành.
- Cách dùng: Nếu màn hình báo cáo có trường Đối tượng khám chữa bệnh, người dùng kiểm tra xem đã có thêm lựa chọn Điều trị ngoại trú hay chưa để lọc đúng nhóm dữ liệu cần xem.

## Người dùng cần lưu ý

- Một số thay đổi hoạt động theo cấu hình riêng của từng Cơ sở y tế. Nếu đơn vị chưa bật cấu hình tương ứng thì màn hình có thể chưa hiển thị thay đổi.
- Với các đơn vị đang dùng Kiosk, PACS, LIS, hóa đơn điện tử hoặc các danh sách đẩy cổng, nên kiểm tra nhanh lại các luồng tích hợp sau khi nâng cấp.
- Với các phiếu in, biểu mẫu điện tử và báo cáo, người dùng nên in thử hoặc tra cứu thử 1 đến 2 hồ sơ mẫu để làm quen với trường mới, bộ lọc mới hoặc cách lấy dữ liệu mới.

## Ghi chú

- Bản thông báo này được biên tập lại từ tài liệu phát hành nội bộ theo hướng dễ đọc đối với Người sử dụng.
- Các mã ticket, trạng thái xử lý và nội dung quản lý tiến độ nội bộ đã được lược bỏ khỏi tài liệu này.
