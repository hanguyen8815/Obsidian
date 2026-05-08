# Knowledge Note - Phụ lục 2 gửi dữ liệu qua API, Công văn 3220/BHXH-CNTT

## Mục tiêu

- Chuyển nội dung chính từ file PDF `Phu luc 2_Gui du lieu qua API_CV 3220_BHXH_CNTT.pdf` sang markdown để dễ tra cứu.
- Tách rõ phần `CT07` và phần `Giấy chứng sinh` để tránh dùng nhầm endpoint hoặc nhầm cấu trúc XML.

## Nguồn

- File gốc: [[Phu luc 2_Gui du lieu qua API_CV 3220_BHXH_CNTT.pdf]]
- Note tổng hợp: [[2026.04.26_thong_tu_22_2025_tt_byt_phu_luc_02_cong_van_3220_bhxh_cntt_ct07_va_giay_chung_sinh]]

## Tóm tắt nhanh

- Phụ lục 2 mô tả liên thông dữ liệu qua dịch vụ web của BHXH Việt Nam.
- Trong file hiện có các nhóm dịch vụ: lấy token, gửi bộ chứng từ có `CT07`, gửi `Giấy báo tử`, gửi `Giấy chứng sinh`.
- `CT07` nằm trong gói chứng từ `loaiHs = 39`.
- `Giấy chứng sinh` dùng endpoint riêng, `loaiHs = 61`, và `fileBase64Str` là base64 của XML `HSDLGCS`.

## Dịch vụ lấy token

- URL: `https://egw.baohiemxahoi.gov.vn/api/token/take`
- Method: `POST`
- Content-Type: `application/x-www-form-urlencoded`
- Input: `username`, `password`
- Output chính: `maKetQua`, `access_token`, `id_token`, `username`, `expires_in`
- Mã kết quả trong phụ lục: `200`, `401`, `402`, `403`, `500`

## Dịch vụ gửi bộ chứng từ có CT07

- Endpoint: `https://egw.baohiemxahoi.gov.vn/api/chungtugw/GuiHoSoChungTu2025`
- Method: `POST`
- `loaiHs = 39`
- XML gói hồ sơ dùng root `HSCHUNGTU`
- Các giá trị `LOAIHOSO` được nêu trong phụ lục: `CT03`, `CT04`, `CT06`, `CT07`, `GIAYDIEUTRINOITRU`, `GIAYDIEUTRIVOSINH`, `GIAYSUCKHOEME`
- Với `CT07`, phần này chỉ cho thấy cách đóng gói và gửi hồ sơ, không phải chuẩn XML của `Giấy chứng sinh`.

## Dịch vụ gửi Giấy chứng sinh theo Thông tư 22/2025/TT-BYT

### Endpoint và tham số

- URL: `https://egw.baohiemxahoi.gov.vn/api/hososuckhoe/guiGiayToDienTu`
- Method: `POST`
- Content-Type: `application/x-www-form-urlencoded`
- Charset: `utf-8`
- Request body:
- `maCskcb`: Mã cơ sở KCB
- `token`: token phiên làm việc
- `id_token`: token ID
- `username`: tài khoản cơ sở KCB
- `password`: mật khẩu cơ sở KCB
- `loaiHs`: `60 = Giấy báo tử`, `61 = Giấy chứng sinh`
- `fileBase64Str`: base64 của XML gốc

### Dữ liệu trả về

- `MaKetQua`
- `MaGD`
- `ThoiGianTiepNhan`: định dạng `yyyyMMddHHmmss`
- Mã lỗi trong phụ lục: `200`, `205`, `401`, `500`, `1001`

### Cấu trúc XML tổng quát

```xml
<HSDLGCS>
  <GIAYCHUNGSINH Id="...">...</GIAYCHUNGSINH>
  <CHUKYDONVI><Signature>...</Signature></CHUKYDONVI>
</HSDLGCS>
```

- Root: `HSDLGCS`
- Nút nghiệp vụ: `GIAYCHUNGSINH`
- Nút chữ ký: `CHUKYDONVI`

### Nhóm trường chính của `GIAYCHUNGSINH`

