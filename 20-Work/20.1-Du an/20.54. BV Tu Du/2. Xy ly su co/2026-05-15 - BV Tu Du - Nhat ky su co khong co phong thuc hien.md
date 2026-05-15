---
loai_note: nhat-ky-su-co-ket-noi
vai_tro: trien-khai
linh_vuc: y-te-ket-noi-thiet-bi
ngay_tao: 2026-05-15
tags:
  - trien-khai
  - su-co
  - bv-tu-du
  - tiep-don
  - phong-thuc-hien
---

# Nhật ký sự cố - Không có Phòng thực hiện

## Thông tin sự cố

- Thời gian phát hiện: 05h59 ngày 15/05/2026
- Bệnh viện / khoa phòng: Bệnh viện Từ Dũ - Cơ sở 1 / khu Tiếp đón
- Thiết bị / serial: Không áp dụng
- Hệ thống bị ảnh hưởng: HIS Production, luồng Tiếp đón và chỉ định Dịch vụ khám
- Mức độ ảnh hưởng: Cao

## Mô tả hiện tượng

- Sau khi cập nhật phiên bản từ `HIS_SAKURA26_5.2.0` lên `HIS_SAKURA26_5.4.0` lúc 20h00 ngày 14/05/2026, sáng 15/05/2026 toàn bộ quầy Tiếp đón phát sinh lỗi `Không có Phòng thực hiện`.
- Người sử dụng không thể tiếp đón Người bệnh và không thể chỉ định các Dịch vụ khám vì droplist chọn `Phòng thực hiện` không có giá trị để chọn.
- Sự cố làm dừng khâu đầu vào của quy trình khám trong khoảng 45 phút, từ 05h59 đến khoảng 06h45 ngày 15/05/2026.

## Bước tái hiện / bằng chứng

- Bước tái hiện:
  - 20h00 ngày 14/05/2026, thực hiện cập nhật phiên bản từ `HIS_SAKURA26_5.2.0` lên `HIS_SAKURA26_5.4.0`.
  - Sáng 15/05/2026, Người sử dụng thao tác tiếp đón hoặc chỉ định Dịch vụ khám tại toàn bộ quầy Tiếp đón.
  - Hệ thống yêu cầu nhập `Phòng thực hiện` nhưng droplist không có dữ liệu để chọn.
  - Lỗi xảy ra trên toàn bộ quầy Tiếp đón và với tất cả các Dịch vụ khám theo phản ánh của người dùng tại viện.
- Log / ảnh chụp / video / bản tin mẫu:
  - 05h59 ngày 15/05/2026, Minh Cơ tại Tiếp đón báo trên nhóm lỗi `Không có phòng thực hiện`, nhưng tin nhắn bị lẫn trong các tin check-in nên Hà Nguyễn và Kiều Trinh chưa để ý ngay.
  - 06h11 ngày 15/05/2026, Kiều Trinh confirm lại hệ thống vẫn đang lỗi và bắt đầu kiểm tra.
  - 06h13 ngày 15/05/2026, Phú Bình gọi cho Hà Nguyễn thông báo `toàn bộ` các quầy Tiếp đón, chỉ định tất cả các Dịch vụ khám đều lỗi `Không có Phòng thực hiện`.
  - Cùng thời điểm 06h13 ngày 15/05/2026, Hà Nguyễn confirm với Tôn thì Danh mục không có thay đổi.
  - 06h14 ngày 15/05/2026, Hà Nguyễn gọi báo Hữu Ánh DEV BE vì nghiệp vụ cân bằng tải do BE xử lý, FE truyền Phòng trống lên.
  - Song song, Hữu Ánh kiểm tra; Hà Nguyễn và Kiều Trinh xem thiết lập bỏ cân bằng tải. Tuy nhiên, khi Kiều Trinh gọi xin ý kiến anh Tiến Ngọc, phụ trách Tiếp đón của Bệnh viện, anh Tiến Ngọc không đồng ý giải pháp cân bằng tải vì Người sử dụng không nắm được cách phân phòng và yêu cầu xử lý gấp.
  - Sau đó, Hà Nguyễn và Kiều Trinh nhả lại thiết lập cũ và dò lại PIC, ticket liên quan.
  - Phát hiện ticket `SAKURA-96573` có yêu cầu bắt buộc nhập `Phòng thực hiện`, nhưng không note rõ điều kiện thiết lập hoặc trường hợp cụ thể nào mới bắt buộc nhập `Phòng thực hiện`.
  - 06h23 ngày 15/05/2026, Hà Nguyễn gọi cho Mai Ngọc Nam để nhờ kiểm tra FE.
  - 06h29 ngày 15/05/2026, Hữu Ánh cung cấp thêm thông tin API sau vẫn có dữ liệu:
    - `https://api-bvtudu.tudu.com.vn/api/his/v1/dm-dv-ky-thuat/tong-hop?page=0&size=50&active=true&timKiem=KB0078&gioiTinh=2&ngaySinh=1979-04-16&dsKhuVucId=4&loaiDichVu=10&khoaChiDinhId=27&loaiDoiTuongId=4&doiTuongKcb=1&khoaChiDinh=false&dsCoSoKcbId=1`
  - Kết quả kiểm tra lúc 06h29 ngày 15/05/2026 cho thấy API vẫn có dữ liệu nhưng droplist chọn phòng trên giao diện không có giá trị để chọn.
  - 06h31 ngày 15/05/2026, Hà Nguyễn gọi lại Mai Ngọc Nam cập nhật tình hình. Nam báo chờ 5 phút để phản hồi lại. Hà Nguyễn trao đổi rõ: nếu chưa tìm được nguyên nhân thì cần revert bản build để có thể tiếp đón được.
  - 06h39 ngày 15/05/2026, Kiều Trinh chưa thấy báo tìm được nguyên nhân nên đã báo cả BE và FE revert code bản build.
  - 06h42 ngày 15/05/2026, Mai Ngọc Nam FE báo revert FE trước để Tiếp đón thử lại trước, sau đó Nam sẽ revert ticket lỗi sau và build lại, BE không cần revert nữa.
  - Sau mốc 06h42 ngày 15/05/2026, Trinh và Hà đã báo Tiếp đón thực hiện tiếp đón Người bệnh và xác nhận đã hoạt động được.
  - Từ 06h45 ngày 15/05/2026, hệ thống tiếp đón vận hành bình thường trở lại.
  - 06h59 ngày 15/05/2026, Mai Ngọc Nam báo đã hoàn thành revert ticket lỗi và build lại.

