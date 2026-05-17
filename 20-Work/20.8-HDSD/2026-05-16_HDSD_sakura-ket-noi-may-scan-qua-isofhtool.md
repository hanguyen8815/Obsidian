# Hướng dẫn sử dụng SAKURA kết nối máy Scan thông qua ISOFHTool

## 1. Mục đích

Tài liệu này hướng dẫn Người dùng:

- Nắm được logic hoạt động của chức năng Scan trên SAKURA.
- Kiểm tra cài đặt ISOFHTool.
- Sử dụng chức năng Scan trên SAKURA.

## 2. Phạm vi áp dụng

- Áp dụng cho tính năng `Scan biểu mẫu HSBA` trên SAKURA.
- Áp dụng cho máy trạm đã cài `ISOFHTool` và máy Scan đã cấu hình share file scan ra folder dùng chung.
- Phần cấu hình chi tiết folder share, quyền truy cập folder và cấu hình ISOFHTool phụ thuộc từng Cơ sở y tế.

## 3. Đối tượng sử dụng

- Người dùng thao tác scan biểu mẫu trên SAKURA.
- Đội triển khai hoặc IT hỗ trợ kiểm tra kết nối ISOFHTool khi cần.

## 4. Điều kiện trước khi thao tác

- Máy trạm đã cài `ISOFHTool`.
- Máy Scan đã được cấu hình để đẩy file scan vào một folder share.
- `ISOFHTool` đã được cấu hình đọc đúng folder share của máy Scan.
- Người dùng đã đăng nhập SAKURA và có quyền dùng tính năng `Scan biểu mẫu HSBA`.
- SAKURA nên được truy cập bằng `HTTPS`.

> Lưu ý quan trọng:
> Bắt buộc ưu tiên triển khai SAKURA bằng `HTTPS` để việc gọi `ISOFHTool` ổn định hơn. Nếu dùng `HTTP`, thường phải add trust link và cấp thêm quyền trên trình duyệt hoặc máy trạm, gây nhiều thao tác phụ và rủi ro không ổn định.

## 5. Cơ chế hoạt động chung

1. Máy Scan share file scan vào một folder dùng chung.
2. `ISOFHTool` được cấu hình đọc folder scan nói trên.
3. Khi Người dùng nhấn `Scan giấy tờ` trên SAKURA, SAKURA gọi `ISOFHTool` để theo dõi file mới xuất hiện trong folder share.
4. Nếu có file có `thời gian tạo` sau thời điểm Người dùng nhấn scan, `ISOFHTool` tự đẩy file đó lên SAKURA.
5. SAKURA hiển thị file scan để Người dùng kiểm tra.
6. Người dùng xác nhận đúng tài liệu và nhấn `Lưu` để hoàn tất.

## 6. Hướng dẫn thiết lập và kiểm tra kết nối ISOFHTool

### 6.1. Cài đặt ISOFHTool

1. Cài `ISOFHTool` trên máy trạm cần dùng máy Scan.
2. Cấu hình `ISOFHTool` đọc đúng folder share của máy Scan.
3. Đảm bảo máy trạm có quyền truy cập tới folder share này.

> [Cần xác nhận] Tài liệu này chưa có ảnh màn hình phần cài đặt và cấu hình folder trong ISOFHTool. Nếu ban hành cho người dùng cuối, nên tách phần này thành tài liệu riêng cho IT hoặc đội triển khai.

### 6.2. Kiểm tra trạng thái ISOFHTool trên SAKURA

**Hình minh họa 1**

`[Chèn ảnh 1 theo file chị gửi: Mở icon Trợ giúp > chọn iSofHTools > kiểm tra Trạng thái sử dụng và Chẩn đoán kết nối iSofHTools]`

Thực hiện như sau:

1. Mở SAKURA.
2. Nhấn icon `Trợ giúp` ở góc phải trên cùng.
3. Chọn mục `iSofHTools`.
4. Kiểm tra `Trạng thái sử dụng`.
5. Nếu trạng thái đang `Tắt`, chuyển sang `Bật`.
6. Nhấn `Chẩn đoán kết nối iSofHTools` để kiểm tra nhanh khả năng kết nối thực tế.

### 6.3. Kiểm tra kết quả chẩn đoán kết nối

**Hình minh họa 2**

`[Chèn ảnh 2 theo file chị gửi: Màn hình Chẩn đoán kết nối iSofHTools]`

Sau khi chạy chẩn đoán, cần kiểm tra các mục sau:

1. `Kiểm tra trạng thái runtime iSofHTools`: trạng thái xanh, nghĩa là runtime đang bật và sẵn sàng.
2. `Kiểm tra host iSofHTools`: trạng thái xanh, nghĩa là SAKURA resolve được host của iSofHTools.
3. `Kiểm tra môi trường truy cập`: trạng thái xanh, nghĩa là môi trường hiện tại cho phép truy cập local tool.
4. `Kiểm tra giao thức kết nối`: ưu tiên trạng thái xanh.
5. Nếu mục này màu vàng, cần kiểm tra lại link truy cập SAKURA và ưu tiên dùng `HTTPS`.
6. `Kiểm tra kết nối thực tế`: trạng thái xanh, nghĩa là SAKURA đã gọi thử API thành công.

Kết quả mong đợi:

