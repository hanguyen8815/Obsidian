# Báo cáo sự cố gửi Ban lãnh đạo và các bên liên quan - Không có `Phòng thực hiện`

## 1. Thông tin chung

- Đơn vị: Bệnh viện Từ Dũ - Cơ sở 1
- Môi trường bị ảnh hưởng: HIS Production
- Phiên bản cập nhật: từ `HIS_SAKURA26_5.2.0` lên `HIS_SAKURA26_5.4.0`
- Thời điểm cập nhật: 20h00 ngày 14/05/2026
- Thời điểm ghi nhận sự cố đầu tiên: 05h59 ngày 15/05/2026
- Thời điểm khôi phục vận hành: khoảng 06h45 ngày 15/05/2026
- Trạng thái hiện tại: Đã vận hành bình thường trở lại, chưa chốt nguyên nhân gốc
- Mức độ: Sự cố nghiêm trọng

## 2. Tóm tắt điều hành

- Sáng 15/05/2026, sau khi cập nhật hệ thống từ `HIS_SAKURA26_5.2.0` lên `HIS_SAKURA26_5.4.0`, toàn bộ quầy Tiếp đón tại Bệnh viện Từ Dũ - Cơ sở 1 gặp lỗi `Không có Phòng thực hiện` khi tiếp đón và chỉ định các Dịch vụ khám.
- Sự cố chặn ngay khâu đầu vào của quy trình khám, làm Người sử dụng không thể tiếp đón Người bệnh trong khung giờ đầu ngày.
- Thời gian ảnh hưởng thực tế khoảng 45 phút, từ lúc ghi nhận 05h59 đến khi vận hành ổn định trở lại khoảng 06h45 ngày 15/05/2026.
- Team Triển khai, FE và BE đã phối hợp kiểm tra. Trong quá trình khoanh vùng, API tổng hợp dịch vụ vẫn trả dữ liệu nhưng droplist chọn phòng trên giao diện không có giá trị để chọn.
- Hướng xử lý đã áp dụng là revert FE trước để khôi phục vận hành, sau đó FE tiếp tục revert ticket nghi ngờ gây lỗi và build lại. Sau khi revert FE, phía BE không cần revert nữa.
- Đến thời điểm lập báo cáo, sự cố đã được xử lý xong ở mức vận hành nhưng chưa có kết luận chính thức về nguyên nhân gốc.

## 3. Mức ảnh hưởng

- Ảnh hưởng trực tiếp tới toàn bộ quầy Tiếp đón.
- Ảnh hưởng tới tất cả Dịch vụ khám ở bước chọn `Phòng thực hiện`.
- Làm dừng chuỗi vận hành ngay từ đầu quy trình khám.
- Tăng nguy cơ ùn tắc khu Tiếp đón và chậm luồng khám nếu thời gian xử lý kéo dài thêm.

## 4. Diễn biến chính

- 14/05/2026 20h00:
  - Thực hiện cập nhật phiên bản từ `HIS_SAKURA26_5.2.0` lên `HIS_SAKURA26_5.4.0`.
- 15/05/2026 05h59:
  - Minh Cơ tại Tiếp đón báo trên nhóm lỗi `Không có phòng thực hiện`.
  - Tin nhắn bị lẫn trong các tin check-in nên Hà Nguyễn và Kiều Trinh không để ý ngay.
- 15/05/2026 06h11:
  - Kiều Trinh confirm lại hệ thống vẫn đang lỗi và bắt đầu kiểm tra.
- 15/05/2026 06h13:
  - Phú Bình gọi cho Hà Nguyễn thông báo `toàn bộ` các quầy Tiếp đón, chỉ định tất cả các Dịch vụ khám đều lỗi `Không có Phòng thực hiện`.
  - Hà Nguyễn confirm với Tôn thì Danh mục không có thay đổi.
- 15/05/2026 06h14:
  - Hà Nguyễn gọi báo Hữu Ánh DEV BE vì nghiệp vụ cân bằng tải do BE xử lý, FE truyền Phòng trống lên.
  - Song song, Hữu Ánh check; Hà Nguyễn và Kiều Trinh xem thiết lập bỏ cân bằng tải.
  - Kiều Trinh gọi xin ý kiến anh Tiến Ngọc, phụ trách Tiếp đón của Bệnh viện, nhưng anh Tiến Ngọc không đồng ý giải pháp cân bằng tải vì NSD không nắm được cách phân phòng và yêu cầu xử lý gấp.
- Sau 06h14 ngày 15/05/2026:
  - Hà Nguyễn và Kiều Trinh nhả lại thiết lập cũ và dò lại PIC, ticket liên quan.
  - Phát hiện ticket `SAKURA-96573` có yêu cầu bắt buộc nhập `Phòng thực hiện`, nhưng không note rõ điều kiện thiết lập hoặc trường hợp cụ thể nào mới bắt buộc nhập `Phòng thực hiện`.
