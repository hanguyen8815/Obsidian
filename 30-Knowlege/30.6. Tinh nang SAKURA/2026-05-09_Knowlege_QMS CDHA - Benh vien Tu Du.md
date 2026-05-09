# Knowledge Note - QMS CĐHA - Bệnh viện Từ Dũ

## Câu hỏi cần trả lời

- QMS CĐHA tại Bệnh viện Từ Dũ đang lấy dữ liệu từ API nào?
- Người bệnh được đẩy qua các danh sách trên QMS theo rule trạng thái nào?
- Khi PACS đổi phòng thực hiện thì danh sách QMS giữa các phòng thay đổi ra sao?
- Khi nào Người bệnh bị loại khỏi danh sách QMS?

## Tóm tắt ngắn

- API QMS: `{{sakura-server}}/api/his/v1/nb-dv-cdha-tdcn-pt-tt/qms?dsPhongThucHienId=659`
- `dsDangThucHien (63)`: Người bệnh đang thực hiện.
- `dsTiepTheo (43)`: Người bệnh chờ thực hiện, hiển thị ở danh sách “Xin mời Người bệnh vào”.
- Khi chỉ định còn ở trạng thái `25 - Chờ tiếp nhận` thì chưa đẩy lên QMS.
- Khi đủ điều kiện thanh toán và gọi API phân phòng thành công thì Người bệnh được đẩy lên `dsDaXacNhan (35)`.
- Khi có Người bệnh chuyển từ `dsTiepTheo (43)` sang `dsDangThucHien (63)`, hệ thống tự đẩy Người bệnh kế tiếp từ `dsDaXacNhan (35)` lên `dsTiepTheo (43)` theo thiết lập số lượng tối đa ở ô tiếp theo.
- Khi PACS nhận ca và gọi API cập nhật trạng thái, Người bệnh sẽ được chuyển từ `dsTiepTheo (43)` sang `dsDangThucHien (63)`.
- Khi PACS trả kết quả, Người bệnh đổi sang trạng thái đã có kết quả trên HIS và bị loại khỏi danh sách QMS.

## Điều đã xác nhận

- Fact:
  - API QMS dùng để lấy danh sách CĐHA: `{{sakura-server}}/api/his/v1/nb-dv-cdha-tdcn-pt-tt/qms?dsPhongThucHienId=659`.
  - Trên QMS:
    - `dsDangThucHien (63)` là danh sách Người bệnh đang thực hiện.
    - `dsTiepTheo (43)` là danh sách Người bệnh chờ thực hiện.
  - Rule thay đổi trạng thái liên quan QMS:
    - Bước 1: Khi chỉ định dịch vụ ở trạng thái `25 - Chờ tiếp nhận` thì chưa đẩy lên QMS.
    - Bước 2: Khi đủ điều kiện thanh toán, nếu gọi API `phan-phong` thành công thì đẩy Người bệnh lên `dsDaXacNhan (35)`.
    - Bước 3: Dựa trên thiết lập thông số hàng đợi về số lượng Người bệnh tối đa hiển thị ở ô tiếp theo, khi có Người bệnh chuyển từ `dsTiepTheo (43)` sang `dsDangThucHien (63)`, hệ thống tự đẩy Người bệnh từ `dsDaXacNhan (35)` lên `dsTiepTheo (43)`.
    - Bước 4: Khi PACS gọi API cập nhật trạng thái `{{sakura-server}}/api/his/v1/pacs/trang-thai` để nhận ca, HIS đổi trạng thái dịch vụ từ chờ tiếp nhận sang đã thực hiện hoặc đang thực hiện theo luồng đang dùng, đồng thời QMS chuyển Người bệnh từ `dsTiepTheo (43)` lên `dsDangThucHien (63)`.
    - Bước 5: Khi PACS trả kết quả, trên HIS dịch vụ đổi từ đã tiếp nhận sang đã có kết quả, khi đó Người bệnh bị loại khỏi danh sách QMS.
  - Trường hợp PACS đổi phòng thực hiện:
    - Nếu trên HIS đang ở Phòng 1 nhưng PACS gọi cập nhật trạng thái sang Phòng 2, thì trên QMS Phòng 2 Người bệnh sẽ vào `dsDangThucHien (63)`.
    - Trên QMS Phòng 1, Người bệnh vẫn giữ nguyên trạng thái ban đầu, có thể vẫn nằm ở `dsTiepTheo (43)` hoặc `dsDaXacNhan (35)` tùy thời điểm PACS gọi.
    - Nếu PACS gọi lại đổi từ Phòng 2 về Phòng 1 nhưng trên HIS không phát sinh đổi trạng thái mới, QMS Phòng 1 vẫn giữ nguyên trạng thái cũ, không tự cập nhật lại.

