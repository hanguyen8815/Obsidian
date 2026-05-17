# Template SRS - Level 2: Mô tả màn hình chi tiết

## Tên tính năng / Màn hình

| Thuộc tính | Nội dung |
| --- | --- |
| Trạng thái | `YellowDraft` |
| Người tạo | `Hà Nguyễn` |
| Ngày tạo | `16/05/2026` |
| Người review | `Hà Nguyễn` |
| Ngày review | `16/05/2026` |
| Người phê duyệt | `Hà Nguyễn` |
| Phân hệ | `Tích hợp thiết bị / ISOFHTool` |
| Ticket / CR / Jira | `[Cần bổ sung]` |
| Cơ sở y tế áp dụng | `Pilot tại MEDI Plus Nam Định, định hướng dùng chung cho nhiều Cơ sở y tế` |
| Mức áp dụng | `Tích hợp + Cấu hình` |
| Tài liệu detail liên quan | `Mục 15. Liên Kết` |

## 1. Phiên Bản Tài Liệu

| Phiên bản | Ngày phát hành | Người cập nhật | Người yêu cầu / Ticket | Nội dung thay đổi | Ảnh hưởng |
| --- | --- | --- | --- | --- | --- |
| V1.0 | 16/05/2026 | Hà Nguyễn | [Cần bổ sung] | Hoàn thiện tài liệu nghiệp vụ detail cho `Danh mục thiết bị Monitor` theo phạm vi đã chốt | ISOFHTool / QA / Tài liệu |

## 2. Tóm Tắt

- Vấn đề / Hiện tại:
    - ISOFHTool cần một danh sách cấu hình để định danh đúng từng thiết bị Monitor gửi dữ liệu HL7.
    - Nếu không khai báo đúng `Mã máy`, `Tên máy`, `IP` và `PORT` thì dễ phát sinh nhầm nguồn dữ liệu hoặc khó đối soát khi vận hành.
- Mục tiêu:
    - Khai báo danh sách để định danh thiết bị Monitor.
    - Khai báo `IP` và `PORT` của Monitor tương ứng để có thể kết nối qua `TCP/IP`.
    - Chức năng được thao tác trực tiếp trên `ISOFHTool`.
- Kết quả mong muốn:
    - Tool nhận diện đúng từng thiết bị gửi dữ liệu.
    - Tool sử dụng đúng cấu hình đang hiệu lực khi xử lý dữ liệu sinh hiệu.
    - Người vận hành tra cứu và cập nhật lại cấu hình thiết bị khi có thay đổi.

## 3. Người Dùng / Vai Trò

- Người dùng chính: `Admin CNTT của Phòng khám`
- Vai trò liên quan:
    - `Triển khai ISOFH`
    - `DEV / QA`
    - `ISOFHTool`
- Phạm vi sử dụng:
    - Giai đoạn đầu dùng máy này làm server vận hành.
    - Chỉ `Admin CNTT của Phòng khám` tiếp cận và thao tác chức năng này trên máy server.
    - `Triển khai ISOFH` tham gia trong giai đoạn cấu hình ban đầu hoặc hỗ trợ đối soát khi cần.

## 4. Phạm Vi

### 4.1. Trong phạm vi

- Khai báo danh sách thiết bị Monitor trên ISOFHTool.
- Xem danh sách thiết bị đã khai báo.
- Thêm mới cấu hình thiết bị.
- Cập nhật cấu hình thiết bị.
- Chuyển thiết bị sang trạng thái `Ngừng dùng`.
- Tìm kiếm theo `Mã máy`, `Tên máy`, `IP`, `PORT`, `Ghi chú`, `Hiệu lực`.
- Dùng cấu hình thiết bị để định danh nguồn gửi dữ liệu và gắn `Mã máy` khi Tool xử lý dữ liệu sinh hiệu.
- Phục vụ pilot hiện tại gồm:
    - `01` thiết bị `NIHON KOHDEN SVM-7260`.
    - `04` thiết bị `NIHON KOHDEN PVM-4761`.

### 4.2. Ngoài phạm vi

