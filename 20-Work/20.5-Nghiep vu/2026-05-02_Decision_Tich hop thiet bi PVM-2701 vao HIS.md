# Decision log - Tích hợp thiết bị PVM-2701 vào HIS

## Thông tin chung

- Ngày chốt: 02/05/2026
- Người xác nhận: Hà Nguyễn
- Phạm vi: Tài liệu nghiệp vụ high level cho kết nối Monitor PVM-2701 với HIS
- Tài liệu liên quan:
  - `2026-05-01_NV_Tich hop thiet bi PVM-2701 vao HIS - Phase I.md`
  - `2026-05-02_NV_Tich hop thiet bi PVM-2701 vao HIS - Phase II.md`
  - `2026-05-02_SRS_High level_Tich hop thiet bi PVM-2701 vao HIS.md`

## Các quyết định đã chốt

### 1. Phạm vi pilot

- Pilot triển khai tại `MEDI Plus`.
- Số lượng Monitor thực tế sẽ chốt khi triển khai.
- Mô hình mạng thực tế sẽ do team triển khai làm việc với Cơ sở y tế để khởi tạo phương án tối ưu.
- Mô hình cài Tool giai đoạn đầu là `1 máy tính trung tâm`.

### 2. Rule ghi nhận dữ liệu sinh hiệu

- Trong cùng `1 chu kỳ` ghi nhận, hệ thống lấy `bản ghi cuối cùng` để gửi sang HIS.
- Chu kỳ ghi nhận sinh hiệu được cấu hình theo mô hình kết hợp:
  - `Cơ sở y tế`
  - `Khoa/Phòng`
  - `Người bệnh`
- Khi có cấu hình riêng trên Người bệnh thì ưu tiên cấu hình của Người bệnh.

### 3. Rule mapping và truy vết thiết bị

- HIS sử dụng `Danh mục Mã máy` đã có sẵn để mapping thiết bị.
- Không tạo mới một danh mục nghiệp vụ khác cho thiết bị đo trong phạm vi hiện tại.
- Khi thiếu hoặc không map được `Mã máy`, HIS phản hồi cho Tool để theo dõi và thực hiện mapping.

### 4. Trạng thái giao dịch gửi HIS

- Bộ trạng thái quản lý giao dịch gửi HIS gồm:
  - `Chờ gửi HIS`
  - `Đã gửi HIS`
  - `Gửi HIS lỗi`
- Bổ sung trường `Lý do lỗi` để lưu chi tiết lỗi khi gửi sang HIS.
- Ví dụ lỗi: sai `Mã máy`, mất kết nối, lỗi API, lỗi dữ liệu.

### 5. Cách xử lý lỗi và vận hành

- Khi gửi HIS lỗi, phải có `bảng theo dõi transaction lỗi` để tra cứu và resend.
- Kênh cảnh báo vận hành hiện tại chỉ dùng `log nội bộ`.
- Nếu quá thời gian không có dữ liệu thì không cần nhận dữ liệu nữa và không cần cảnh báo cho tình huống này.

### 6. Phân quyền và vai trò vận hành

- Phân quyền nhìn thấy dữ liệu được áp dụng theo:
  - `Người bệnh`
  - `Khoa`
  - `Cơ sở y tế`
- Đầu mối theo dõi Monitor gồm:
  - `Điều dưỡng`
  - `Trưởng khoa`
  - `CNTT`
  - `Giám đốc`
- `CNTT` phụ trách xử lý mapping.
- `Triển khai ISOFH` và `CNTT` là nhóm được thực hiện resend.

### 7. Dữ liệu và đối soát

- Không lưu dữ liệu thô tại HIS trong giai đoạn hiện tại.
- Chốt thời gian truy xuất dữ liệu theo thiết lập trước.
- Sau triển khai sẽ đánh giá lại, nếu cần mới mở rộng lưu toàn bộ dữ liệu thô.

### 8. Mục tiêu chất lượng đã chốt

- Tỷ lệ ghép đúng Người bệnh trên Monitor: `100%`.
- Tỷ lệ bản ghi sinh hiệu gửi thành công sang HIS: `100%`.
- Tỷ lệ bản ghi lỗi được phát hiện và xử lý resend trong SLA: `100%`.
- Dữ liệu Người bệnh phải được ghi nhận đầy đủ và chính xác `100%` do đây là dữ liệu quan trọng, sai lệch có thể gây sự cố nghiêm trọng.

### 9. Nội dung sẽ xác định khi kiểm thử hoặc UAT

- Khả năng Monitor gửi ổn định các thông tin:
  - `Mã hồ sơ`
  - `Thời điểm đo`
  - `Thông tin nhận diện thiết bị`
- Khả năng HIS trả đúng chu kỳ ghi nhận sinh hiệu tại thời điểm Tool gọi.
- Khả năng bản ghi gắn ổn định với `Mã máy` tương ứng.

## Nội dung còn mở

- Ticket / CR / Jira liên quan.
- Người tạo, Người review, Người phê duyệt.
- Có cần tách tiếp tài liệu detail level hay không.

## Liên kết gợi ý

- [[2026-05-01_NV_Tich hop thiet bi PVM-2701 vao HIS - Phase I]]
- [[2026-05-02_NV_Tich hop thiet bi PVM-2701 vao HIS - Phase II]]
- [[2026-05-02_SRS_High level_Tich hop thiet bi PVM-2701 vao HIS]]
