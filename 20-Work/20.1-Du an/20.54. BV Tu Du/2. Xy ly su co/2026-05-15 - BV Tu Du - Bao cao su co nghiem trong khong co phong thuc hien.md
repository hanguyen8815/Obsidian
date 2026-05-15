# Báo cáo sự cố nghiêm trọng và bản gửi lãnh đạo - Không có Phòng thực hiện sau cập nhật `HIS_SAKURA26_5.4.0`

## 1. Thông tin chung

- Đơn vị: Bệnh viện Từ Dũ - Cơ sở 1
- Môi trường bị ảnh hưởng: HIS Production
- Phiên bản cập nhật: từ `HIS_SAKURA26_5.2.0` lên `HIS_SAKURA26_5.4.0`
- Thời điểm cập nhật: 20h00 ngày 14/05/2026
- Thời điểm ghi nhận sự cố đầu tiên: 05h59 ngày 15/05/2026
- Thời điểm khôi phục tiếp đón: khoảng 06h45 ngày 15/05/2026
- Thời điểm chốt revert ticket lỗi và build lại: 06h59 ngày 15/05/2026
- Trạng thái hiện tại: Đã tiếp đón bình thường trở lại, chưa xác định được nguyên nhân gốc
- Mức độ: Sự cố nghiêm trọng

## 2. Bản tóm tắt gửi lãnh đạo

- Sáng 15/05/2026, sau khi cập nhật phiên bản hệ thống từ `HIS_SAKURA26_5.2.0` lên `HIS_SAKURA26_5.4.0` vào tối 14/05/2026, toàn bộ quầy Tiếp đón tại Bệnh viện Từ Dũ - Cơ sở 1 gặp lỗi `Không có Phòng thực hiện` khi tiếp đón và chỉ định các Dịch vụ khám.
- Sự cố làm chặn ngay khâu đầu vào của quy trình khám, dẫn tới không thể tiếp đón Người bệnh trong khung giờ đầu ngày.
- Thời gian ảnh hưởng thực tế từ lúc ghi nhận đầu tiên 05h59 đến khi tiếp đón ổn định trở lại khoảng 06h45 ngày 15/05/2026, tương đương khoảng 45 phút gián đoạn vận hành.
- Team Triển khai, FE và BE đã phối hợp kiểm tra. Trong quá trình khoanh vùng, API tổng hợp dịch vụ vẫn trả dữ liệu nhưng droplist chọn phòng trên giao diện không có giá trị để chọn.
- Hướng xử lý cuối cùng là revert FE trước để khôi phục vận hành, sau đó FE tiếp tục revert ticket nghi ngờ gây lỗi và build lại. Sau khi revert FE, phía BE không cần revert nữa.
- Đến thời điểm lập note, sự cố đã xử lý xong ở mức vận hành, nhưng chưa có kết luận chính thức về nguyên nhân gốc. Đây là sự cố nghiêm trọng vì ảnh hưởng toàn bộ khâu Tiếp đón và cần được làm RCA đầy đủ để tránh lặp lại.

## 3. Mức ảnh hưởng

- Ảnh hưởng trực tiếp tới toàn bộ quầy Tiếp đón.
- Ảnh hưởng tới tất cả Dịch vụ khám ở bước chọn `Phòng thực hiện`.
- Người dùng không thể tiếp đón Người bệnh, làm dừng chuỗi vận hành ngay từ đầu quy trình.
- Trong khung giờ đầu ngày, đây là điểm nghẽn nghiêm trọng vì số lượng Người bệnh bắt đầu đến khám tăng nhanh.
- Nếu xử lý chậm hơn, nguy cơ kéo theo ùn tắc khu Tiếp đón, chậm luồng khám và tăng áp lực cho đội tại viện là rất cao.

## 4. Diễn biến chi tiết theo thời gian

- 20h00 ngày 14/05/2026:

  - Thực hiện cập nhật phiên bản từ `HIS_SAKURA26_5.2.0` lên `HIS_SAKURA26_5.4.0`.

- 05h59 ngày 15/05/2026:

  - Minh Cơ tại Tiếp đón báo trên nhóm lỗi `Không có phòng thực hiện`.
  - Tin nhắn bị lẫn trong các tin check-in nên Hà Nguyễn và Kiều Trinh chưa nhận ra ngay mức độ nghiêm trọng.

- 06h11 ngày 15/05/2026:

  - Kiều Trinh xác nhận lại vẫn đang lỗi và bắt đầu kiểm tra.

- 06h13 ngày 15/05/2026:

  - Phú Bình gọi cho Hà Nguyễn thông báo toàn bộ các quầy Tiếp đón, chỉ định tất cả các Dịch vụ khám đều lỗi `Không có Phòng thực hiện`.
  - Xác nhận với Tôn thì Danh mục không có thay đổi.