- Đồng bộ tự động danh sách thiết bị từ hệ thống khác.
- Phân quyền chức năng chi tiết ngay trong ISOFHTool.
- Dashboard theo dõi trạng thái thiết bị nâng cao.
- Kiểm tra kết nối thật tới thiết bị ngay trên màn hình này.
- Đặc tả kỹ thuật chi tiết tầng socket server hoặc hạ tầng mạng.

## 5. Câu Hỏi / Điểm Cần Xác Nhận

| STT | Câu hỏi / Điểm chưa rõ | Người cần xác nhận | Trạng thái | Kết quả xác nhận |
| --- | --- | --- | --- | --- |
| 1 | Hiện chưa có câu hỏi mở chặn handover trong phạm vi màn hình này | - | Đã rà soát | Các rule chính của màn hình đã đủ để DEV và QA triển khai |

## 6. Luồng Xử Lý

| Bước | Người dùng thao tác | Điều kiện | Hệ thống xử lý | Kết quả (thành công) | Xử lý khi thất bại |
| --- | --- | --- | --- | --- | --- |
| 1 | Mở màn hình `Danh mục thiết bị Monitor` | Người dùng tiếp cận được máy server chạy ISOFHTool | Hiển thị danh sách thiết bị đã khai báo theo `01` bảng thông tin | Người dùng xem được dữ liệu hiện có | Nếu không truy cập được máy server hoặc hệ thống thì xử lý theo lỗi hạ tầng vận hành |
| 2 | Tìm theo `Mã máy`, `Tên máy`, `IP`, `PORT`, `Ghi chú`, `Hiệu lực` | Có dữ liệu đã khai báo | Lọc danh sách theo tiêu chí nhập | Trả ra danh sách phù hợp | Nếu không có dữ liệu phù hợp thì hiển thị danh sách rỗng |
| 3 | Thêm mới cấu hình thiết bị | Có đủ thông tin cần khai báo | Kiểm tra dữ liệu, kiểm tra trùng `IP + PORT`, kiểm tra trùng `Mã máy`, sau đó lưu | Tạo mới được cấu hình thiết bị; hệ thống tự sinh `Ngày tạo`, `Ngày sửa` | Không cho lưu và hiển thị message lỗi phù hợp |
| 4 | Cập nhật cấu hình thiết bị đã có | Tồn tại bản ghi cần sửa | Kiểm tra lại rule dữ liệu và rule trùng trước khi lưu | Lưu được thay đổi; hệ thống cập nhật lại `Ngày sửa`; Tool dùng cấu hình mới cho dữ liệu nhận sau thời điểm lưu | Không cho lưu và hiển thị message lỗi phù hợp |
| 5 | Chuyển cấu hình sang `Ngừng dùng` | Tồn tại bản ghi đang `Đang dùng` | Cập nhật `Hiệu lực = Ngừng dùng`, giữ lại lịch sử bản ghi và cập nhật `Ngày sửa` | Bản ghi không còn được dùng cho dữ liệu mới | Nếu cập nhật không thành công thì hiển thị lỗi hệ thống |

## 7. Quy Tắc Nghiệp Vụ

- Mỗi thiết bị Monitor đang dùng phải gắn đúng một `Mã máy` để Tool định danh thiết bị và xử lý dữ liệu.
- `Tên máy` dùng để nhận biết thiết bị trong quá trình khai báo, rà soát và vận hành.
- `IP` và `PORT` được khai báo theo từng thiết bị để ISOFHTool kết nối với Monitor qua `TCP/IP`.
- Một cấu hình thiết bị chỉ được Tool sử dụng khi ở trạng thái `Đang dùng`.
- Nếu thiết bị đổi `IP`, `PORT` hoặc tên nhận biết thì `Admin CNTT của Phòng khám` phải cập nhật lại cấu hình trước khi nhận dữ liệu thật.
- Không xóa vật lý bản ghi cấu hình đã từng dùng; chỉ chuyển `Hiệu lực` sang `Ngừng dùng` để bảo đảm truy vết.
- Dữ liệu sinh hiệu gửi sang `SAKURA` phải lấy `Mã máy` từ cấu hình thiết bị đang hiệu lực tại thời điểm Tool xử lý dữ liệu.

