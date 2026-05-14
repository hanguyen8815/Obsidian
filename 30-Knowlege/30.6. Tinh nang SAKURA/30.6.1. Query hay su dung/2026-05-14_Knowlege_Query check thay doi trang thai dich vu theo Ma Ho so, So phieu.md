# Knowledge Note - Query check thay đổi trạng thái dịch vụ theo Mã Hồ sơ, Số phiếu

## Câu hỏi cần trả lời

- Dùng query nào để kiểm tra lịch sử thay đổi trạng thái dịch vụ kỹ thuật?
- Cần lọc theo những thông tin nào để truy ra đúng bản ghi?
- Kết quả trả về giúp đối chiếu trạng thái theo thứ tự thời gian như thế nào?

## Tóm tắt ngắn

- Query này dùng để tra log thay đổi trạng thái dịch vụ kỹ thuật theo `Mã Hồ sơ` và `Số phiếu`.
- Dữ liệu được lấy từ bảng log `LOG_NB_DV_KY_THUAT`, nối với bảng `nb_dich_vu_tong_hop` để lọc theo hồ sơ thực tế.
- Kết quả được sắp xếp theo `THOI_GIAN_THAY_DOI_BAN_GHI` để dễ nhìn thứ tự thay đổi trạng thái.

## Điều đã xác nhận

- Fact:
  - Bảng log tra cứu: `LOG_NB_DV_KY_THUAT` alias `LNDKT`.
  - Bảng đối chiếu dịch vụ: `nb_dich_vu_tong_hop` alias `ndvth`.
  - Điều kiện nối bảng: `LNDKT.id = ndvth.id`.
  - Điều kiện lọc mẫu:
    - `ndvth.MA_HO_SO = '2605131130'`
    - `ndvth.SO_PHIEU = '14038'`
  - Kết quả có lấy riêng cột `LNDKT.TRANG_THAI` và toàn bộ cột của bảng log `LNDKT.*`.
  - Thứ tự hiển thị: tăng dần theo `THOI_GIAN_THAY_DOI_BAN_GHI`.

## Giả định

- Assumption:
  - `MA_HO_SO` và `SO_PHIEU` là cặp thông tin đủ để khoanh vùng đúng dịch vụ cần kiểm tra trong ngữ cảnh đang tra cứu.
  - Trường `TRANG_THAI` trong log phản ánh trạng thái dịch vụ tại từng thời điểm ghi log.

## Cách áp dụng

- Dùng khi cần kiểm tra dịch vụ đã đi qua những trạng thái nào theo thời gian.
- Dùng khi đối chiếu giữa màn hình nghiệp vụ, log hệ thống và dữ liệu bảng dịch vụ tổng hợp.
- Khi tra cứu thực tế, thay giá trị `MA_HO_SO` và `SO_PHIEU` bằng hồ sơ cần kiểm tra.

## Ví dụ hoặc tình huống dùng

- Tình huống 1:
  - Người dùng phản ánh dịch vụ đổi trạng thái không đúng kỳ vọng.
  - Cách dùng: thay `MA_HO_SO`, `SO_PHIEU` vào query để xem chuỗi log thay đổi trạng thái.

- Tình huống 2:
  - Cần kiểm tra vì sao dịch vụ đã thực hiện nhưng màn hình chưa cập nhật đúng.
  - Cách dùng: đối chiếu mốc `THOI_GIAN_THAY_DOI_BAN_GHI` với thời điểm thao tác thực tế hoặc thời điểm hệ thống tích hợp trả dữ liệu.

## Rủi ro hoặc giới hạn

- Nếu `MA_HO_SO` hoặc `SO_PHIEU` nhập sai thì dễ không ra dữ liệu hoặc đối chiếu nhầm bản ghi.
- Query đang ưu tiên xem log thay đổi trạng thái, chưa tự giải thích nguyên nhân nghiệp vụ gây ra thay đổi đó.
- Cần kết hợp thêm thông tin màn hình, người thao tác hoặc log tích hợp nếu cần phân tích sâu nguyên nhân.

## Nguồn tham chiếu

- Ghi chú nội bộ: Query check thay đổi trạng thái dịch vụ theo Mã Hồ sơ, Số phiếu.

## Query SQL

```sql
select LNDKT.TRANG_THAI, LNDKT.*
from LOG_NB_DV_KY_THUAT LNDKT
left join nb_dich_vu_tong_hop ndvth on LNDKT.id = ndvth.id
where ndvth.MA_HO_SO = '2605131130'
  and ndvth.SO_PHIEU = '14038'
order by THOI_GIAN_THAY_DOI_BAN_GHI;
```

## Note liên quan

- [[2026-05-09_Knowlege_PACS - Phan phong tu dong day va loc nguoi benh du dieu kien]]
- [[2026-05-09_Knowlege_QMS CDHA - Benh vien Tu Du]]