- 06h14 ngày 15/05/2026:

  - Hà Nguyễn gọi báo Hữu Ánh DEV BE vì nghiệp vụ cân bằng tải do BE xử lý, FE truyền phòng trống lên.
  - Song song, Hữu Ánh kiểm tra; Hà Nguyễn và Kiều Trinh xem phương án bỏ cân bằng tải để vận hành tạm.
  - Kiều Trinh gọi xin ý kiến anh Tiến Ngọc, phụ trách Tiếp đón của Bệnh viện. Anh Tiến Ngọc không đồng ý giải pháp bật lại cân bằng tải vì Người sử dụng không nắm được cách phân phòng, yêu cầu phải xử lý gấp theo đúng luồng đang dùng.

- Sau 06h14 ngày 15/05/2026:

  - Hà Nguyễn và Kiều Trinh nhả lại thiết lập cũ, đồng thời dò lại PIC và ticket liên quan.
  - Phát hiện ticket `SAKURA-96573` có nội dung yêu cầu bắt buộc nhập `Phòng thực hiện`, nhưng không note rõ điều kiện thiết lập hoặc trường hợp cụ thể nào mới bắt buộc nhập.

- 06h23 ngày 15/05/2026:

  - Hà Nguyễn gọi cho Mai Ngọc Nam để nhờ kiểm tra FE.

- 06h29 ngày 15/05/2026:

  - Hữu Ánh cung cấp thêm thông tin API:
    - `https://api-bvtudu.tudu.com.vn/api/his/v1/dm-dv-ky-thuat/tong-hop?page=0&size=50&active=true&timKiem=KB0078&gioiTinh=2&ngaySinh=1979-04-16&dsKhuVucId=4&loaiDichVu=10&khoaChiDinhId=27&loaiDoiTuongId=4&doiTuongKcb=1&khoaChiDinh=false&dsCoSoKcbId=1`
  - Kết quả kiểm tra cho thấy API vẫn có dữ liệu, nhưng droplist chọn phòng trên giao diện không có giá trị để chọn.

- 06h31 ngày 15/05/2026:

  - Hà Nguyễn gọi lại Mai Ngọc Nam để cập nhật tình hình.
  - Nam báo chờ 5 phút để phản hồi lại.
  - Hà Nguyễn nêu rõ: nếu chưa tìm được nguyên nhân thì cần revert bản build để khôi phục khả năng tiếp đón.

- 06h39 ngày 15/05/2026:

  - Kiều Trinh chưa thấy báo đã tìm được nguyên nhân nên đề nghị cả BE và FE revert code bản build.

- 06h42 ngày 15/05/2026:

  - Mai Ngọc Nam FE báo revert FE trước để Tiếp đón thử lại trước.
  - Nam tiếp tục xử lý revert ticket lỗi và build lại, BE không cần revert nữa.
  - Team đã báo lại phía BE dừng phương án revert.
  - Hà Nguyễn và Kiều Trinh báo Tiếp đón thực hiện lại tiếp đón Người bệnh và xác nhận đã hoạt động được.

- Từ 06h45 ngày 15/05/2026:

  - Tiếp đón vận hành bình thường trở lại.

- 06h59 ngày 15/05/2026:

  - Mai Ngọc Nam báo đã hoàn thành revert ticket lỗi và build lại.

## 5. Nhận định hiện tại

- Chưa đủ căn cứ để kết luận nguyên nhân gốc.
- Có dấu hiệu cho thấy lỗi liên quan thay đổi rule bắt buộc nhập `Phòng thực hiện` trong ticket `SAKURA-96573` nhưng mô tả ticket chưa nêu rõ điều kiện áp dụng theo thiết lập hoặc theo từng trường hợp nghiệp vụ.
- API tổng hợp dịch vụ vẫn trả dữ liệu, nhưng giao diện không hiển thị được danh sách phòng để chọn. Dấu hiệu này nghiêng về một trong các khả năng sau:
  - FE lọc hoặc render sai điều kiện hiển thị danh sách phòng.
  - Rule bắt buộc nhập `Phòng thực hiện` được áp dụng rộng hơn phạm vi dự kiến.
  - Logic phối hợp giữa FE, dữ liệu phòng trống và rule cân bằng tải chưa khớp sau cập nhật phiên bản.
- Việc chỉ cần revert FE để khôi phục vận hành là tín hiệu cần ưu tiên rà soát phần thay đổi phía FE và ticket liên quan trước.
- Tuy vậy, đây mới là nhận định sơ bộ, chưa phải kết luận chính thức.

## 6. Điểm hở đã lộ ra từ sự cố

