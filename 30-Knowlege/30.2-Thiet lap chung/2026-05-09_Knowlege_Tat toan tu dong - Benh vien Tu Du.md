# Knowledge Note - Tất toán tự động - Bệnh viện Từ Dũ

## Câu hỏi cần trả lời

- Hồ sơ của Người bệnh Không BH cần thỏa điều kiện nào để được tất toán tự động?
- Cần chuẩn bị các danh mục và thiết lập chung nào để job tất toán tự động chạy được?

## Tóm tắt ngắn

- Bắt đầu từ tối nay, đã thống nhất với anh Hậu TCKT cho phép tất toán tự động với Người bệnh Không BH khi hồ sơ thỏa đồng thời các điều kiện sau:
- Tất cả dịch vụ khám của hồ sơ đã đóng.
- Tiền tạm ứng lớn hơn hoặc bằng số tiền Người bệnh chưa thanh toán.
- Dự kiến job tự động chạy theo 2 khung giờ: 20h và 21h.
- Cần tạo thêm 1 bộ ở các danh mục và thiết lập các thiết lập chung liên quan để dùng cho cấu hình này.

## Điều đã xác nhận

- Fact: Phạm vi áp dụng hiện tại là Người bệnh Không BH.
- Fact: Điều kiện tất toán tự động gồm:
- Tất cả dịch vụ khám của hồ sơ đã đóng.
- Tiền tạm ứng lớn hơn hoặc bằng tiền Người bệnh chưa thanh toán.
- Fact: Tài khoản dùng cho cấu hình là `TTTD - Tất toán tự động`.
- Fact: Quầy dùng cho cấu hình là `Quầy tất toán tự động`.
- Fact: Các thiết lập chung liên quan gồm:
- `TAI_KHOAN_MAC_DINH_THU_TIEN_TAT_TOAN_TU_DONG`
- `MA_CA_MAC_DINH_THU_TIEN_TAT_TOAN_TU_DONG`
- `MA_QUAY_MAC_DINH_THU_TIEN_TAT_TOAN_TU_DONG`
- `HOAN_UNG_TT_VA_SINH_TAM_UNG_KET_CHUYEN`
- `LY_DO_TAM_UNG_KET_CHUYEN`
- Fact: Danh sách tên đi kèm yêu cầu gồm:
- Hà Nguyễn
- c Hà Nguyễn
- Nguyệt Nguyễn Thị Thu
- Minh Nguyễn Phạm Nhật
- Anh Nguyễn Mai
- Cơ Trương Minh

## Giả định

- Assumption: `hs` là hồ sơ.
- Assumption: `Không BH` là Người bệnh không có BHYT.
- Assumption: Nội dung "có tạo thêm 1 bộ ở các danh mục" là tạo thêm bộ danh mục phục vụ riêng cho tất toán tự động.
- Assumption: Danh sách tên đi kèm là các cá nhân liên quan đến việc thiết lập hoặc sử dụng cấu hình này.

## Cách áp dụng

- Tạo bổ sung bộ danh mục phục vụ tất toán tự động.
- Tạo tài khoản thu tiền mặc định: `TTTD - Tất toán tự động`.
- Tạo quầy thu tiền mặc định: `Quầy tất toán tự động`.
- Thiết lập các tham số cấu hình liên quan:
- `TAI_KHOAN_MAC_DINH_THU_TIEN_TAT_TOAN_TU_DONG`
- `MA_CA_MAC_DINH_THU_TIEN_TAT_TOAN_TU_DONG`
- `MA_QUAY_MAC_DINH_THU_TIEN_TAT_TOAN_TU_DONG`
- `HOAN_UNG_TT_VA_SINH_TAM_UNG_KET_CHUYEN`
- `LY_DO_TAM_UNG_KET_CHUYEN`
- Thiết lập job tự động chạy vào 20h và 21h mỗi ngày.
- Khi đến giờ chạy, hệ thống quét hồ sơ Người bệnh Không BH và tự tất toán nếu đủ đồng thời 2 điều kiện đã thống nhất.

## Ví dụ hoặc tình huống dùng

- Hồ sơ A: tất cả dịch vụ khám đã đóng, tiền tạm ứng 500.000, tiền Người bệnh chưa thanh toán 450.000 thì đủ điều kiện tất toán tự động.
- Hồ sơ B: tất cả dịch vụ khám đã đóng, tiền tạm ứng 300.000, tiền Người bệnh chưa thanh toán 450.000 thì chưa đủ điều kiện tất toán tự động.
- Hồ sơ C: tiền tạm ứng đủ nhưng vẫn còn dịch vụ khám chưa đóng thì chưa được tất toán tự động.

## Rủi ro hoặc giới hạn

- Cần xác nhận rõ `Không BH` có đúng là không có BHYT hay không.
- Cần xác nhận rõ "dịch vụ khám đã đóng" là đã thu tiền xong hay đã hoàn tất trạng thái nghiệp vụ.
- Cần xác nhận vai trò của danh sách tên đi kèm: người dùng, người phối hợp hay người được gán vào bộ danh mục.
- Cần xác nhận giá trị cụ thể của `MA_CA_MAC_DINH_THU_TIEN_TAT_TOAN_TU_DONG`, `HOAN_UNG_TT_VA_SINH_TAM_UNG_KET_CHUYEN`, `LY_DO_TAM_UNG_KET_CHUYEN`.
- Cần làm rõ cách xử lý khi job 20h không đủ điều kiện nhưng 21h đủ điều kiện.

## Nguồn tham chiếu

- Trao đổi nội bộ về cấu hình tất toán tự động tại Bệnh viện Từ Dũ.

## Note liên quan

- [[ ]]
