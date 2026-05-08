---
loai_note: quan-ly-su-co-va-rca
vai_tro: trien-khai
linh_vuc: y-te-ket-noi-thiet-bi
ngay_tao: 2026-04-28
tags:
  - trien-khai
  - su-co
  - rca
  - hplus
  - pacs
  - cplus
  - mat-du-lieu
---

# Quản lý sự cố và phân tích nguyên nhân gốc - HPlus PACS C+ - Yêu cầu C+ xử lý sự cố mất dữ liệu

## 1. Thông tin nhanh

- Mã sự cố: [Chưa có]
- Thời gian phát hiện: [Cần bổ sung]
- Đơn vị / Khoa phòng: HPlus
- Hệ thống / Thiết bị bị ảnh hưởng: HPlus PACS C+
- Người báo sự cố: ISOFH
- Người phụ trách chính: C+ phối hợp cùng ISOFH và HPlus

## 2. Mô tả và mức độ ảnh hưởng

- Hiện tượng: Hệ thống PACS C+ tại HPlus xảy ra sự cố lưu trữ dữ liệu, dẫn đến mất dữ liệu.
- Mức độ ưu tiên: Khẩn cấp
- Phạm vi ảnh hưởng: Dữ liệu hình ảnh và dữ liệu liên quan được HPlus nhập lên hệ thống PACS C+ trong giai đoạn xảy ra sự cố.
- Ảnh hưởng dữ liệu: Có
- Ảnh hưởng đến an toàn Người bệnh: Chưa rõ

## 3. Xử lý ban đầu

- Giả thuyết nguyên nhân ban đầu: Lỗi trong quá trình quản lý và chuyển dữ liệu giữa các khu vực lưu trữ do C+ vận hành.
- Cách xử lý tạm thời: ISOFH gửi yêu cầu chính thức tới C+ để xác nhận trách nhiệm, tiếp tục phối hợp xử lý và chờ phản hồi từ HPlus.
- Hướng xử lý gốc: C+ cần phối hợp khắc phục sự cố, làm rõ trách nhiệm bồi thường và cung cấp quy trình vận hành bảo đảm an toàn, toàn vẹn dữ liệu.
- Trạng thái hiện tại: Đang xử lý
- Mốc cập nhật tiếp theo: Chờ phản hồi từ HPlus và phản hồi chính thức từ C+

## 4. Dòng thời gian xử lý

| Thời gian | Cập nhật | Người thực hiện |
| --- | --- | --- |
| 2025-11 | Nghiệm thu, đưa hệ thống vào sử dụng | HPlus / ISOFH / C+ |
| 2025-11 đến 2026-11 | Thời gian bảo hành 12 tháng | C+ |
| 2026-04-28 | ISOFH tổng hợp căn cứ hợp đồng và đề nghị C+ chịu trách nhiệm xử lý sự cố mất dữ liệu | ISOFH |

## 5. Nguyên nhân gốc

- Nhóm nguyên nhân: Quy trình / Cấu hình / Phần mềm
- Nguyên nhân gốc đã xác nhận: Theo báo cáo sự cố do chính C+ lập, nguyên nhân mất dữ liệu xuất phát từ việc quên chuyển dữ liệu, thiếu sót khi chuyển dữ liệu giữa các phân vùng và thiếu sót trong quản lý khu vực lưu trữ NAS.
- Bằng chứng: Báo cáo sự cố do C+ lập; chứng cứ vận hành hệ thống và nhập dữ liệu của HPlus trên hệ thống PACS C+

## 6. Vì sao xảy ra

- Vì sao 1: Dữ liệu không được chuyển đầy đủ giữa các khu vực lưu trữ.
- Vì sao 2: Quá trình chuyển dữ liệu giữa các phân vùng có thiếu sót.
- Vì sao 3: Khu vực lưu trữ NAS không được quản lý chặt, dẫn đến rủi ro mất dữ liệu.

## 7. Căn cứ trách nhiệm của C+

### 7.1. Về trách nhiệm bảo đảm tính toàn vẹn thông tin, dữ liệu

- Căn cứ Điều 8.3 Hợp đồng số `09.2023/C+-ISOFH/KYVN` về cung cấp hệ thống phần mềm lưu trữ và xử lý hình ảnh VRIMAGE giữa ISOFH và C+.
- PACS cần được bảo đảm cung cấp và vận hành trong thời gian hợp đồng còn hiệu lực.
- Theo Điều 8 của hợp đồng này, C+ có trách nhiệm bảo vệ tính toàn vẹn của thông tin dữ liệu tại địa điểm sử dụng.
- Trường hợp vi phạm, C+ chịu phạt 8% giá trị phần nghĩa vụ bị vi phạm và bồi thường các thiệt hại phát sinh.
- Tại thời điểm xảy ra sự cố, dù chưa có biên bản bàn giao chính thức, C+ vẫn là bên vận hành và bảo trì hệ thống.
- Nếu có chứng cứ khác chứng minh HPlus đã nhập thông tin vào hệ thống và dữ liệu bị mất do lỗi của C+, thì C+ vẫn phải chịu trách nhiệm.

