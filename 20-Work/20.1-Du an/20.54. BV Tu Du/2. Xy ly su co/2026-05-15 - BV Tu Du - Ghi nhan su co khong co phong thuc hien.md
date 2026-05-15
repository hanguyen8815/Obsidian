# Work Note - Ghi nhận sự cố không có Phòng thực hiện

## Mục tiêu

- Ghi nhận diễn biến sự cố `Không có Phòng thực hiện` tại Bệnh viện Từ Dũ.
- Lưu lại mốc thời gian phối hợp giữa Triển khai, FE và BE để phục vụ truy vết.

## Bối cảnh

- Tối 14/05/2026, hệ thống được cập nhật từ `HIS_SAKURA26_5.2.0` lên `HIS_SAKURA26_5.4.0`.
- Sáng 15/05/2026, toàn bộ quầy Tiếp đón phát sinh lỗi `Không có Phòng thực hiện` khi tiếp đón và chỉ định các Dịch vụ khám.
- API kiểm tra vẫn có dữ liệu, nhưng droplist chọn phòng trên giao diện không có giá trị để chọn.
- FE đã thực hiện revert để khôi phục vận hành. Đến thời điểm lập note, hệ thống đã tiếp đón bình thường trở lại nhưng chưa chốt nguyên nhân gốc.

## Phạm vi

- Bao gồm:

    - Ghi nhận diễn biến sự cố từ lúc cập nhật phiên bản tối 14/05/2026 đến khi khôi phục vận hành sáng 15/05/2026.
    - Ghi nhận hướng xử lý tạm thời và phối hợp giữa các bên liên quan.
- Không bao gồm:

    - Kết luận nguyên nhân gốc cuối cùng.
    - Biên bản RCA hoặc kế hoạch phòng ngừa tái diễn.

## Trạng thái

- Trạng thái hiện tại: Từ khoảng 06h45 ngày 15/05/2026, Tiếp đón đã vận hành bình thường trở lại. Chưa chốt nguyên nhân gốc.
- Người liên quan:

    - Minh Cơ
    - Hà Nguyễn
    - Kiều Trinh
    - Phú Bình
    - Tôn
    - Hữu Ánh DEV BE
    - Mai Ngọc Nam FE
    - Anh Tiến Ngọc - Phụ trách Tiếp đón của Bệnh viện
- Mốc thời gian:

    - 14/05/2026 20h00: Cập nhật phiên bản.
    - 15/05/2026 05h59: Ghi nhận lỗi đầu tiên trên nhóm.
    - 15/05/2026 06h13 - 06h31: Khoanh vùng nguyên nhân và đối chiếu ticket, API.
    - 15/05/2026 06h42: FE revert để khôi phục vận hành.
    - 15/05/2026 06h45: Tiếp đón hoạt động bình thường trở lại.
    - 15/05/2026 06h59: FE báo hoàn thành revert ticket lỗi và build lại.

## Khoanh vùng ảnh hưởng

- Ảnh hưởng tới NSD:

    - Toàn bộ quầy Tiếp đón không thể tiếp đón Người bệnh.
    - Tất cả các Dịch vụ khám đều lỗi do không chọn được `Phòng thực hiện`.
    - Người sử dụng không thể thao tác tiếp đón và chỉ định theo quy trình bình thường.
- Ảnh hưởng nghiêm trọng đến vận hành đầu ngày:

    - Sự cố chặn toàn bộ khâu đầu vào của quy trình khám.
    - Trong khung giờ đầu ngày, nguy cơ ùn tắc khu Tiếp đón và chậm toàn bộ luồng khám tăng cao.
    - Thời gian ảnh hưởng thực tế khoảng 45 phút, từ 05h59 đến khoảng 06h45 ngày 15/05/2026.

## Nội dung chính

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

## Việc cần làm

- [ ] FE chốt chính xác commit hoặc ticket đã revert để khôi phục vận hành.
- [ ] DEV và Chuyên viên cùng đối chiếu ticket `SAKURA-96573`, phạm vi ảnh hưởng thực tế và điều kiện áp dụng rule `Phòng thực hiện` cũng như toàn bộ các ticket khác trong version để tìm ra nguyên nhân gốc.
- [ ] Chốt nguyên nhân gốc và biện pháp phòng ngừa tái diễn.

## Business rules hoặc lưu ý thực thi

- `Phòng thực hiện` là dữ liệu bắt buộc đối với luồng Tiếp đón và chỉ định Dịch vụ khám trong bối cảnh vận hành hiện tại của Bệnh viện Từ Dũ.
- Nếu thay đổi rule bắt buộc nhập `Phòng thực hiện`, phải mô tả rõ điều kiện áp dụng theo cấu hình hoặc theo từng case nghiệp vụ.
- Chưa đủ căn cứ để kết luận nguyên nhân gốc chỉ từ hiện trạng vận hành và thông tin đối chiếu ban đầu.
- Cần đối chiếu thêm ticket, commit FE, logic cân bằng tải và dữ liệu API trước khi chốt nguyên nhân chính thức.

## Rủi ro và điểm cần làm rõ

- Rủi ro:
  - Nếu chưa chốt được nguyên nhân gốc, lỗi có thể lặp lại ở bản build kế tiếp.
  - Nếu rule bắt buộc nhập `Phòng thực hiện` còn áp dụng sai phạm vi, các Cơ sở y tế khác cũng có thể gặp lỗi tương tự.
  - Nếu chưa bổ sung smoke test cho luồng Tiếp đón, nguy cơ lỗi chặn đầu vào vẫn còn cao.
- Open question:
  - Ticket `SAKURA-96573` thực tế intended cho trường hợp nào?
  - Điều kiện bắt buộc nhập `Phòng thực hiện` phụ thuộc cấu hình nào?
  - Dữ liệu API có đủ nhưng FE không hiển thị do lọc sai, map sai hay do điều kiện ẩn danh sách?
  - Tại sao thay đổi này không bị phát hiện trong bước kiểm tra sau build tối 14/05/2026?

## Tài liệu và note liên quan

- [[2026-05-15 - BV Tu Du - Nhat ky su co khong co phong thuc hien]]
- [[2026-05-15 - BV Tu Du - Bao cao su co nghiem trong khong co phong thuc hien]]
- Ticket liên quan: `SAKURA-96573`

## Nhật ký cập nhật

- 2026-05-15: Tạo note ghi nhận sự cố theo mẫu `2026-05-13 - BV Tu Du - Su co luu bao loi xu ly du lieu.md`.