- `Gọi thử API trực tiếp`: thành công.
- Các mục kiểm tra chính đều ở trạng thái xanh.

## 7. Hướng dẫn sử dụng tính năng scan biểu mẫu

### 7.1. Mở chức năng scan biểu mẫu

**Hình minh họa 3**

`[Chèn ảnh 3 theo file chị gửi: Tại màn hình Chỉ định dịch vụ, chọn Scan biểu mẫu HSBA]`

1. Mở màn hình `Tiếp đón - Chỉ định dịch vụ`.
2. Chọn chức năng `Scan biểu mẫu HSBA`.

### 7.2. Thêm mới biểu mẫu cần scan

**Hình minh họa 4**

`[Chèn ảnh 4 theo file chị gửi: Popup Danh sách biểu mẫu scan và form Thêm mới]`

1. Trên popup `Danh sách biểu mẫu scan`, nhấn icon `Thêm mới` dấu `+` màu xanh.
2. Tại form `Thêm mới`, chọn `Tên biểu mẫu` cần scan.
3. Kiểm tra lại `Thời gian thực hiện` nếu hệ thống có hiển thị sẵn.
4. Nếu cần, nhập thêm `Ghi chú`.

### 7.3. Thực hiện scan giấy tờ

Tiếp tục trên form `Thêm mới`:

1. Nhấn nút `Scan giấy tờ`.
2. SAKURA gọi sang `ISOFHTool` để theo dõi file mới xuất hiện trong folder share của máy Scan.
3. Đặt giấy tờ vào máy Scan và thực hiện scan.
4. Chờ hệ thống nhận file scan mới.

Lưu ý:

- File được nhận phải có `thời gian tạo` sau thời điểm nhấn `Scan giấy tờ`.
- Nếu scan xong nhưng không thấy file lên SAKURA, cần kiểm tra lại folder share và cấu hình đọc folder của `ISOFHTool`.

### 7.4. Kiểm tra file scan trên popup Ký và in

**Hình minh họa 5**

`[Chèn ảnh 5 theo file chị gửi: Popup Ký và in hiển thị file scan]`

1. Sau khi scan thành công, SAKURA hiển thị file tại popup `Ký và in`.
2. Kiểm tra đúng tài liệu vừa scan.
3. Kiểm tra đúng tên biểu mẫu ở danh sách phiếu bên phải.
4. Nếu tài liệu hiển thị đúng, quay lại bước lưu biểu mẫu.

### 7.5. Lưu biểu mẫu scan

1. Tại form thêm mới biểu mẫu, nhấn `Lưu` để hoàn tất.
2. Hệ thống lưu file scan vào biểu mẫu đã chọn.

**Hình minh họa 6**

`[Chèn ảnh 6 theo file chị gửi: Danh sách biểu mẫu scan sau khi lưu thành công]`

Kết quả mong đợi:

- Biểu mẫu scan xuất hiện trong danh sách.
- Người dùng có thể nhìn thấy các tiện ích như xem, sửa hoặc xóa tùy quyền được cấp.
- Trạng thái ban đầu có thể là `Chưa ký` nếu chưa thực hiện ký.

## 8. Lưu ý và lỗi thường gặp

### 8.1. Không mở được kết nối ISOFHTool

- Kiểm tra `Trạng thái sử dụng` trong menu `iSofHTools` đã bật chưa.
- Chạy lại `Chẩn đoán kết nối iSofHTools`.
- Nếu cần, dùng chức năng `Khởi động lại iSofHTools`.

### 8.2. Mục giao thức kết nối báo màu vàng

- Kiểm tra lại link truy cập SAKURA.
- Ưu tiên dùng `HTTPS`.
- Không khuyến khích vận hành lâu dài bằng `HTTP` vì cần cấp thêm trust hoặc quyền truy cập, dễ phát sinh lỗi không ổn định.

### 8.3. Scan xong nhưng không thấy file lên SAKURA

- Kiểm tra máy Scan đã thực sự đẩy file vào folder share chưa.
- Kiểm tra `ISOFHTool` có đang đọc đúng folder share không.
- Kiểm tra file scan có thời gian tạo sau thời điểm nhấn `Scan giấy tờ` không.
- Kiểm tra máy trạm còn quyền truy cập folder share không.

### 8.4. Người dùng thấy file nhưng chưa lưu được

- Kiểm tra đã chọn `Tên biểu mẫu` trước khi scan chưa.
- Kiểm tra quyền thao tác trên màn hình `Scan biểu mẫu HSBA`.
- Nếu còn lỗi, chụp lại màn hình và gửi đội triển khai hoặc IT để kiểm tra log.

## 9. Cần xác nhận trước khi ban hành

- Tên chính xác của đơn vị dùng tài liệu này và phạm vi áp dụng nội bộ hay gửi khách hàng.
- Có cần tách riêng phần `Cài đặt ISOFHTool` cho đội IT hay không.
- Có cần bổ sung phần phân quyền chi tiết theo vai trò người dùng hay không.
- Có cần nhúng trực tiếp file ảnh gốc vào tài liệu Word hoặc PDF trước khi phát hành hay không.
- Có cần bổ sung thêm tình huống scan nhiều trang hoặc scan lại khi chọn sai biểu mẫu hay không.