## Giả định

- Assumption:
  - Mã trạng thái `35`, `43`, `63` là mã danh sách nội bộ của QMS đang dùng tại Bệnh viện Từ Dũ.
  - `dsPhongThucHienId=659` là ví dụ cho một phòng thực hiện cụ thể, cần thay theo từng phòng khi tra cứu thực tế.
  - Trạng thái HIS tại bước PACS nhận ca cần kiểm tra lại cách đặt tên hiển thị trong từng màn hình, vì ghi chú đầu vào đang dùng lẫn các cụm như `Đã tiếp nhận`, `Đã thực hiện`, `Đang thực hiện`.

## Cách áp dụng

- Dùng note này khi phân tích sự khác nhau giữa danh sách HIS, QMS và PACS ở phân hệ CĐHA.
- Dùng để giải thích vì sao Người bệnh có thể xuất hiện ở QMS Phòng 2 nhưng vẫn còn danh sách cũ ở QMS Phòng 1.
- Dùng làm căn cứ viết rule nghiệp vụ, test scenario hoặc trao đổi với đội PACS khi có lệch trạng thái hàng đợi.
- Khi kiểm tra sự cố, cần đối chiếu theo thứ tự:
  - Trạng thái dịch vụ trên HIS.
  - Phòng thực hiện đang gắn trên HIS.
  - PACS đã gọi API `pacs/trang-thai` hay chưa.
  - PACS đã trả kết quả hay chưa.
  - Thiết lập số lượng tối đa hiển thị ở ô tiếp theo trên hàng đợi.

## Ví dụ hoặc tình huống dùng

- Tình huống 1:
  - Người bệnh đã thanh toán đủ điều kiện.
  - API phân phòng gọi thành công.
  - Kết quả mong đợi: Người bệnh xuất hiện ở `dsDaXacNhan (35)`.

- Tình huống 2:
  - QMS đang có Người bệnh được nhận ca từ `dsTiepTheo (43)` sang `dsDangThucHien (63)`.
  - Kết quả mong đợi: hệ thống tự đẩy thêm một Người bệnh từ `dsDaXacNhan (35)` lên `dsTiepTheo (43)` nếu còn dữ liệu chờ.

- Tình huống 3:
  - HIS đang gắn Phòng 1.
  - PACS nhận ca và cập nhật sang Phòng 2.
  - Kết quả mong đợi:
    - QMS Phòng 2 có Người bệnh ở `dsDangThucHien (63)`.
    - QMS Phòng 1 không tự xóa bản ghi cũ.

- Tình huống 4:
  - PACS trả kết quả.
  - Kết quả mong đợi: Người bệnh biến mất khỏi danh sách QMS.

## Rủi ro hoặc giới hạn

- Ghi chú đầu vào đang mô tả trạng thái HIS theo nhiều cách gọi khác nhau, dễ gây hiểu sai khi viết test hoặc đối chiếu log.
- Khi PACS đổi phòng thực hiện, dữ liệu QMS giữa phòng cũ và phòng mới không tự đồng bộ ngược, dễ tạo cảm giác trùng hoặc sai hàng đợi nếu chỉ nhìn một màn hình.
- Chưa có thông tin rõ về rule dọn dữ liệu tồn ở phòng cũ khi đổi phòng sau khi PACS đã nhận ca.
- Chưa có thông tin về cơ chế retry, timeout hoặc xử lý khi API PACS gọi thất bại.

## Nguồn tham chiếu

- Ghi chú nội bộ: QMS CĐHA - Bệnh viện Từ Dũ.

## Note liên quan

- [[ ]]