- Ticket `SAKURA-96573` chưa mô tả rõ điều kiện nào mới bắt buộc nhập `Phòng thực hiện`.
- Chưa có chốt rõ phần thay đổi này ảnh hưởng chung toàn hệ thống hay chỉ áp dụng khi có cấu hình cụ thể.
- Sau khi nâng phiên bản, chưa phát hiện sớm ở ca đầu ngày dù lỗi ảnh hưởng toàn bộ Tiếp đón.
- Tin báo lỗi đầu tiên bị trôi trong nhóm check-in, cho thấy cơ chế nhận diện và escalte sự cố nghiêm trọng còn yếu.
- Chưa có kết luận nguyên nhân gốc ngay sau khi khôi phục vận hành, dẫn tới rủi ro lặp lại ở bản build sau.

## 7. Việc cần làm ngay

- [ ] FE chốt chính xác commit hoặc ticket đã revert để khôi phục vận hành.
- [ ] FE và BE cùng đối chiếu ticket `SAKURA-96573`, phạm vi ảnh hưởng thực tế và điều kiện áp dụng rule `Phòng thực hiện`.
- [ ] Rà lại logic hiển thị droplist phòng trên FE trong case có cân bằng tải và case không dùng cân bằng tải.
- [ ] Kiểm tra lại điều kiện mapping giữa dữ liệu API trả về và dữ liệu FE dùng để hiển thị danh sách phòng.
- [ ] Làm RCA chính thức, chốt nguyên nhân gốc, nguyên nhân lọt test và biện pháp ngăn lặp lại.
- [ ] Bổ sung checklist smoke test sau build cho các luồng sống còn: Tiếp đón, chỉ định khám, chọn Phòng thực hiện.
- [ ] Thống nhất cơ chế báo động riêng cho sự cố nghiêm trọng, tránh trôi tin trong nhóm check-in.

## 8. Bài học và kiến nghị rút kinh nghiệm

- Với thay đổi chạm vào rule chọn `Phòng thực hiện`, phải mô tả rất rõ điều kiện áp dụng, phạm vi ảnh hưởng và case không áp dụng.
- Với các thay đổi chạm vào luồng Tiếp đón, cần coi đây là luồng sống còn và bắt buộc có smoke test ngay sau build hoặc trước giờ vận hành đầu ngày.
- Nếu một ticket có thể ảnh hưởng toàn bộ Tiếp đón, cần review chéo giữa BA, FE, BE và triển khai trước khi release.
- Cần tách riêng kênh hoặc quy ước nhận diện tin nhắn sự cố nghiêm trọng để không bị lẫn trong tin check-in.
- Sau khi khôi phục tạm bằng revert, bắt buộc phải tiếp tục chốt RCA thay vì dừng ở mức “đã chạy lại được”.

## 9. Rủi ro và điểm cần làm rõ

- Rủi ro:
  - Nếu chưa chốt được nguyên nhân gốc, lỗi có thể lặp lại ở bản build kế tiếp.
  - Nếu rule bắt buộc nhập `Phòng thực hiện` còn áp dụng sai phạm vi, các Cơ sở y tế khác cũng có thể gặp lỗi tương tự.
  - Nếu chưa bổ sung test hồi quy cho luồng Tiếp đón, nguy cơ lỗi chặn đầu vào vẫn còn cao.
- Open question:
  - Ticket `SAKURA-96573` thực tế intended cho trường hợp nào?
  - Điều kiện bắt buộc nhập `Phòng thực hiện` phụ thuộc cấu hình nào?
  - Dữ liệu API có đủ nhưng FE không hiển thị do lọc sai, map sai hay do điều kiện ẩn danh sách?
  - Tại sao thay đổi này không bị phát hiện trong bước kiểm tra sau build tối 14/05/2026?

## 10. Kết luận

- Đây là sự cố nghiêm trọng vì chặn toàn bộ khâu Tiếp đón trong khoảng 45 phút vào đầu ngày 15/05/2026.
- Sự cố đã được khôi phục vận hành bằng phương án revert FE và revert ticket nghi ngờ gây lỗi.
- Đến thời điểm lập note, hệ thống đã tiếp đón bình thường trở lại nhưng chưa có kết luận nguyên nhân gốc.
- Cần ưu tiên RCA đầy đủ, chốt rõ ticket gây lỗi, điều kiện áp dụng rule và bổ sung kiểm tra sau release để tránh lặp lại.

## 11. Tài liệu và note liên quan

- [[2026-05-14 - BV Tu Du - Bao cao su co Rabbit tren App 2]]
- Ticket liên quan: `SAKURA-96573`

## 12. Nhật ký cập nhật

- 2026-05-15: Tạo note báo cáo sự cố nghiêm trọng và bản tóm tắt gửi lãnh đạo từ thông tin Hà Nguyễn cung cấp.