## 8. Quy Tắc Kiểm Tra Dữ Liệu

- Bắt buộc nhập `Mã máy`.
- Bắt buộc nhập `Tên máy`.
- Bắt buộc nhập `IP`.
- Bắt buộc nhập `PORT`.
- Không được trùng tổ hợp `IP + PORT` giữa các dòng đang `Đang dùng`.
- Không được trùng `Mã máy` giữa các dòng đang `Đang dùng`.
- `PORT` phải là số nguyên dương.

## 9. Điều Kiện Chấp Nhận

- [x] Người dùng xem được danh sách thiết bị Monitor đã khai báo.
- [x] Người dùng tìm kiếm được theo `Mã máy`, `Tên máy`, `IP`, `PORT`, `Ghi chú`, `Hiệu lực`.
- [x] Người dùng thêm mới được cấu hình thiết bị và hệ thống tự sinh `Ngày tạo`, `Ngày sửa`.
- [x] Hệ thống chặn lưu nếu trùng tổ hợp `IP + PORT` trong danh sách đang dùng.
- [x] Hệ thống chặn lưu nếu trùng `Mã máy` trong danh sách đang dùng.
- [x] Hệ thống chặn lưu nếu thiếu trường bắt buộc hoặc `PORT` không hợp lệ.
- [x] Người dùng sửa được cấu hình thiết bị và Tool dùng cấu hình mới cho dữ liệu nhận sau thời điểm lưu.
- [x] Người dùng chuyển được thiết bị sang `Ngừng dùng` mà không xóa mất lịch sử cấu hình.
- [x] Giai đoạn đầu chức năng được vận hành trên máy server và chỉ `Admin CNTT của Phòng khám` tiếp cận.

## 10. Trường Hợp Ngoại Lệ

- Trùng `IP + PORT` với thiết bị khác đang dùng.
- Trùng `Mã máy` với thiết bị khác đang dùng.
- Thiếu trường bắt buộc khi lưu.
- `PORT` không hợp lệ.
- Không tìm thấy dữ liệu phù hợp với điều kiện tìm kiếm.

## 11. Message Lỗi / Cảnh Báo Chính

| STT | Nhóm | Tình huống | Nội dung hiển thị | Loại | Thời điểm hiển thị | Ghi chú |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Validate dữ liệu | Thiếu trường cần khai báo | Chưa nhập đủ thông tin cấu hình thiết bị. | Lỗi chặn | Khi bấm lưu mà chưa nhập đủ thông tin | Áp dụng cho các trường bắt buộc trên màn hình |
| 2 | Validate nghiệp vụ | Trùng `IP + PORT` | Thiết bị với IP và PORT này đã tồn tại trong danh sách đang dùng. | Lỗi chặn | Khi lưu dữ liệu | Chặn tạo mới hoặc cập nhật |
| 3 | Validate nghiệp vụ | Trùng `Mã máy` | Mã máy này đã được khai báo cho một thiết bị đang dùng. | Lỗi chặn | Khi lưu dữ liệu | Chặn tạo mới hoặc cập nhật |
| 4 | Validate dữ liệu | `PORT` không hợp lệ | PORT phải là số nguyên dương. | Lỗi chặn | Khi lưu dữ liệu | |
| 5 | Hệ thống | Lỗi không xác định trong quá trình lưu | Có lỗi xảy ra trong quá trình xử lý. Vui lòng thử lại hoặc liên hệ quản trị hệ thống. | Lỗi hệ thống | Khi lưu thất bại do lỗi hệ thống | Chỉ dùng cho lỗi không xác định rõ nghiệp vụ |

## 12. Trường Thông Tin

### 12.1. Cấu Trúc Màn Hình / Khu Vực

| STT | Khu vực | Loại dữ liệu | Quan hệ dữ liệu | Nguồn dữ liệu | Mô tả |
| --- | --- | --- | --- | --- | --- |
| 1 | Bảng thông tin thiết bị Monitor | Dữ liệu chính | `1-1` với từng bản ghi cấu hình thiết bị | Người dùng khai báo trên ISOFHTool | Gồm toàn bộ thông tin định danh và kết nối của thiết bị |

