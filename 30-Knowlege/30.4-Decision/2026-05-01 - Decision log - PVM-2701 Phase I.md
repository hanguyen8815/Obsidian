# Nhật ký quyết định - PVM-2701 Phase I

## Bối cảnh

- Cần chốt phạm vi và hướng triển khai Phase I cho bài toán tích hợp thiết bị Monitor PVM-2701 vào HIS.
- Mục tiêu của Phase I là pilot sớm để đánh giá tính khả thi vận hành, độ ổn định kết nối và cách ghi nhận dữ liệu sinh hiệu vào HIS.
- Giai đoạn này ưu tiên kết nối được, truy vết được và xử lý được lỗi, chưa ưu tiên kiến trúc mở rộng hoặc dashboard nâng cao.

## Danh sách quyết định

| Ngày | Quyết định | Lý do | Tác động | Người chốt |
| --- | --- | --- | --- | --- |
| 2026-05-01 | Triển khai Phase I theo mô hình pilot với 1 Tool trung gian cài trên 1 server trung tâm. | Giảm phạm vi để triển khai nhanh, dễ kiểm chứng quy trình và kết nối thực tế trước khi mở rộng. | Chấp nhận điểm lỗi đơn trong giai đoạn đầu. Cần đánh giá lại sau pilot nếu mở rộng số lượng thiết bị hoặc vùng mạng. | Ha.NT |
| 2026-05-01 | Tool là thành phần trung gian giao tiếp với Monitor và HIS. | Tách phần giao tiếp thiết bị ra khỏi HIS, dễ xử lý log, retry, resend và điều chỉnh rule linh hoạt hơn. | Cần vận hành thêm 1 thành phần trung gian và theo dõi sức khỏe của Tool. | Ha.NT |
| 2026-05-01 | Tool áp rule lọc theo thời gian trước khi gửi dữ liệu sinh hiệu sang HIS trong Phase I. | Hạn chế HIS ghi nhận quá nhiều dữ liệu chưa cần thiết trong giai đoạn đầu. | Cần chốt rõ rule lọc, lưu vết dữ liệu bị loại và đánh giá lại sau pilot để quyết định có giữ rule tại Tool hay đưa về HIS. | Ha.NT |
| 2026-05-01 | `Mã hồ sơ` dùng để tích hợp được xem là mã định danh duy nhất tương ứng với 1 mã vào viện, 1 lượt khám hoặc 1 đợt điều trị. | Bảo đảm việc ghép đúng Người bệnh theo thiết kế hiện tại của HIS. | Giảm rủi ro nhầm Người bệnh, nhưng cần giữ cách diễn đạt này rõ ràng trong tài liệu để tránh team khác hiểu `Mã hồ sơ` là mã Người bệnh dùng chung. | Ha.NT |
| 2026-05-01 | HIS xử lý chống trùng theo `Mã hồ sơ + thời điểm đo`. | Tận dụng cơ chế sẵn có của HIS để ngăn tạo lặp bản ghi sinh hiệu. | Tool vẫn phải quản lý trạng thái đã gửi, lỗi, chờ gửi lại để tránh gửi lặp không kiểm soát. | Ha.NT |
| 2026-05-01 | Phase I chỉ tích hợp các chỉ số đã có căn cứ từ bản tin thực tế của Monitor. | Tránh cam kết vượt quá khả năng thật của thiết bị hoặc bản tin hiện có. | Loại bỏ khỏi danh sách Phase I các trường viết thừa hoặc chưa có căn cứ như Nhóm máu, AVPU, Glasgow, chiều cao, cân nặng, PI. | Ha.NT |
| 2026-05-01 | Truy vết thiết bị trong Phase I được ưu tiên quản lý trên Tool trước. | Giảm phạm vi sửa đổi bảng nghiệp vụ của HIS trong giai đoạn pilot. | HIS chưa sử dụng Mã máy ở bảng nghiệp vụ sinh hiệu trong Phase I. Cần đánh giá đưa xuống HIS ở giai đoạn sau nếu cần truy vết nghiệp vụ sâu hơn. | Ha.NT |

## Phương án đã cân nhắc

- Phương án 1:
  Đẩy đầy đủ mọi bản ghi sinh hiệu sang HIS, sau đó để HIS lọc theo chu kỳ ghi nhận.
- Phương án 2:
  Lọc dữ liệu ngay tại Tool rồi chỉ gửi các bản ghi đủ điều kiện sang HIS.
- Phương án được chọn:
  Chọn Phương án 2 cho Phase I để giảm tải cho HIS và triển khai pilot nhanh hơn.

## Điểm cần theo dõi sau quyết định

- Độ ổn định của mô hình 1 server trung tâm.
- Hiệu quả của rule lọc thời gian tại Tool.
- Số lượng bản ghi bị loại và ảnh hưởng nghiệp vụ thực tế.
- Tỷ lệ bản ghi lỗi, resend và retry.
- Nhu cầu đưa Mã máy xuống HIS ở giai đoạn tiếp theo.
- Nhu cầu mở rộng nhiều server hoặc collector khi tăng số lượng Monitor.

## Note liên quan

- [[2026-05-01_NV_Tich hop thiet bi PVM-2701 vao HIS - Phase I]]
