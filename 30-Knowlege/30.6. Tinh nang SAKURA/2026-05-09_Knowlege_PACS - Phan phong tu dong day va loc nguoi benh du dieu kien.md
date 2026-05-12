# Knowledge Note - PACS: Phân phòng tự động đẩy và lọc Người bệnh đủ điều kiện

## Câu hỏi cần trả lời

- Sau khi phân phòng thành công, HIS có thể tự đẩy dịch vụ sang PACS trong trường hợp nào?
- API `GET /api/his/v1/pacs/nguoi-benh` đã hỗ trợ lọc Người bệnh đủ điều kiện thực hiện như thế nào?
- Cờ `thanhToan` trên danh sách Người bệnh PACS đang được tính theo rule nào?
- Các thay đổi này phù hợp với luồng vận hành nào tại Bệnh viện Từ Dũ?

## Tóm tắt ngắn

- Bổ sung thiết lập chung `TU_DONG_DAY_PACS_KHI_PHAN_PHONG` để HIS tự đẩy dịch vụ sang PACS ngay sau khi phân phòng thành công.
- Mục tiêu là giúp PACS nhận đúng phòng thực hiện thực tế, giảm thao tác phân lại phòng trên PACS.
- Bổ sung param `checkin` cho API `GET /api/his/v1/pacs/nguoi-benh` để PACS chỉ lấy danh sách Người bệnh đã qua bước cần thiết.
- API cũng trả thêm các bản ghi ngoại trú có `tienConLai < 0`, đồng thời tính lại cờ `thanhToan` theo dữ liệu thật thay vì gán cứng.

## Điều đã xác nhận

- Fact:
  - Ticket `SAKURA-96012`: thêm chức năng tự động đẩy PACS sau khi phân phòng thành công.
  - Ticket `SAKURA-96004`: cải thiện API `GET /api/his/v1/pacs/nguoi-benh`.
  - Cả 2 ticket đều ở trạng thái `Passed Stable`.

- Fact:
  - Thiết lập mới: `TU_DONG_DAY_PACS_KHI_PHAN_PHONG = True/False`.
  - Nếu `True`:
    - Sau khi API phân phòng chạy thành công, HIS tự gọi luồng đẩy dịch vụ sang PACS.
    - Chỉ đẩy các dịch vụ phân phòng thành công và có `id`, `phongThucHienId` hợp lệ.
  - Nếu `False` hoặc chưa khai báo:
    - Giữ nguyên hành vi cũ, không tự đẩy sang PACS sau phân phòng.

- Fact:
  - Thiết lập này độc lập với `TU_DONG_DAY_PACS_KHI_KE_XOA_DV`.
  - Có thể bật đồng thời cả 2 thiết lập nếu đơn vị cần vừa đẩy khi kê dịch vụ vừa đẩy lại sau khi phân phòng.

- Fact:
  - Lý do nghiệp vụ tại Bệnh viện Từ Dũ:
    - Khi mới kê dịch vụ, dịch vụ có thể đang gắn vào Phòng chung.
    - Chỉ sau khi đủ điều kiện thanh toán và phân phòng thành công mới xác định được Phòng thực hiện thật.
    - Nếu PACS chỉ nhận dữ liệu ở lúc kê dịch vụ thì dễ sai phòng thực hiện, làm người dùng phải phân lại trên PACS.

- Fact:
  - API `GET /api/his/v1/pacs/nguoi-benh` bổ sung param `checkin` kiểu Boolean, không bắt buộc.
  - Rule:
    - `checkin = true`: không trả các dịch vụ ở trạng thái `CHO_TIEP_NHAN`.
    - `checkin = false` hoặc không truyền: giữ hành vi cũ, vẫn trả cả `CHO_TIEP_NHAN`.

- Fact:
  - API bỏ điều kiện lọc cứng chỉ lấy ngoại trú có `tienConLai >= 0`.
  - Sau thay đổi, API vẫn trả cả bản ghi ngoại trú có `tienConLai < 0`.

- Fact:
  - Cờ `thanhToan` trên response được tính lại như sau:
    - Khám sức khỏe: `thanhToan = true`
    - Nội trú: `thanhToan = true`
    - Ngoại trú:
      - `tienConLai >= 0`: `thanhToan = true`
      - `tienConLai < 0` hoặc `null`: `thanhToan = false`

## Giả định

- Assumption:
  - Thay đổi này đang phục vụ trực tiếp cho luồng PACS hoặc QMS CĐHA tại Bệnh viện Từ Dũ.
  - PACS phía nhận sẽ dùng cờ `thanhToan` để tự chặn hoặc cảnh báo các ca chưa đủ điều kiện thực hiện.
  - Khi bật đồng thời 2 thiết lập tự đẩy PACS, đơn vị triển khai đã chấp nhận khả năng một dịch vụ được đẩy ở nhiều thời điểm khác nhau theo đúng chủ đích vận hành.

## Cách áp dụng

- Dùng khi cần giải thích vì sao PACS chỉ nên lấy danh sách Người bệnh đã qua bước check-in hoặc đã phân phòng.
- Dùng khi viết rule test cho luồng:
  - kê dịch vụ
  - thanh toán
  - phân phòng
  - đẩy PACS
  - lọc danh sách Người bệnh thực hiện
- Dùng khi khảo sát cấu hình theo từng Cơ sở y tế:
  - Có cần bật tự đẩy PACS sau khi kê dịch vụ không
  - Có cần bật tự đẩy PACS sau khi phân phòng không
  - PACS có dùng cờ `thanhToan` để chặn thực hiện hay không

## Ví dụ hoặc tình huống dùng

- Tình huống 1:
  - Người bệnh đã đủ điều kiện thanh toán.
  - Người dùng phân phòng thành công.
  - Thiết lập `TU_DONG_DAY_PACS_KHI_PHAN_PHONG = true`.
  - Kết quả mong đợi: HIS tự đẩy dịch vụ sang PACS theo phòng thực hiện vừa phân.

- Tình huống 2:
  - PACS gọi `GET /api/his/v1/pacs/nguoi-benh?...&checkin=true`.
  - Kết quả mong đợi: không nhận các dịch vụ còn ở trạng thái `CHO_TIEP_NHAN`.

- Tình huống 3:
  - Người bệnh ngoại trú còn thiếu tiền, `tienConLai < 0`.
  - Kết quả mong đợi:
    - API vẫn trả bản ghi.
    - `thanhToan = false`.
    - PACS có thể hiển thị nhưng tự chặn thực hiện theo rule của đơn vị.

## Rủi ro hoặc giới hạn

- Nếu bật đồng thời đẩy PACS khi kê dịch vụ và khi phân phòng, cần làm rõ PACS xử lý trùng hoặc cập nhật lại chỉ định như thế nào.
- Luồng đẩy PACS sau phân phòng đang chạy bất đồng bộ, nên cần kiểm tra thêm log nếu phía PACS báo chưa nhận dữ liệu.
- Cần thống nhất rõ ý nghĩa nghiệp vụ của `checkin` giữa HIS, PACS và QMS để tránh mỗi bên hiểu khác nhau.
- Cần kiểm tra thêm tác động với các phân hệ không phải CĐHA nếu cùng dùng chung API hoặc cùng dùng logic lọc trạng thái.

## Nguồn tham chiếu

- [SAKURA-96012](https://jira.isofh.com.vn/browse/SAKURA-96012)
- [SAKURA-96004](https://jira.isofh.com.vn/browse/SAKURA-96004)

## Note liên quan

- [[2026-05-09_Knowlege_QMS CDHA - Benh vien Tu Du]]