- Nhóm định danh chứng từ: `MA_GCS`, `MA_BN`, `MA_CT`, `SO_SERI`, `SO`, `QUYEN_SO`, `CAP_LAN_DAU`
- Nhóm mẹ/Người nuôi dưỡng/bên mang thai hộ: `MA_BHXH_NND`, `MA_THE_NND`, `HOTEN_NND`, `NGAYSINH_NND`, `MA_DANTOC_NND`, `MA_QUOCTICH_NND`, `LOAI_GIAYTO_NND`, `SO_CCCD_NND`, `NGAYCAP_CCCD_NND`, `NOICAP_CCCD_NND`, `NOI_CU_TRU_NND`, `MATINH_CU_TRU`, `MAHUYEN_CU_TRU`, `MAXA_CU_TRU`, `HO_TEN_CHA`, `MA_THE_TAM`
- Nhóm thông tin con: `TEN_CON`, `GIOI_TINH_CON`, `SO_CON`, `LAN_SINH`, `SO_CON_SONG`, `CAN_NANG_CON`, `NGAY_SINH_CON`, `NOI_SINH_CON`, `TINH_TRANG_CON`, `SINHCON_PHAUTHUAT`, `SINHCON_DUOI32TUAN`, `GHI_CHU`
- Nhóm người lập và đơn vị: `NGUOI_DO_DE`, `NGUOI_GHI_PHIEU`, `MA_TTDV`, `THU_TRUONG_DVI`, `NGAY_CT`
- Nhóm mang thai hộ: `MA_BHXH_MTH`, `MA_THE_MTH`, `HOTEN_MTH`, `NGAYSINH_MTH`, `MA_DANTOC_MTH`, `MA_QUOCTICH_MTH`, `LOAI_GIAYTO_MTH`, `SO_CCCD_MTH`, `NGAYCAP_CCCD_MTH`, `NOICAP_CCCD_MTH`, `NOI_CU_TRU_MTH`, `MATINH_CU_TRU_MTH`, `MAXA_CU_TRU_MTH`
- Nhóm chồng bên nhờ mang thai hộ: `HO_TEN_CHA_MTH`, `NGAYSINH_CHA_MTH`, `MA_DANTOC_CHA_MTH`, `NOI_CU_TRU_CHA_MTH`, `MATINH_CU_TRU_CHA_MTH`, `MAXA_CU_TRU_CHA_MTH`, `LOAI_GIAYTO_CHA_MTH`, `SO_CCCD_CHA_MTH`, `NGAYCAP_CCCD_CHA_MTH`, `NOICAP_CCCD_CHA_MTH`
- Nhóm cha của mẹ/Người nuôi dưỡng/bên mang thai hộ: `NGAYSINH_CHA_NND`, `MA_DANTOC_CHA_NND`, `LOAI_GIAYTO_CHA_NND`, `SO_CCCD_CHA_NND`, `NGAYCAP_CCCD_CHA_NND`, `NOICAP_CCCD_CHA_NND`

### Quy tắc định dạng cần nhớ

- Ngày kiểu ngày thuần thường dùng `yyyyMMdd`
- `NGAY_SINH_CON` dùng `yyyyMMddHHmmss`
- `GIOI_TINH_CON`: `1 = Nam`, `2 = Nữ`, `3 = Chưa xác định`
- `SINHCON_PHAUTHUAT`, `SINHCON_DUOI32TUAN`, `CAP_LAN_DAU`: `1 = có`, `0 = không`
- `LOAI_GIAYTO_NND`, `LOAI_GIAYTO_MTH`, `LOAI_GIAYTO_CHA_MTH`, `LOAI_GIAYTO_CHA_NND`: phụ lục ghi `1 = CCCD`, `2 = CMND`, `4 = Định danh công dân`, `3 = Hộ chiếu`
- XML phải có khối `CHUKYDONVI`, nên khi tích hợp cần làm rõ rule ký số thực tế

## Lưu ý khi dùng tài liệu này

- `CT07` và `Giấy chứng sinh` là hai luồng kỹ thuật khác nhau trong cùng phụ lục.
- `CT07` đi theo gói chứng từ `HSCHUNGTU`, còn `Giấy chứng sinh` đi theo XML `HSDLGCS`.
- Trong PDF có một dòng mô tả `GIAYCHUNGSINH` nhưng ghi nhầm là `Thông tin giấy báo tử`; nên giữ đây là điểm cần đối chiếu lại khi chốt nghiệm thu.
- Khi cần map field chi tiết hơn, vẫn nên mở lại file PDF gốc để so từng mô tả trường.