### 7.2. Bổ sung căn cứ từ hợp đồng dịch vụ CNTT

- Căn cứ Điều 4.8 Hợp đồng số `05122025/HPLUS/CPLUS-ISOFH`.
- Nội dung chính: C+ chịu hoàn toàn trách nhiệm nếu thông tin, dữ liệu bị mất an toàn, bị lộ hoặc bị mất tính an toàn thông tin xuất phát từ dịch vụ do C+ cung cấp.
- Báo cáo sự cố do C+ lập đã xác nhận nguyên nhân mất dữ liệu xuất phát từ các thiếu sót trong dịch vụ do C+ cung cấp và vận hành.
- Kết luận làm việc: Căn cứ cả hai hợp đồng, C+ chịu trách nhiệm đối với việc mất dữ liệu tại HPlus.

### 7.3. Về việc C+ đơn phương ngừng thực hiện hợp đồng

- Thời gian nghiệm thu đưa vào sử dụng: tháng 11/2025.
- Thời gian bảo hành: 12 tháng, đến tháng 11/2026.
- Tại thời điểm ghi nhận nội dung này, hệ thống vẫn đang trong thời gian bảo hành.
- Hợp đồng không có quy định cho phép C+ đơn phương ngừng thực hiện hợp đồng trong trường hợp này.
- Nếu C+ đơn phương ngừng thực hiện hợp đồng trái quy định, C+ phải bồi thường các thiệt hại phát sinh cho ISOFH.

## 8. Đề nghị của ISOFH

- `3.1.` C+ chịu trách nhiệm bồi thường chi phí nếu HPlus có yêu cầu bồi thường liên quan đến sự cố mất dữ liệu này.
- `3.2.` Trong thời gian chờ HPlus phản hồi, ISOFH và C+ tiếp tục thương thảo phương án phối hợp cho giai đoạn tiếp theo.
- `3.3.` C+ cung cấp quy trình vận hành bảo đảm an toàn, toàn vẹn dữ liệu khách hàng. Nội dung này ISOFH đã nêu trong email trước nhưng C+ chưa phản hồi. ISOFH đề nghị C+ sớm gửi để các bên cùng chốt.

## 9. Chốt sự cố

- Cách xác nhận đã ổn định: Cần có xác nhận từ HPlus, đối soát lại dữ liệu đã mất và phương án khắc phục từ C+.
- Dữ liệu cần đối soát / chạy bù: Toàn bộ dữ liệu bị ảnh hưởng trong giai đoạn sự cố tại hệ thống PACS C+.
- Trạng thái đóng sự cố: Đóng tạm sau khi có phản hồi chính thức và phương án xử lý được các bên thống nhất.

## 10. Hành động phòng ngừa tái diễn

| Hành động phòng ngừa | Người phụ trách | Hạn hoàn thành | Trạng thái |
| --- | --- | --- | --- |
| C+ gửi quy trình vận hành bảo đảm an toàn, toàn vẹn dữ liệu | C+ | [Cần chốt] | Open |
| C+ làm rõ phương án khắc phục và trách nhiệm bồi thường | C+ | [Cần chốt] | Open |
| ISOFH theo dõi phản hồi từ HPlus và cập nhật hướng phối hợp tiếp theo | ISOFH | [Cần chốt] | Open |

## 11. Rủi ro và điểm cần làm rõ

- Rủi ro: HPlus có thể yêu cầu bồi thường hoặc mở rộng phạm vi ảnh hưởng nếu xác định dữ liệu mất nhiều hơn dự kiến.
- Rủi ro: Nếu C+ chậm phối hợp hoặc ngừng thực hiện nghĩa vụ trong thời gian bảo hành, ISOFH có thể phát sinh thiệt hại với khách hàng.
- Open question: Thời gian phát hiện chính xác và phạm vi dữ liệu bị mất cụ thể là gì.
- Open question: Có biên bản, log hoặc xác nhận vận hành nào cần đính kèm thêm để củng cố hồ sơ làm việc với C+ không.

## 12. Tham chiếu

- Hợp đồng số `09.2023/C+-ISOFH/KYVN`
- Hợp đồng số `05122025/HPLUS/CPLUS-ISOFH`
- Báo cáo sự cố do C+ lập
- Email trao đổi trước đó giữa ISOFH và C+ về quy trình vận hành bảo đảm an toàn dữ liệu