## Đánh giá nhanh nguyên nhân

- Nhóm nguyên nhân nghi ngờ: Phần mềm / cấu hình rule nghiệp vụ
- Nhận định ban đầu:
  - Có dấu hiệu liên quan tới thay đổi rule bắt buộc nhập `Phòng thực hiện` trong ticket `SAKURA-96573`.
  - Ticket chưa mô tả rõ điều kiện thiết lập hoặc trường hợp cụ thể nào mới bắt buộc nhập `Phòng thực hiện`.
  - API vẫn có dữ liệu nhưng FE không hiển thị được danh sách phòng để chọn.
  - Chưa đủ căn cứ để kết luận nguyên nhân gốc.

## Hướng xử lý

- Xử lý tạm thời:
  - Hà Nguyễn và Kiều Trinh từng xem phương án bỏ cân bằng tải, nhưng anh Tiến Ngọc phía Bệnh viện không đồng ý vì Người sử dụng không nắm được cách phân phòng.
  - 06h39 ngày 15/05/2026, Kiều Trinh đề nghị BE và FE cùng revert code bản build nếu chưa tìm được nguyên nhân.
  - 06h42 ngày 15/05/2026, Mai Ngọc Nam FE thực hiện revert FE trước để khôi phục vận hành.
  - Từ khoảng 06h45 ngày 15/05/2026, Tiếp đón hoạt động bình thường trở lại.
- Xử lý gốc:
  - FE tiếp tục revert ticket lỗi nghi ngờ gây sự cố và build lại.
  - 06h59 ngày 15/05/2026, Mai Ngọc Nam báo đã hoàn thành revert ticket lỗi và build lại.
  - Cần làm RCA chính thức để chốt nguyên nhân gốc và biện pháp phòng ngừa tái diễn.
- Owner: Hà Nguyễn, Kiều Trinh, Hữu Ánh DEV BE, Mai Ngọc Nam FE
- Hạn cập nhật: Cần chốt RCA trong ngày làm việc gần nhất sau khi các bên đối chiếu ticket, commit và phạm vi ảnh hưởng

## Kết quả sau xử lý

- Trạng thái hiện tại: Đã khắc phục tạm thời, cần theo dõi và chốt RCA
- Cách xác nhận đã ổn định:
  - Hà Nguyễn và Kiều Trinh đã báo Tiếp đón thực hiện lại tiếp đón Người bệnh và xác nhận đã hoạt động được.
  - Sau revert FE, từ khoảng 06h45 ngày 15/05/2026 hệ thống tiếp đón bình thường trở lại.
- Bài học kinh nghiệm:
  - Với thay đổi chạm vào rule `Phòng thực hiện`, bắt buộc mô tả rõ điều kiện áp dụng và case không áp dụng.
  - Luồng Tiếp đón là luồng sống còn nên phải có smoke test ngay sau build hoặc trước giờ vận hành đầu ngày.
  - Cần cơ chế nhận diện riêng cho tin báo sự cố nghiêm trọng để tránh bị lẫn trong nhóm check-in.