- 15/05/2026 06h23:
  - Hà Nguyễn gọi cho Mai Ngọc Nam để nhờ kiểm tra FE.
- 15/05/2026 06h29:
  - Hữu Ánh cung cấp thêm thông tin API sau vẫn có dữ liệu:

```text
https://api-bvtudu.tudu.com.vn/api/his/v1/dm-dv-ky-thuat/tong-hop?page=0&size=50&active=true&timKiem=KB0078&gioiTinh=2&ngaySinh=1979-04-16&dsKhuVucId=4&loaiDichVu=10&khoaChiDinhId=27&loaiDoiTuongId=4&doiTuongKcb=1&khoaChiDinh=false&dsCoSoKcbId=1
```

  - Kết quả kiểm tra cho thấy API vẫn có dữ liệu nhưng droplist chọn phòng trên giao diện không có giá trị để chọn.
- 15/05/2026 06h31:
  - Hà Nguyễn gọi lại Mai Ngọc Nam cập nhật tình hình.
  - Nam báo chờ 5 phút để phản hồi lại.
  - Hà Nguyễn trao đổi rõ: nếu chưa tìm được nguyên nhân thì cần revert bản build để có thể tiếp đón được.
- 15/05/2026 06h39:
  - Kiều Trinh chưa thấy báo tìm được nguyên nhân nên đã báo cả BE và FE revert code bản build.
- 15/05/2026 06h42:
  - Mai Ngọc Nam FE báo revert FE trước để Tiếp đón thử lại trước, sau đó Nam sẽ revert ticket lỗi và build lại, BE không cần revert nữa.
  - Team đã báo lại phía BE dừng phương án revert.
  - Trinh và Hà đã báo Tiếp đón thực hiện lại tiếp đón Người bệnh và xác nhận đã hoạt động được.
- Từ 15/05/2026 06h45:
  - Tiếp đón vận hành bình thường trở lại.
- 15/05/2026 06h59:
  - Mai Ngọc Nam báo đã hoàn thành revert ticket lỗi và build lại.

## 5. Nhận định sơ bộ

- Chưa đủ căn cứ để kết luận nguyên nhân gốc.
- Có dấu hiệu cho thấy lỗi liên quan thay đổi rule bắt buộc nhập `Phòng thực hiện` trong ticket `SAKURA-96573`, nhưng mô tả ticket chưa nêu rõ điều kiện áp dụng theo thiết lập hoặc theo từng trường hợp nghiệp vụ.
- API vẫn trả dữ liệu nhưng giao diện không hiển thị được danh sách phòng để chọn, nên cần ưu tiên rà soát phần thay đổi phía FE và logic phối hợp với dữ liệu phòng trống.
- Việc chỉ cần revert FE để khôi phục vận hành là dấu hiệu cần tập trung kiểm tra kỹ phần thay đổi trên FE trước.

## 6. Việc cần làm tiếp theo

- [ ] FE chốt chính xác commit hoặc ticket đã revert để khôi phục vận hành.
- [ ] DEV và Chuyên viên cùng đối chiếu ticket `SAKURA-96573`, phạm vi ảnh hưởng thực tế và điều kiện áp dụng rule `Phòng thực hiện` cũng như toàn bộ các ticket khác trong version để tìm ra nguyên nhân gốc.
- [ ] Chốt nguyên nhân gốc và biện pháp phòng ngừa tái diễn.

## 7. Kiến nghị rút kinh nghiệm

- Kiến nghị trước 1 số nội dung:
- Với thay đổi chạm vào rule `Phòng thực hiện`, phải mô tả rất rõ điều kiện áp dụng, phạm vi ảnh hưởng và case không áp dụng.
- Với các thay đổi chạm vào luồng Tiếp đón, cần coi đây là luồng sống còn và bắt buộc có smoke test ngay sau build hoặc trước giờ vận hành đầu ngày.
- Sự cố xảy ra gây tắc luồng nhắn nhóm không thấy reaction thì cần gọi điện luôn.

## 8. Kết luận

- Đây là sự cố nghiêm trọng vì chặn toàn bộ khâu Tiếp đón trong khoảng 45 phút vào đầu ngày 15/05/2026.
- Sự cố đã được khôi phục vận hành bằng phương án revert FE và revert ticket nghi ngờ gây lỗi.
- Đến thời điểm lập báo cáo, hệ thống đã hoạt động bình thường trở lại nhưng chưa chốt nguyên nhân gốc.
- Đề nghị ưu tiên RCA đầy đủ và bổ sung kiểm tra sau release để tránh lặp lại.

## 9. Tài liệu liên quan

- [[2026-05-15 - BV Tu Du - Ghi nhan su co khong co phong thuc hien]]
- [[2026-05-15 - BV Tu Du - Nhat ky su co khong co phong thuc hien]]
- Ticket liên quan: `SAKURA-96573`

## 10. Nhật ký cập nhật

- 2026-05-15: Tạo bản báo cáo sự cố riêng để gửi Ban lãnh đạo và các bên liên quan.