### 12.2. Bảng Trường Thông Tin

| STT | Tên trường | Kiểu DL | Kiểu điều khiển | Bắt buộc | Chỉ đọc | Giá trị mặc định | Quy tắc / Ghi chú |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | `Mã máy` | String | Textbox | Có | Không | Trống | Dùng để định danh thiết bị và gắn vào dữ liệu sinh hiệu |
| 2 | `Tên máy` | String | Textbox | Có | Không | Trống | Tên hiển thị để nhận biết thiết bị |
| 3 | `IP` | String | Textbox | Có | Không | Trống | Địa chỉ `TCP/IP` của thiết bị |
| 4 | `PORT` | Integer | Textbox số | Có | Không | Trống | Cổng `TCP/IP` của thiết bị |
| 5 | `Ghi chú` | String | Textarea | Không | Không | Trống | Ghi chú vận hành nếu có |
| 6 | `Hiệu lực` | Enum | Dropdown | Có | Không | `Đang dùng` | `Đang dùng` hoặc `Ngừng dùng` |
| 7 | `Ngày tạo` | LocalDateTime | Datetime label | Không | Có | Hệ thống tự sinh | Thời điểm tạo bản ghi |
| 8 | `Ngày sửa` | LocalDateTime | Datetime label | Không | Có | Hệ thống tự cập nhật | Thời điểm sửa gần nhất |

## 13. Danh Sách / Tìm Kiếm

### 13.1. Danh Sách hiển thị

| STT | Cột hiển thị | Mô tả | Ghi chú |
| --- | --- | --- | --- |
| 1 | `Mã máy` | Mã dùng để định danh thiết bị | |
| 2 | `Tên máy` | Tên hiển thị để nhận biết thiết bị | |
| 3 | `IP` | Địa chỉ thiết bị | |
| 4 | `PORT` | Cổng kết nối | |
| 5 | `Ghi chú` | Ghi chú vận hành | |
| 6 | `Hiệu lực` | Trạng thái sử dụng cấu hình | |
| 7 | `Ngày tạo` | Thời điểm tạo bản ghi | Chỉ đọc |
| 8 | `Ngày sửa` | Thời điểm sửa gần nhất | Chỉ đọc |

### 13.2. Điều kiện tìm kiếm

| STT | Tên trường | Kiểu điều khiển | Tìm chính xác / gần đúng | Quy tắc / Ghi chú |
| --- | --- | --- | --- | --- |
| 1 | `Mã máy` | Textbox | Gần đúng | Tìm theo chuỗi nhập |
| 2 | `Tên máy` | Textbox | Gần đúng | Tìm theo chuỗi nhập |
| 3 | `IP` | Textbox | Gần đúng | Tìm theo chuỗi nhập |
| 4 | `PORT` | Textbox số | Chính xác | |
| 5 | `Ghi chú` | Textbox | Gần đúng | Tìm theo chuỗi nhập |
| 6 | `Hiệu lực` | Dropdown | Chính xác | |

## 14. Ghi Chú Vận Hành

- Chức năng được thao tác trực tiếp trên `ISOFHTool`.
- Tài liệu này tập trung ở mức nghiệp vụ màn hình và cấu hình vận hành trên `ISOFHTool`.
- Giai đoạn đầu dùng máy này làm server vận hành.
- Chỉ `Admin CNTT của Phòng khám` tiếp cận và thao tác chức năng này trên máy server.
- Cấu hình đã từng sử dụng không xóa vật lý để phục vụ truy vết khi cần.

## 15. Liên Kết

- Jira: `[Cần bổ sung]`
- Figma: `[Cần bổ sung]`
- Confluence liên quan:
    - HLR: `https://confluence.isofh.com.vn/pages/viewpage.action?pageId=226035184`
    - Trang detail hiện tại: `https://confluence.isofh.com.vn/pages/viewpage.action?pageId=226036881`
    - Template: `https://confluence.isofh.com.vn/display/II/SRS+Template+detail`
- Kiểm thử / Test case: `[Cần bổ sung]`
