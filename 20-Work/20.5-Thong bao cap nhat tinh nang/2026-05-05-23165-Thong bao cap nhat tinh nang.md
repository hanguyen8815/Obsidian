# Thông báo cập nhật tính năng Ver.23165 ngày 05/05/2026

- Phiên bản: 23165
- Đối tượng nhận: Người dùng hệ thống HIS/EMR SAKURA
- Ngày phát hành: 05/05/2026
- Kỳ cập nhật: Đợt phát hành ngày 05/05/2026
- Phạm vi: JIRA version 23165 - Đẩy code 05/05/2026_HIS_SAKURA

Đợt cập nhật version 23165 tập trung vào Báo cáo vận hành và báo cáo quản trị, Hồ sơ bệnh án điện tử, Khám ngoại trú, Quản lý chẩn đoán hình ảnh, thăm dò, phục hồi chức năng, Khác hoặc chưa phân hệ. Nội dung phát hành gồm các cải tiến thao tác, bổ sung biểu mẫu và hoàn thiện báo cáo nhằm hỗ trợ vận hành thực tế tại các Cơ sở y tế đang sử dụng SAKURA.

## Điểm nổi bật

- Bổ sung và hoàn thiện nhiều biểu mẫu, phiếu in và giấy tờ điện tử phục vụ khám, điều trị và tiêm chủng.
- Tối ưu các thao tác tại Tiếp đón, Khám ngoại trú, Kiosk và Quản lý nội trú để giảm bước xử lý cho người dùng.
- Cập nhật thêm tiêu chí lọc, dữ liệu hiển thị và logic thống kê cho nhiều báo cáo vận hành và quản trị.
- Bổ sung hoặc điều chỉnh một số cơ chế cấu hình theo đặc thù từng Cơ sở y tế để giảm thao tác thủ công.
- Đã loại khỏi thông báo các ticket nội bộ hoặc nhạy cảm không phù hợp chia sẻ rộng.

## Nội dung cập nhật theo Phân hệ

## 1. Quản lý danh mục dùng chung

### 1.1. [Danh mục địa chỉ hành chính] Fix lỗi tab Quốc gia, Tỉnh/TP không đổi size được (SAKURA-95759)

- Loại cập nhật: Sửa lỗi
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Đang không tăng size từ 10->20,50,100 được ở 2 tab Quốc gia và Tỉnh/TP
  + Sau khi cập nhật: Cho phép đổi size theo giá trị chọn
- Thông tin tham chiếu: Loại: Lỗi | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Danh mục | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 2. Tiếp đón và đăng ký khám

### 2.1. [Kiosk tiếp đón] Thêm mới API get thông tin đợt điều trị (SAKURA-94710, SAKURA-95243)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Dùng riêng api cho Kiosk2 tối ưu hơn -> không dùng api dùng chung
- Nội dung cập nhật:
  + Hiện tại: Để lấy thông tin NB, dịch vụ khám -> để đăng ký khám/ lấy số; Do hiện tại là api dùng chung bị giới hạn
  + Sau khi cập nhật: Đổi sang api đầu /kiosk2/nb-dot-dieu-tri -> không truyền params phân trang; các params lọc khác vẫn giữ nguyên như hiện tại
- Tác động tới người dùng: Nhân viên Tiếp đón sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: KIOSK Tiếp đón | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 2.2. [Kiosk tiếp đón] Thêm thiết lập bắt trùng thông tin lấy số (SAKURA-94701)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Viện tim mong muốn: mỗi thông tin nb hoặc mã Nb -> thì chỉ lấy được 1 số duy nhất trong cùng 1 ngày; Thêm thiết lập để không ảnh hưởng các viện cho phép NB lấy lại nhiều số
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: Thêm thiết lập chung: BAT_TRUNG_THONG_TIN_LAY_SO_TIEP_DON = True/false; TRUE:; khi gửi request
- Tác động tới người dùng: Nhân viên Tiếp đón sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: KIOSK Tiếp đón | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 2.3. [Kết nối TTTM- Viện E]Tiếp đón: Cho phép chỉ định nhiều DV khám với NB ngoại viện nếu CHI_DINH_NHIEU_CONG_KHAM_TAI_TIEP_DON = FALSE (SAKURA-95517)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Đang bật thiết lập = FALSE thì cấm cả NB ngoại viện
  + Sau khi cập nhật: Xử lý riêng với NB ngoại viện ( loai=10) thì vẫn cho phép kê nhiều dịch vụ khám nếu *CHI_DINH_NHIEU_CONG_KHAM_TAI_TIEP_DON = FALSE
- Tác động tới người dùng: Nhân viên Tiếp đón sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Ready for Stable | Ưu tiên: Cao | Phân hệ: Tiếp đón | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 2.4. [Mắt TW] [Tiếp đón] Xử lý không tự động tick ưu tiên theo lần khám trước của nb (SAKURA-95460)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bên bệnh viện E không check ưu tiên nb
- Nội dung cập nhật:
  + Hiện tại: Tự động tick ưu tiên nb theo lần khám trước
  + Sau khi cập nhật: Không lấy ưu tiên nb
- Tác động tới người dùng: Nhân viên Tiếp đón sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Tiếp đón | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 2.5. [PSHN][QMS] Sắp xếp lại giá trị các đối tượng ưu tiên (SAKURA-94201)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: PSHN muốn sắp xếp lại đúng vị trí giá trị các đối tượng ưu tiên
- Nội dung cập nhật:
  + Hiện tại: Sắp xếp chưa đúng vị trí so với mong muốn BV PSHN; Dòng "Người bệnh là đối tượng ưu tiên" không bôi đỏ viết hoa
  + Sau khi cập nhật: Sắp xếp lại vị trí giá trị các đối tượng ưu tiên: theo thứ tự:; Cấp cứu; Trẻ em dưới 6 tuổi
- Tác động tới người dùng: Nhân viên Tiếp đón sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: QMS | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 2.6. [Tiếp đón] Trả thêm trường Quân hàm, Chức vụ, Đơn vị vào API check trùng khi tiếp đón NB cũ (SAKURA-94166)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Trả thêm trường Quân hàm, Chức vụ, Đơn vị vào API check trùng khi tiếp đón NB cũ
- Tác động tới người dùng: Nhân viên Tiếp đón sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Tiếp đón | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 2.7. [Tiếp đón] Tự fill thông tin trường Quân hàm, Chức vụ, Đơn vị khi tiếp đón NB cũ (SAKURA-94167)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Không fill thông tin trường Quân hàm, Chức vụ, Đơn vị
  + Sau khi cập nhật: Tự động fill thông tin trường Quân hàm, Chức vụ, Đơn vị khi tiếp đón NB cũ
- Tác động tới người dùng: Nhân viên Tiếp đón sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Tiếp đón | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 3. Khám ngoại trú

### 3.1. [BVT0101] Điều chỉnh thiết lập CHI_DINH_THUOC_KE_THEO_THU_VA_BUOI (SAKURA-95502)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Người dùng muốn giảm thao tác khi kê đơn thuốc theo lịch/ QLYC-5797
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: cập nhật thiết lập CHI_DINH_THUOC_KE_THEO_THU_VA_BUOI cho phép kê liều dùng theo tuần cho viện tim; BỔ SUNG THÊM; Thêm cột SL/buổi trên Pop up Liều dùng theo tuần để nhập số lượng thuốc theo buổi sáng, trưa, chiều, tối.
- Tác động tới người dùng: Bác sĩ và Nhân viên hỗ trợ khám ngoại trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Chỉ định thuốc | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 3.2. [Form phiếu] Thêm key vào phiếu các phiếu bên dưới (SAKURA-94295)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung dữ liệu cho BV Từ Dũ; Phiếu tổng hợp khám bệnh (phieu_tong_hop_kb) - SAKURA - ISOFH Confluence
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Thêm mới các key sau vào các API:; Các key được thêm; dsVoOi_thoiGian
- Tác động tới người dùng: Bác sĩ và Nhân viên hỗ trợ khám ngoại trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 3.3. [Form phiếu][EMR_BA259.1] Cải thiện Logic liên kết dữ liệu từ màn hình khám bệnh sang phiếu KSK lái xe theo TT36 BYT (SAKURA-93638)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Đảm bảo giảm bớt thao tác cho người dùng, không phải nhập lại lần thứ 2 vì đã nhập trước đó tại khám bệnh.
- Nội dung cập nhật:
  + Hiện tại: Chưa có các key này và chưa liên kết dữ liệu từ Khám bệnh sang
  + Sau khi cập nhật: Trả thêm các key liên kết dữ liệu từ màn hình Khám bệnh:; Tai - Mũi - Họng:; Tai trái --> *taiTraiNoiThuong* --> String --> Mapping với dữ liệu từ trường taiTraiNoiThuong của MH Khám bệnh PK Tai Mũi Họng
- Tác động tới người dùng: Bác sĩ và Nhân viên hỗ trợ khám ngoại trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 3.4. [Khám bệnh] Cải thiện trường Ưu tiên nơi lấy mẫu bệnh phẩm (SAKURA-95579)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: đáp ứng yêu cầu dữ liệu từ BV Từ Dũ; 1 Danh mục nơi lấy mẫu bệnh phẩm - SAKURA - ISOFH Confluence
- Nội dung cập nhật:
  + Hiện tại: Đang không tự động fill nơi lấy mẫu
  + Sau khi cập nhật: Tự động mặc định Fill trường "Nơi lấy mẫu" khi kê dịch vụ dựa trên mức độ ưu tiên của Phòng; VD: Dịch vụ A có 2 phòng lấy mẫu --> Khi chỉ định tự động fill Nơi lấy mẫu là phòng có Ưu tiên nhỏ hơn
- Tác động tới người dùng: Bác sĩ và Nhân viên hỗ trợ khám ngoại trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Khám bệnh | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 3.5. [Khám bệnh] Thêm trường vào Chuyên khoa sản (SAKURA-93881)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Cải thiện form khám theo yêu cầu của trưởng khoa BV Từ Dũ; Chuyên khoa sản - SAKURA - ISOFH Confluence
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Thêm mới các trường sau vào form Khám Sản; Thời gian vỡ ối: Datetime; Không rõ thời gian vỡ ối: Checkbox
- Tác động tới người dùng: Bác sĩ và Nhân viên hỗ trợ khám ngoại trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm lựa chọn mới trên màn hình liên quan khi thao tác.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Khám bệnh | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 3.6. [Khám bệnh/Chỉ định] Trả thêm thông tin định mức chỉ định theo tuần vào dsPhongThucHien (SAKURA-94645)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Cần thông tin định mức chỉ định theo tuần trong danh mục phòng để FE check có cho phép bs chỉ định hay không
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: trả thêm thông tin định mức chỉ định theo tuần đã khai báo ở danh mục phòng vào dsPhongThucHien
- Tác động tới người dùng: Bác sĩ và Nhân viên hỗ trợ khám ngoại trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Test on Stable | Ưu tiên: Cao | Phân hệ: Chỉ định dịch vụ | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 3.7. [Khám bệnh] Bấm in Phân phòng không tải lại màn hình nên lưu sửa dịch vụ bị trả về phòng cũ (SAKURA-95702)

- Loại cập nhật: Sửa lỗi
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Sau khi bấm in phiếu chỉ định, dịch vụ đã được phân từ phòng chung sang phòng con nhưng màn hình không tự tải lại thông tin.; Nếu người dùng không F5 màn hình mà bấm sửa bản ghi rồi lưu, FE vẫn giữ phòng cha và đẩy dịch vụ về lại phòng cũ.
  + Sau khi cập nhật: Sau khi phân phòng xong, màn hình tự F5 hoặc tự tải lại thông tin dịch vụ để luôn hiển thị phòng mới nhất.; Link Người bệnh:; Chưa xác định
- Tác động tới người dùng: Bác sĩ và Nhân viên hỗ trợ khám ngoại trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Lỗi | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Khám bệnh | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 3.8. [Khám bệnh] Bổ sung Thiết lập chung bắt điều kiện tăng biến đếm số lượng tại Header trạng thái khám của người bệnh (SAKURA-91885)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Đáp ứng yêu cầu của viện theo quy trình vận hành của viện và lỗi bug test ra từ QC.
- Nội dung cập nhật:
  + Hiện tại: Khi kê cho NB tại tiếp đón theo idPhong rồi thì API biến đếm doiKham + 1 và chỉ hiển thị số lượng tổng dù người bệnh chưa đo thị lực.; Ví dụ: Đợi khám 3, chỉ hiện tên 1 người bệnh (đã đo thị lực) + 2 người bệnh chưa hiện tên (chưa đo)
  + Sau khi cập nhật: Bổ sung Thiết lập chung *TANG_SL_SAU_KHI_DO_THI_LUC:*-; Nếu Giá trị = True:*-; Chỉ khi Người bệnh đã hoàn thành đo thị lực (Xác nhận thông qua việc gọi thành công API: POST
- Tác động tới người dùng: Bác sĩ và Nhân viên hỗ trợ khám ngoại trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Khám bệnh | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 3.9. [Khám bệnh] Bổ sung Thiết lập chung không tự động gọi API chuyển trạng thái Đang khám tại thanh Header trạng thái khám người bệnh (SAKURA-94847)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Hiện tại viện Mắt Việt Nga đang dùng luồng Đo thị lực trước rồi mới hiện tên lên Đợi khám và đang vướng lỗi khi kê dịch vụ từ tiếp đón xong chưa đo vẫn lên nên log Fix.
- Nội dung cập nhật:
  + Hiện tại: Đang tự động call API chuyển trạng thái sang Đang khám (60) khi nhấn button Lưu và chuyển khám nên chưa check được yêu cầu ô Đợi khám
  + Sau khi cập nhật: Bổ sung Thiết lập chung *KHONG_CHUYEN_TRANG_THAI_DV_SAU_DO_THI_LUC**:; Nếu Giá trị = True:; Ghép key *doiKham2* mà BE đã trả lên ô Đợi khám trong thanh Header trạng thái khám người bệnh
- Tác động tới người dùng: Bác sĩ và Nhân viên hỗ trợ khám ngoại trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Khám bệnh | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 3.10. [QLYC-5937] Đổi tên trường thông tin ở màn hình Khám bệnh (SAKURA-95554)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Viện muốn đổi tên thành Dịch kính để đúng với thuật ngữ của bên viện; Conf:
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: Đổi tên trường Dịch kính vẫn ở *Tùy chỉnh giao diện*, mh Khám bệnh/Đo thị lực; | Hiện tại | Mong muốn |; | Dịch kính vẫn | Dịch kính |
- Tác động tới người dùng: Bác sĩ và Nhân viên hỗ trợ khám ngoại trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Khám bệnh | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 3.11. [TUDU0464] Bổ sung thiết lập chung cho phép ẩn cột Giá trong popup chỉ định DVKT ở tất cả các màn hình (SAKURA-95458)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Tránh gây hiểu lầm ở những khu khám dịch vụ (vì giá dịch vụ khác đơn giá BH)
- Nội dung cập nhật:
  + Hiện tại: đang hiển thị cột giá cho tất cả dịch vụ kỹ thuật trong popup chỉ định dịch vụ kỹ thuật
  + Sau khi cập nhật: bổ sung thiết lập chung *AN_COT_GIA_TIEN_CHI_DINH_DICH_VU_KB; =true: ẩn cột giá tiền tại popup chỉ định dịch vụ kỹ thuật tại chỉ hiển thị cột đơn giá khi chọn dịch vụ kỹ thuật; = false/không hiệu lực: hiển thị cột giá như bình thường
- Tác động tới người dùng: Bác sĩ và Nhân viên hỗ trợ khám ngoại trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Khám bệnh | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 3.12. [TUDU0464] Lỗi ẩn cột đơn giá khi có thiết lập AN_COT_GIA_TIEN_CHI_DINH_DICH_VU_KB (SAKURA-95685)

- Loại cập nhật: Sửa lỗi
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Đang ẩn cột đơn giá bên phải tại popup chỉ định DVKT.
  + Sau khi cập nhật: Chỉ ẩn bên phần các DVKT bên trái không ẩn cột đơn giá bên phải.
- Tác động tới người dùng: Bác sĩ và Nhân viên hỗ trợ khám ngoại trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Lỗi | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Khám bệnh | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 4. Chỉ định dịch vụ

### 4.1. [Chỉ định dịch vụ] Cải thiện chỉ định gói dịch vụ có dịch vụ đã tắt hiệu lực (SAKURA-93748)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: QLYC-5648
- Nội dung cập nhật:
  + Hiện tại: Dịch vụ tắt hiệu lực không chỉ định riêng lẻ được, nhưng chỉ định trong gói vẫn được
  + Sau khi cập nhật: Không get dịch vụ đã tắt hiệu lực trong Danh mục dịch vụ khi chỉ định gói dịch vụ
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Ready for Stable | Ưu tiên: Bình thường | Phân hệ: Chỉ định dịch vụ | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 4.2. [Từ dũ] [Tiếp đón giao diện 2] Lỗi bị duplicate khi chỉ định các dịch vụ khám (SAKURA-94326)

- Loại cập nhật: Sửa lỗi
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Nếu chỉ định nhiều dịch vụ có *cùng loaiDichVu*, trong số đó có dịch vụ không thỏa điều kiện để chỉ định thành công thì đang cho phép kê dv thỏa nhiều lần => gây chỉ định trùng lặp nhiều dv khám; Check thấy dù dịch vụ đã kê thành công những FE vẫn gọi API chỉ định dichVuId đó chứ không xử lý theo logic:"*ít nhất 1 dịch...
  + Sau khi cập nhật: Rà soát và fix lỗi này; Link tiếp đón mới: Tiếp đón; link chi tiết dv: Quản lý tiếp đón
- Thông tin tham chiếu: Loại: Lỗi | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Chỉ định dịch vụ, Tiếp đón | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 5. Kê đơn ngoại trú

### 5.1. [Kê thuốc] Lỗi nhấn F2 không quay lại vị trí tìm kiếm tên thuốc (SAKURA-95585)

- Loại cập nhật: Sửa lỗi
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: trong popup chỉ định thuốc khi tìm kiếm tên thuốc chọn 1 thuốc; nhấn tiếp F2 , con trỏ chuột đang không quay lại vị trí ô nhập tên thuốc
  + Sau khi cập nhật: fix lại lỗi trên
- Thông tin tham chiếu: Loại: Lỗi | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Chỉ định thuốc | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 6. Quản lý nội trú

### 6.1. [BV PSTW] Số lượng thực dùng hiển thị sai định dạng thập phân (SAKURA-95660)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Tại màn hình chi tiết người bệnh nội trú, trường “Số lượng thực dùng” của thuốc đang hiển thị giá trị không đúng định dạng số thập phân.; Cụ thể: khi nhập giá trị 1.45, sau khi lưu hệ thống hiển thị thành 1.4500000000000002.
  + Sau khi cập nhật: Chuẩn hóa hiển thị tối đa 2 chữ số thập phân, Lưu thành công dữ liệu khi chỉnh sửa.
- Tác động tới người dùng: Bác sĩ và Điều dưỡng nội trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Nội trú | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 6.2. [BVP] Nb hẹn điều trị, lấy chẩn đoán đoán mô tả chi tiết từ thông tin ra viện của lượt điều trị gần nhất vào tờ điều trị của lượt điều trị hiện tại (SAKURA-95513)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: lấy Chẩn đoán (mô tả chi tiết) ở group thông tin ra viện là: *TEST CHẨN ĐOÁN MÔ TẢ CHI TIẾT* vào lần điều trị hiện tại; THực tế:* Đang lấy theo Chẩn đoán (mô tả chi tiết): *F40.2 - Ám ảnh sợ đặc hiệu (riêng lẻ)* ở group thông tin Thông tin điều trị
- Tác động tới người dùng: Bác sĩ và Điều dưỡng nội trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Nội trú | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 6.3. [PSHN] Giường tự chọn: Lỗi cập nhật thời gian nằm đến của line trước khi cho ra viện (SAKURA-93911)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: khi kết thúc điều trị cho ra viện ở B7: Lúc này thời gian đến của line giường tự chọn 2 lại đang cập nhật = thời gian ra viện => *SAI (đúng vẫn phải giữ nguyên thời gian: 15/04/2026 15:00:00 )
  + Sau khi cập nhật: xử lý lại logic trên khi cho ra viện , chỉ cập nhật lại thời gian nằm đến của line giường cuối cùng ( thời gian nằm đến các line khác vẫn giữ nguyên không cập nhật)
- Tác động tới người dùng: Bác sĩ và Điều dưỡng nội trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Phòng giường nội trú | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 6.4. [TUDU0434] Thêm button Xuất danh sách vào màn hình danh sách tờ điều trị khám chuyên khoa (SAKURA-93559)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: Thêm button Xuất DS vào màn hình danh sách tờ điều trị khám chuyên khoa theo điều kiện lọc trên màn hình đang chọn
- Tác động tới người dùng: Bác sĩ và Điều dưỡng nội trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm nút thao tác mới trên màn hình liên quan sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Quản lý nội trú | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 6.5. [TUDU0434] Thêm tính năng Xuất danh sách màn hình danh sách tờ điều trị khám chuyên khoa (SAKURA-93628)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Thêm tính năng Xuất danh sách màn hình danh sách tờ điều trị khám chuyên khoa. Xuất ra danh sách các cột trên màn hình theo giá trị lọc đang truyền vào
- Tác động tới người dùng: Bác sĩ và Điều dưỡng nội trú sẽ thấy thay đổi này khi thao tác trên màn hình liên quan.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Quản lý nội trú | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 7. Quản lý điều trị ngoại trú

### 7.1. [Báo cáo] Thêm các tiêu chí lọc vào báo cáo TC05 BÁO CÁO CHI TIẾT THU TIỀN THEO BÁC SĨ (SAKURA-94277)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: Thêm các tiêu chí lọc vào báo cáo TC05; Lấy dữ liệu NB đã thanh toán, NB ngoại trú và nội trú, không bao gồm dịch vụ hoàn; Tiêu chí lọc báo cáo:
- Tác động tới người dùng: Bác sĩ sẽ thấy thay đổi này khi thao tác trên phân hệ liên quan.
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: baocao | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 7.2. [MVNHCM] [Báo cáo] Thêm logic lấy key Ký quỹ ngoại trú/nội trú ở báo cáo TC42 (SAKURA-95446)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Lấy lên đúng số tiền thực thu trong ngày của viện; Conf*: TC42. Báo cáo nộp tiền (nghiệp vụ mới) - SAKURA - ISOFH Confluence
- Nội dung cập nhật:
  + Hiện tại: cột trên đang lấy cả line rút cọc (nguonTamUng = 0 & datCocId != null) nên BC tính sai tổng số tiền
  + Sau khi cập nhật: | Tên cột | Key | Mô tả |; | Ký quỹ Ngoại trú | [[tienKyQuy]] | *Không* lấy line rút cọc (nguonTamUng = 0 & datCocId = null); Chỉ lấy line đặt cọc (nguonTamUng = 100) và line tạm ứng thường (nguonTamUng = 0 & datCocId = null) |
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 7.3. [Ngoại trú] Thêm button Nguồn tài trợ cho bác sĩ ngoại trú (SAKURA-94854)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Phù hợp workflow thực tế tại Việt Đức (BS tích DV, thu ngân thanh toán); Tối ưu hiệu năng và thao tác (phân trang, sort, filter)
- Nội dung cập nhật:
  + Hiện tại: Đã có tính năng nguồn tài trợ đề tài theo dịch vụ tại màn thu ngân; Việt đức: Thu ngân không tích dịch vụ -> YC BS tích
  + Sau khi cập nhật: [Ngoại trú] Thêm button Nguồn tài trợ cho bác sĩ ngoại trú
- Tác động tới người dùng: Bác sĩ sẽ thấy thay đổi này khi thao tác trên phân hệ liên quan.
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm nút thao tác mới trên màn hình liên quan sau khi cập nhật.
- Phân quyền:
  + 2100368 - Hiển thị button Nguồn tài trợ
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Điều trị ngoại trú | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 8. Quản lý khám sức khỏe hợp đồng

### 8.1. [TC82.1] Thêm bộ lọc KSK hợp đồng cho TC82.1 (SAKURA-94232, SAKURA-94375)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Báo cáo đang không lọc được danh sách NB theo các thông tin của hợp đồng ksk
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Bổ sung lọc thêm theo; | Tiêu chí lọc | Đặc điểm | Mô tả | Ghi chú |; | Hợp đồng KSK | droplist | Hiển thị theo format: Mã TTHĐ - Tên hợp đồng
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: baocao | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 8.2. [Tờ điều trị/Chỉ định thuốc] Thêm trường Ngày SD thuốc cho phép theo dõi STT ngày sử dụng thuốc với đơn nhà thuốc và đơn kê ngoài (SAKURA-89744, SAKURA-89795)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: ĐK Đống Đa yêu cầu theo dõi số ngày sử dụng thuốc của đơn kê ngoài, đơn nhà thuốc giống như thuốc kho; POST
- Nội dung cập nhật:
  + Hiện tại: chưa theo dõi được STT ngày sử dụng thuốc với đơn nhà thuốc và đơn kê ngoài
  + Sau khi cập nhật: Theo dõi được số ngày SD thuốc cho tất cả các dịch vụ có tick *Theo dõi ngày SD = Yes; Thêm trường Ngày SD thuốc cho phép theo dõi STT ngày sử dụng thuốc với đơn nhà thuốc và đơn kê ngoài *loaiThuoc = 50, 55, 60; | Ngày sử dụng thuốc | | Number | | | | - STT ngày sử dụng thuốc, chỉ hiển thị với các thuốc theo dõi (Trên...
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Tờ điều trị | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 9. Quản lý chẩn đoán hình ảnh, thăm dò, phục hồi chức năng

### 9.1. [BVCR] Lọc các trường trong danh mục dịch vụ kĩ thuật không hoạt động đúng (SAKURA-95535)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: [BVCR] Lọc các trường trong danh mục dịch vụ kĩ thuật không hoạt động đúng
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Chốt sổ doanh thu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 9.2. [BVE] Danh sách PTTT: cho hiển thị thông tin người PTV/TTV chính, Phụ 1/Phụ 2/phụ 3 (SAKURA-93458)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: GET; chưa trả ra giá trị các vị trí đã chấm công. mới chỉ ra người thực hiện
  + Sau khi cập nhật: ptv1Id; tenPtv1; ptv2Id
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Quản lý PTTT | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 9.3. [BVP] Lọc "Không sinh báo cáo" không hoạt động đúng (SAKURA-95531)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: [BVP] Lọc "Không sinh báo cáo" không hoạt động đúng
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Chốt sổ doanh thu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 9.4. [Chỉ định dịch vụ/Thu tạm ứng] Thêm thiết lập DS_MA_BAO_CAO_IN_PHIEU_CHI_DINH (SAKURA-95525)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Viện yêu cầu không in ra các phiếu này; Link thiết lập chung:* Thiết lập chung.xlsx
- Nội dung cập nhật:
  + Hiện tại: Với thao tác được mô tả thì các phiếu chỉ định cls liên quan đang tự động in ra khi bấm nút Thu tạm ứng
  + Sau khi cập nhật: Thêm thiết lập; DS_MA_BAO_CAO_IN_PHIEU_CHI_DINH; , nếu:
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Thiết lập | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 9.5. [CĐHA] - Bổ sung nút in trên màn chi tiết dịch vụ (SAKURA-93923)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có nút In kết quả nên người dùng phải vào màn danh sách để lọc và in kết quả.; Mục đích/ Lý do cải thiện:* Giúp người thực hiện (Bác sĩ/Kỹ thuật viên) có thể in ngay kết quả chẩn đoán sau khi đã nhập liệu mà không cần phải truy cập vào màn kết quả CDHA.
  + Sau khi cập nhật: Bổ sung thêm nút *In kết quả.* Sử dụng lại API xem kết quả tại màn Thực hiện CDHA/TDCN.; Điều kiện hiển thị nút:* Khi field trangThai = 155 (nghĩa là Results available). Sử dụng lại API dưới đây cho nút *In kết quả.
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm nút thao tác mới trên màn hình liên quan sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: CDHA, Thăm dò chức năng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 9.6. [EDITOR] Component Table: Sửa lỗi ô check box Ẩn viền (SAKURA-95611)

- Loại cập nhật: Sửa lỗi
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Bảng đã tick Ẩn hiền. View phiếu thì thấy đã ẩn viền nhưng khi In phiếu lại vẫn hiển thị viền
  + Sau khi cập nhật: Component Table: Nếu hideBorder = true thì ẩn viền khi view phiếu + in phiếu
- Thông tin tham chiếu: Loại: Lỗi | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Editor | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 9.7. [Form phiếu][cdha_ket_qua_chung] Cải thiện logic trả key chieuCao cho Biểu mẫu (SAKURA-95638)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Viện mong muốn lấy thêm được chiều cao của người bệnh lên Form phiếu trả kết quả cho người bệnh để đầy đủ thông tin hành chính của người bệnh.
- Nội dung cập nhật:
  + Hiện tại: Đã kiểm tra Postman và API đang trả về 2 key chieuCao tại 2 object dsCdVaoVien và dsCdChinh nhưng đều false và không có dữ liệu số để lấy data lên. (Đính kèm)
  + Sau khi cập nhật: Cải thiện Logic trả key *chieuCao* cho Form phiếu *Phiếu kết quả CĐHA chung* --> Trả thêm key *chieuCao* ngang hàng với *canNang* để có thể lấy lên được báo cáo; Tên báo cáo : Phiếu kết quả CĐHA chung
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 9.8. [Tiếp nhận NB KSK] Thêm điều kiện lấy danh sách tiếp đón NB KSK (SAKURA-93481, SAKURA-94604)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Hiển thị được đầy đủ danh sách NB KSK để có thể tiếp nhận nhiều cùng 1 lúc
- Nội dung cập nhật:
  + Hiện tại: Popup Tiếp nhận nhiều NB KSK đang bắt điều kiện:; NB là KSK hợp đồng
  + Sau khi cập nhật: Bổ sung thêm điều kiện lấy danh sách NB trong API; Lọc theo params thanhToanSau = TRUE
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: CDHA, Thăm dò chức năng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 9.9. Cho phép sửa thông tin người thực hiện khi dv hoàn thành khi dc gán quyền 2300113 (SAKURA-95550)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: khi tk bác sĩ có được gán quyền : *2300113; nb đã thanh toán ra viện: FE đã mở cho nhập thông tin người thực hiện nhưng BE đang chặn không cho lưu
  + Sau khi cập nhật: bổ sung check thêm quyền: *2300113; nếu tài khoản không có quyền: 2300113 thì chặn như hiện tại ( nếu nb đã thanh toán ra viện không cho cập nhật); nếu tài khoản có quyền: 2300113 thì luôn luôn cho cập nhật thông tin người thực hiện ( không quan tâm đến trạng thái của nb)
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: CDHA, Thăm dò chức năng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 10. Quản lý phẫu thuật - thủ thuật

### 10.1. [Nội tiết] EMR_BA153: xử lý trả về thông tin phiếu của hồ sơ mới nhất (SAKURA-94385)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: nb có 3 mã hồ sơ:; HS1: phiếu in đã ký.
  + Sau khi cập nhật: sửa lại logic trên; luôn check theo thông tin của hồ sơ mới nhất.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 10.2. [PSHN] Đổi popup thông báo khi TK bị chặn đổi khoa chỉ định dịch vụ ở màn hình danh sách phẫu thuật thủ thuật (SAKURA-95588)

- Loại cập nhật: Sửa lỗi
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Khi tài khoản không có quyền đổi khoa chỉ định dịch vụ BE đã trả về "message": "Dịch vụ đã được thanh toán, quyền hạn của bạn không đủ để đổi khoa chỉ định dịch vụ" nhưng FE popup hiển thị đổi khoa thành công.
  + Sau khi cập nhật: Đổi popup thông báo từ "Đổi khoa chỉ định dịch vụ thành công" -> "Dịch vụ đã được thanh toán, quyền hạn của bạn không đủ để đổi khoa chỉ định dịch vụ" khi TK bị chặn đổi khoa chỉ định dịch vụ ở màn hình danh sách phẫu thuật thủ thuật
- Thông tin tham chiếu: Loại: Lỗi | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Quản lý PTTT | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 10.3. [PSHN]EMR_BA332 và EMR_BA332.1/ Phiếu bàn giao người bệnh: Thêm trường để lưu thông tin Bác sĩ phẫu thuật, thời gian dự kiến, Phương pháp phẫu thuật (SAKURA-95564)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: Thêm trường lưu các thông tin; Bác sĩ phẫu thuật: chọn từ Danh mục nhân viên; Thời gian dự kiến phẫu thuật: datetime tự nhập
- Tác động tới người dùng: Bác sĩ sẽ thấy thay đổi này khi thao tác trên phân hệ liên quan.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Ready for Stable | Ưu tiên: Cao | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 10.4. [PTTT] Thêm cơ chế hoàn thành đồng thời các dịch vụ phẫu thuật con khi bấm "Hoàn thành tất cả PT" (SAKURA-92911)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Theo cơ chế lấy danh sách dịch vụ từ chiDinhDichVuId từ dịch vụ cha đã chỉ định các dịch vụ phẫu thuật
  + Sau khi cập nhật: Thêm cơ chế hoàn thành đồng thời các dịch vụ phẫu thuật con khi bấm "Hoàn thành tất cả PT"; chiDinhTuDichVuId: 1248876; chiDinhTuLoaiDichVu: 10
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Quản lý PTTT | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 11. Nhà thuốc

### 11.1. [Nhà thuốc - Chi tiết đơn thuốc] Cải thiện tính năng sao chép đơn nhà thuốc (SAKURA-93372)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chức năng sao chép đơn nhà thuốc đang sao chép đơn thuốc gốc chứ không phải đơn thuốc sau chỉnh sửa; Đơn nhà thuốc sau khi tư vấn: Nhà thuốc
  + Sau khi cập nhật: Sau khi nhấn sao chép đơn, api POST sẽ lấy lên đủ thuốc từ đơn sau tư vấn; Link nghiệp vụ: 3.2.7.8.2. Chi tiết đơn thuốc cho NB có đơn trong viện - SAKURA - ISOFH Confluence (Mục 7.13. Sao chép thuốc)
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Nhà thuốc | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 11.2. [Thiết lập chung/Chi tiết đơn thuốc] Thêm mới thiết lập không cho phép Tư vấn đơn để sửa SL bán > SL đã kê (SAKURA-95484)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: theo yc kiểm soát bán thuốc của Mắt TW, mong muốn chặn ko cho bán nhiều hơn SL đã chỉ định; GET
- Nội dung cập nhật:
  + Hiện tại: ở chi tiết đơn thuốc Nhà thuốc, khi nhấn Tư vấn đơn, vẫn cho sửa tăng SL bán > SL kê + thêm sửa xóa thuốc khác
  + Sau khi cập nhật: Thêm mới thiết lập : KHONG_CHO_SUA_SL_BAN_NHIEU_HON_SL_KE, với đơn nội viện (đơn thuốc bác sĩ chỉ định, ko check đơn khách vãng lai); thiết lập = TRUE: khi nhấn tư vấn đơn Nhà thuốc: ko cho sửa SL bán > SL bác sĩ đã kê + ko cho phép nhập thêm thuốc mới vào đơn đã kê ban đầu; thiết lập = FALSE / ko khai báo: như cũ, vẫn...
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Nhà thuốc | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 11.3. Tờ điều trị: Ngày thực hiện đang không cập nhật chính xác (kho nhà thuốc và thuốc kê ngoài) (SAKURA-95524)

- Loại cập nhật: Sửa lỗi
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Thời gian thực hiện đang hiển thị chưa chính xác
  + Sau khi cập nhật: ở ngoài tờ điều trị, thông tin ngày thực hiện phải khớp và chính xác như khi mở rộng tờ điều trị
- Thông tin tham chiếu: Loại: Lỗi | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Nội trú | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 12. Quản lý kho

### 12.1. [BC K05, KNT07] Sửa lại bộ lọc Tên hàng hóa (SAKURA-95526)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: [BC K05, KNT07] Sửa lại bộ lọc Tên hàng hóa
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Kho | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 12.2. [BVE-Chi tiết thầu] Màn hình chi tiết thầu thêm trường Tên hàng hóa theo NCC (SAKURA-90811, SAKURA-90812)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: mã hàng hóa có khi nhập thầu có thể nhập từ nhiều NCC, mỗi NCC sẽ quy định 1 tên hàng hóa khác nhau ( không phải tên hàng hóa trúng thấu) nên cần thêm trường để phân biệt và lên phiếu nhập kho.
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Màn chi tiết thầu thêm trường Tên hàng hóa theo NCC; ND điền freetext; Mục đích: Cho phép ND tự điền tên hàng hóa theo NCC
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Kho | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 12.3. [TUDU0474] Thêm nút Hủy gửi duyệt tại màn hình chi tiết phiếu lĩnh (PTTT) (SAKURA-94618)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Thêm nút Hủy gửi duyệt tại màn hình chi tiết phiếu lĩnh (PTTT) cho user có thể thu hồi phiếu sau khi đã gửi duyệt; /ảnh chụp MH/
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Thêm nút "Hủy gửi duyệt" tại màn hình chi tiết phiếu lĩnh (PTTT) khi dl ở trạng thái chờ duyệt; => Logic xử lý giống nút "Hủy gửi duyệt" tại màn hình chi tiết phiếu lĩnh (Quản lý nội trú)
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm nút thao tác mới trên màn hình liên quan sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Kho | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 12.4. API kê thuốc kê ngoài trả thêm dữ liệu đơn vị tính sơ cấp, hệ số định mức (SAKURA-90266)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có dữ liệu đơn vị sơ cấp, Hệ số định mức
  + Sau khi cập nhật: Trả thêm dữ liệu đơn vị sơ cấp, Hệ số định mức; Mục đích:* View dữ liệu để FE nhập thông tin khi kê thuốc phục vụ tính năng pha chế thuốc kê ngoài
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Kho | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 13. Ngân hàng máu hoặc tủ máu

### 13.1. [BV19-8] Trả thêm key vào phiếu xuất kho máu (SAKURA-95705)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Trả thêm key *noiNhan; Khối hồng cầu trả riêng theo từng nhóm máu
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 13.2. [Hiến máu] Xử lý trường ngày sản xuất khi chiết xuất chế phẩm máu (SAKURA-95716)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Tạo phiếu hiến máu -> chiết xuất chế phẩm: Trường Ngày sản xuất đang lấy theo ngày tạo bản ghi hiến máu
  + Sau khi cập nhật: Xử lý trường ngày sản xuất khi chiết xuất chế phẩm máu; Ngày sản xuất = Thời gian lấy máu; Mục đích: Do Kho máu thực hiện hiến máu và chiết xuất ngày A nhưng không tạo phiếu ngay, có thể vài hôm sau (ngày B hoặc C) mới tạo phiếu
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Kho máu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 13.3. [Quản lý hiến máu] Thêm quyền chỉnh sửa thông tin Hiến máu có trạng thái Đã duyệt (SAKURA-95519)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Đang có quyền 3100601 cho chỉnh sửa phiếu ở trạng thái Tạo mới
  + Sau khi cập nhật: Thêm quyền chỉnh sửa thông tin Hiến máu có trạng thái Đã duyệt; | 3100603 | Cho phép chỉnh sửa thông tin hiến máu có trạng thái Đã duyệt |
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Kho máu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 14. Theo dõi sử dụng thuốc

### 14.1. [DS yêu cầu hoàn đổi] Cho phép hoàn đổi khi số lượng trả thực tế < số lượng yêu cầu trả ( kể cả số lượng thực tế trả = 0) (SAKURA-91774)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Viện có một số trường hợp cần yêu cầu trả thuốc nhưng người bệnh không có thuốc trả, thì số lượng thực tế = 0 nên vẫn mong muốn trả và bệnh nhân sẽ bồi thường tiền cho bệnh viện; Body: "dsDichVu":["nbDichVuCuId":1237611,"soLuong":0,"soLuongYeuCau":1],"loai":50
- Nội dung cập nhật:
  + Hiện tại: Hiện tại TDT ra viện đã ký và khi *Tạo yêu cầu trả thuốc tái khám sớm* nếu nhập SL yêu cầu trả = SL thực tế trả thì phần mềm không chặn, vẫn trả thuốc thành công; Còn nếu nhập số lượng trả < SL yêu cầu trả thì pm chặn và cảnh báo "Tờ điều trị đã được ký. Cần hủy ký để thực hiện thay đổi"
  + Sau khi cập nhật: Nếu nhập SL thực tế trả > SL yêu cầu trả (kể cả SL yêu cầu trả = 0) thì vẫn cho phép trả thuốc thành công, không chặn và thông báo "Tờ điều trị đã được ký. Cần hủy ký để thực hiện thay đổi"
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Hoàn đổi trả | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 15. Quyết toán BHYT

### 15.1. [Ký vân tay] Chỉnh sửa thiết lập bắt ký vân tay theo phòng khám (SAKURA-92880)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Cho phép cảnh báo/ chặn theo loại đối tượng áp dụng cho các phòng khám được khai báo; Cải thiện thao tác lấy vân tay nhanh hơn
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Cải thiện thiết lập: *LAY_VAN_TAY_NB_O_KHAM_BENH; Chỉnh sửa lại cách khai báo thiết lập theo cấu trúc X/Y/A,B,C,...; Trong đó:
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Test on Stable | Ưu tiên: Nghiêm trọng | Phân hệ: Khám bệnh | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 15.2. [MVN HCM] Thiết lập bộ lọc thời gian mặc định hôm nay ở các mh Duyệt BH/ Mẫu QĐ130 BH (SAKURA-94091)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Viện muốn Bộ lọc thời gian set mặc định ngày hôm nay; Conf:* 3.2.39. Duyệt bảo hiểm (Duyệt BH) - SAKURA - ISOFH Confluence
- Nội dung cập nhật:
  + Hiện tại: Bộ lọc thời gian chưa set mặc định ngày hôm nay ở các mh Duyệt BH/ Mẫu QĐ130 BH
  + Sau khi cập nhật: Bộ lọc thời gian set mặc định ngày hôm nay ở các mh *Duyệt BH/ Mẫu QĐ130 BH; Ở mh Khám bệnh (Ds NB) chỉ hiển thị ds NB đúng theo bộ lọc thời gian (hôm nay); Set riêng cho site MVN, mã bv: vietnga
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Duyệt bảo hiểm, Khám bệnh | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 15.3. [QLYC-5651/PSHN] – Giấy chứng sinh: Thêm mới trường Không có giấy tờ tuỳ thân (mới) ở GIẤY CHỨNG SINH (SAKURA-93519)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Trên *GIẤY CHỨNG SINH* mong muốn hiển thị checkbox *Không có giấy tờ tuỳ thân* để bác sĩ có thể lựa chọn, những giấy được tích không có giấy tờ tùy thân sẽ được bỏ qua bắt buộc CCCD/BHXH của người bệnh. Ở màn hình giấy chứng sinh lọc được danh sách hồ sơ không có giấy tờ tùy thân.
- Nội dung cập nhật:
  + Hiện tại: Chưa có trường khongCoGiayToTuyThan; Hệ thống luôn bắt buộc nhập CCCD/BHXH
  + Sau khi cập nhật: Thêm trường dữ liệu:* khongCoGiayToTuyThan (boolean) - checkbox trên giấy chứng sinh; Trường hợp 1:* khongCoGiayToTuyThan = false/null; → Giữ nguyên như hiện tại:
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm lựa chọn mới trên màn hình liên quan khi thao tác.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Nghiêm trọng | Phân hệ: Danh sách giấy đẩy cổng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 15.4. [Tiếp đón] Sửa lại lấy trường thông tin từ ngày khi quét QR code các mã thẻ công an, quân đội, cơ yếu (SAKURA-94886)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Quét QR code các thẻ CA-QN-CI, trường thông tin *Bảo hiểm từ ngày ** đang lấy dữ liệu của trường *ngày cấp* trên thẻ BH
  + Sau khi cập nhật: Quét QR code các thẻ CA-QN-CI, trường thông tin *Bảo hiểm từ ngày ** sẽ lấy dữ liệu của trường *giá trị từ* trên thẻ BH (vị trí thứ 7 trên chuỗi); CA59 | 5068E1BAA16D2048E1BB936E6727 | 20/10/1981 | 1 | 42E1BB876E68207669E1BB876E203139382C2042E1BB992043C3B46E6720616E | 01-043 | 01/01/2026 | 31/12/2030 | 17/12/2025 | 393...
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Tiếp đón | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 15.5. Bảng kê chi phí khám bệnh, chữa bệnh 6556 thêm thiết lập BANG_KE_CHI_PHI_GROUP_THEO_NHOM_DICH_VU (SAKURA-94408, SAKURA-95567)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Bảng kê đang hiển thị theo: Nhóm dịch vụ khoa Khoa chỉ định → Nhóm dịch vụ cấp 1 hoặc ngược lại theo thiết lập chung BANG_KE_CHI_PHI_NHOM_DV_THEO_KHOA
  + Sau khi cập nhật: Thêm thiết lập BANG_KE_CHI_PHI_GROUP_THEO_NHOM_DICH_VU; BANG_KE_CHI_PHI_GROUP_THEO_NHOM_DICH_VU = 0/Không thiết lập; Hiển thị theo hiện tại
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Thu ngân | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 16. Hồ sơ bệnh án điện tử

### 16.1. [BV Nội tiết] Sổ giao ban Khoa phòng sửa số lượng tồn đầu kỳ loại những NB đang Chờ nhập viện (SAKURA-95095)

- Loại cập nhật: Sửa lỗi
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: NB trên không vào sổ do đang ở trạng thái: Chờ lập bệnh khác các trạng thái:; Đang điều trị; Đang chuyển khoa
- Thông tin tham chiếu: Loại: Lỗi | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Chốt sổ doanh thu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.2. [BV Nội tiết] Tờ điều trị ra viện màn hình Danh sách người bệnh nội trú, thêm thiết lập chung HIEN_THI_TO_DIEU_TRI_RA_VIEN (SAKURA-84517)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: lọc bỏ tờ điều trị sinh ra khi tạo đơn thuốc ra viện màn hình Danh sách người bệnh nội trú --> CHi tiết NB, tab Đơn thuốc ra viện:; Thực tế:* Chưa có; Mục đích:* do bên Nội tiết, Phổi không kí xác nhận tờ điều trị ra viện nên yêu cầu không sinh ra khi lưu thông tin Đơn thuốc ra viện
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Nội trú | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.3. [Danh sách giấy đẩy cổng] Chỉnh sửa tính năng Danh sách đẩy cổng bệnh truyền nhiễm (SAKURA-93963)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Chỉnh sửa lại bộ lọc icd và khoa; Cho phép lọc theo dạng search droplist; Cho phép chọn nhiều
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Danh sách giấy đẩy cổng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.4. [Danh sách giấy đẩy cổng] Thêm trạng thái Thất bại khi đẩy cổng người bệnh truyền nhiễm thất bại (SAKURA-95497)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Cho người dùng phân biệt và lọc được trên bộ lọc các hồ sơ đẩy cổng thất bại
- Nội dung cập nhật:
  + Hiện tại: Khi đẩy cổng thất bại thì không hiển thị trạng thái Thất bại, chỉ có thành công
  + Sau khi cập nhật: Hiển thị trạng thái Thất bại khi đẩy cổng thất bại; Mặc định khi sinh bản ghi truyền nhiễm trạng thái = Tạo mới
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Danh sách giấy đẩy cổng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.5. [EMR_BA224] Cho phép sửa Thời gian thực hiện trên Phiếu thực hiện kỹ thuật phục hồi chức năng (SAKURA-94416, SAKURA-94418)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Do quy trình hiện tại của BV là BS chỉ định dịch vụ sẽ tự động lưu lại thời gian thực hiện ở thời điểm kê. Kỹ thuật viên có thể làm Phiếu thực hiện kỹ thuật phục hồi chức năng vào hôm sau nên có thể sửa lại Thời gian thực hiện để thể hiện đúng
- Nội dung cập nhật:
  + Hiện tại: Chỉnh sửa trường *thoiGianThucHien* trong array dsTheoDoi không lưu được
  + Sau khi cập nhật: Thêm config "Sửa thời gian thực hiện DV" dạng check box:; True: Cho phép chỉnh sửa thời gian ở cột Ngày giờ; False: Không cho chỉnh sửa như hiện tại
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.6. [EMR_BA733] Cải thiện tự động tính toán các trường Thời gian lọc máu Rút ml/giờ, Rút ml/24h (SAKURA-94448)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Trường *Thời gian lọc máu* chưa tự động tính toán từ _Thời gian bắt đầu_ và _Thời gian kết thúc_.; Hai trường *Rút (ml/giờ)* và *Rút (ml/24h)* chưa có cơ chế tự động quy đổi qua lại, phải nhập tay từng trường
  + Sau khi cập nhật: Trường Thời gian lọc máu (Giờ); Tự động tính: = Thời gian kết thúc – Thời gian bắt đầu; Đơn vị hiển thị: Giờ
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.7. [Form phiếu/EMR_BA111] Thêm config cho đổi text ở trường "Định nhóm máu người cho" và "Kết quả hòa hợp tại giường" (SAKURA-94581)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Cho phép linh hoạt thay đổi từ ngữ theo từng bệnh viện/dự án
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Thêm config đổi text tùy ý tương tự như *Text Xác Định nhóm máu; Text Định nhóm máu người cho; Text* *Kết quả hòa hợp tại giường
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.8. [Form phiếu] Bổ sung key vào Phiếu tư vấn tiền sản (SAKURA-94825)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: QLYC-5730
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Thêm các key vào phiếu tư vấn tiền sản (EMR_TUDU1); | Tên trường | Mô tả |
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.9. [Form phiếu] Bổ sung trường STT ở tiêu chí in phiếu giải phẫu bệnh (SAKURA-95542)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: QLYC-6004
- Nội dung cập nhật:
  + Hiện tại: Không có
  + Sau khi cập nhật: Bổ sung STT vào Tiêu chí in Phiếu giải phẫu bệnh, STT tương tự màn hình chỉ định dịch vụ kỹ thuật
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.10. [Form phiếu] EMR_BA089 - Phiếu đếm gạc, mèche, củ ấu, dụng cụ: Bổ sung trường dữ liệu (SAKURA-93720)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung nhằm đảm bảo đầy đủ danh mục dụng cụ thực tế sử dụng trong chuyên môn
- Nội dung cập nhật:
  + Hiện tại: chưa có
  + Sau khi cập nhật: Bổ sung thêm 2 trường: *gạc cầu (củ ấu), bông sọ não* vào dsDungCu
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.11. [Form phiếu] EMR_BA089 - Phiếu đếm gạc, mèche, củ ấu, dụng cụ: Setting form hiển thị thêm trường (SAKURA-93727)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung nhằm đảm bảo đầy đủ danh mục dụng cụ thực tế sử dụng trong chuyên môn
- Nội dung cập nhật:
  + Hiện tại: chưa có
  + Sau khi cập nhật: Setting vào config form hiển thị thêm 2 trường: *gạc cầu (củ ấu), bông sọ não* vào Phiếu đếm gạc, mèche, củ ấu, dụng cụ
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.12. [Form phiếu] Hiển thi hhhv theo nv lấy tên bác sĩ kết luận (SAKURA-95452)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: [Form phiếu] Hiển thi hhhv theo nv lấy tên bác sĩ kết luận
- Tác động tới người dùng: Bác sĩ sẽ thấy thay đổi này khi thao tác trên phân hệ liên quan.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Ready for Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.13. [Form phiếu] Xử lý component Đống Đa theo form mới EMR_BA356 (SAKURA-90928)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Xử lý component Đống Đa theo form mới EMR_BA356; Đổi vị trí, sắp xếp lại bảng theo ảnh đính kèm và chuyển đổi các tùy chọn checkbox sang droplist, chi tiết theo file đính kèm
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm lựa chọn mới trên màn hình liên quan khi thao tác.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.14. [Giấy tờ đẩy cổng/ký số] Cho ký số USB token XML giấy tờ khi gửi cổng _ Gửi hàng loạt giấy tờ (SAKURA-94228)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có xử lý ở tính năng gửi hàng loạt
  + Sau khi cập nhật: Cho ký số file XML của giấy tờ trước khi đẩy cổng khi gửi hàng loạt (Xử lý chung cho các giấy tờ - nút gửi hàng loạt); Thiết lập chung XML_KY_SO_GIAY_DAY_CONG = true:* check thêm thiết lập; | Mã | Giá trị | Mô tả |
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm nút thao tác mới trên màn hình liên quan sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Danh sách giấy đẩy cổng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.15. [Giấy tờ đẩy cổng/ký số] Cho ký số USB token XML giấy tờ khi gửi cổng _ Gửi từng giấy tờ (SAKURA-94230)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có xử lý ở tính năng gửi hàng loạt
  + Sau khi cập nhật: Cho ký số file XML của giấy tờ trước khi đẩy cổng khi gửi hàng loạt (Xử lý chung cho các giấy tờ - nút gửi từng giấy tờ ); Thiết lập chung XML_KY_SO_GIAY_DAY_CONG = true:* check thêm thiết lập; | Mã | Giá trị | Mô tả |
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm nút thao tác mới trên màn hình liên quan sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Danh sách giấy đẩy cổng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.16. [Hồ sơ bệnh án] Cải thiện hiển thị Phiếu Medical Report - Kết quả khám (SAKURA-94509)

- Loại cập nhật: Sửa lỗi
- Lý do thực hiện và giá trị mang lại: xem chính xác được nội dung phiếu tương ứng đã chọn
- Nội dung cập nhật:
  + Hiện tại: Tại hồ sơ khám bệnh, phiếu khám chuyên khoa:; Khi NB có nhiều Phiếu khám chuyên khoa
  + Sau khi cập nhật: h4. Với danh sách phiếu:; Click vào từng phiếu:; Load đúng nội dung phiếu tương ứng
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Lỗi | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Hồ sơ bệnh án | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.17. [Kết nối TCKT] Cải thiện kết nối đồng bộ misa phiếu thu (SAKURA-95641)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: [Kết nối TCKT] Cải thiện kết nối đồng bộ misa phiếu thu
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Ready for Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.18. [Mắt TW] [QLYC-5591] Thêm tính năng cho component Image của form editor (SAKURA-92148)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: component Image* cho phép chỉnh sửa bằng cách nhập chữ, vẽ tự do,...... nhưng *chưa có vẽ đường thẳng (line*) và *sau khi lưu không cho undo* các thao tác trước đó -> muốn chỉnh sửa phải vẽ lại hoàn toàn.
  + Sau khi cập nhật: Component Image cho phép vẽ thêm đường thẳng (line) và có thể *undo* lại thao tác *sau khi bấm Lưu
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.19. [Mắt TW] Bắt điều kiện Chọn mắt trước khi chọn chẩn đoán Chẩn đoán ở Mh Lập bệnh án (SAKURA-94803)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Khi chỉnh sửa Chẩn đoán chính, Chẩn đoán kèm theo và Chẩn đoán phân biệt trong màn hình Lập bệnh án *không bắt buộc* chọn mắt; Đã có thiết lập: MA_NHOM_DV_CAP1_BAT_BUOC_BO_PHAN_KHI_KE
  + Sau khi cập nhật: Thêm thiết lập áp dụng cho màn hình; nếu có thiết lập chung MA_NHOM_DV_CAP1_BAT_BUOC_BO_PHAN_KHI_KE, nếu có khai báo *giá trị bất kỳ và có hiệu lực* thì bắt buộc chọn Lựa chọn mắt trước khi chọn 1 trong các chẩn đoán:; Chẩn đoán vào viện
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Lập bệnh án | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.20. [PSHN][Bệnh án] API nb-benh-an-chung trả thêm thông tin NB liên kết (SAKURA-94522)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Có thêm thông tin NB liên kết (có thể là vợ hoặc chồng ...) để làm cơ sở dữ liệu cho phiếu in đòi hỏi thông tin nhiều NB; Mã các form phiếu:
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: Sau khi liên kết hồ sơ của NB(vợ) và NB(chồng),API trả thêm thông tin NB liên kết. người dùng có thể in được các giấy tờ có liên quan đến cả thông tin của cả vợ và chồng,....
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Ready for Stable | Ưu tiên: Bình thường | Phân hệ: Hồ sơ bệnh án | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.21. [QLYC-5651/PSHN] – Giấy chứng sinh: Thêm mới trường Không có giấy tờ tuỳ thân (mới) ở GIẤY CHỨNG SINH (SAKURA-93496)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: hỗ trợ bác sĩ trong trường hợp sản phụ không có giấy tờ tuỳ thân, đồng bộ với xử lý BE và tránh bị chặn thao tác trên UI
- Nội dung cập nhật:
  + Hiện tại: chưa có checkbox “Không có giấy tờ tuỳ thân” trên giấy chứng sinh
  + Sau khi cập nhật: Thêm checkbox “Không có giấy tờ tuỳ thân” trên form *giấy chứng sinh; Xử lý trên phiếu:; Nếu không tích checkbox*: giữ nguyên bắt buộc nhập CCCD/BHXH như hiện tại
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm lựa chọn mới trên màn hình liên quan khi thao tác.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Nghiêm trọng | Phân hệ: Danh sách giấy đẩy cổng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.22. [TUDU0434] Thêm bộ lọc theo khoa vào màn hình danh sách tờ điều trị khám chuyên khoa (SAKURA-94148)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Người dùng muốn thêm tính năng lọc theo khoa điều trị của người bệnh ở màn hình danh sách tờ điều trị khám chuyên khoa.
- Nội dung cập nhật:
  + Hiện tại: Chưa có.
  + Sau khi cập nhật: Thêm bộ lọc Khoa. Cho chọn nhiều; Chọn để get danh sách tất cả các khoa trong API danh mục phòng tổng hợp. Người dùng chọn để lọc danh sách tờ điều trị khám chuyên khoa theo Khoa đang điều trị.
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Quản lý nội trú | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.23. [TUDU0434] Thêm bộ lọc theo phòng vào màn hình danh sách tờ điều trị khám chuyên khoa (SAKURA-93641)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Người dùng chưa lọc được danh sách tờ điều trị khám chuyên khoa theo phòng
  + Sau khi cập nhật: Thêm bộ lọc Phòng. Cho chọn nhiều; Chọn để get danh sách tất cả các phòng người bệnh trong API danh sách phòng thuộc khoa trong tờ điều trị khám chuyên khoa. Người dùng chọn khoa -> chọn phòng -> Màn hình hiển thị các tờ điều trị theo phòng.
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Quản lý nội trú | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.24. [Đẩy cổng bệnh truyền nhiễm] Cải thiện API đẩy cổng bệnh truyền nhiễm (SAKURA-94444)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Hiện tại viện có đề cập tính năng nếu như NB đang điều trị nội trú
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Check API trên, nếu như huongDieuTri = null thì khi đẩy cổng, kiểm tra api nb-dot-dieu-tri:; Nếu doiTuongKcb = 1, 2 thì sẽ truyền Hình thức điều trị khi đẩy cổng = 11129; Nếu doiTuongKcb = 3 thì sẽ truyền Hình thức điều trị khi đẩy cổng = 11128
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Danh sách giấy đẩy cổng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.25. Chỉnh sửa trường Số lượng; Ngày thực hiện tại Phiếu yêu cầu sử dụng dịch vụ yêu cầu (SAKURA-95576)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Trường Số lượng ; từ ngày .. đến ngày ( User tự nhập)
  + Sau khi cập nhật: Fill dữ liệu trường( Chỉ show không edit):; Số lượng_ -> Lấy theo trường *Số lượng* ( Chi tiết dịch vụ); Từ ngày đến ngày_ -> Gộp lấy theo *Thời gian thực hiện* của DV
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 16.26. Thêm mới Phiếu chỉ định lọc máu (EMR_BA733) (SAKURA-93655)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: chưa có
  + Sau khi cập nhật: Thêm mới phiếu; Loại biểu mẫu: Editor; In tại các vị trí sau:
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 17. Quản lý dinh dưỡng

### 17.1. [Báo cáo K29] Trả thêm tên đường dùng "tenDuongDung" để hiển thị đường dùng lên báo cáo (SAKURA-94151)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: Trả thêm tên đường dùng "tenDuongDung" để hiển thị lên báo cáo; // code placeholder; "emailBenhVien": "bvdemo@gmail.com"
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 17.2. [Sàng lọc dinh dưỡng/(Editor) Phiếu sàng lọc nguy cơ SDD MST + SGA ] Làm thêm mã phiếu EDITOR để in gộp Phiếu sàng lọc dinh dưỡng MST + Phiếu đánh giá SGA (SAKURA-94038, SAKURA-94453)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Phiếu sàng lọc dinh dưỡng MST + Phiếu đánh giá SGA đang in 2 phiếu riêng biệt.; Form config:
  + Sau khi cập nhật: (1) Làm thêm mã phiếu EDITOR để in gộp Phiếu sàng lọc dinh dưỡng MST + Phiếu đánh giá SGA; Mã phiếu: *P1602; Với mỗi phiếu sàng lọc dinh dưỡng MST đã tồn tại phiếu đánh giá SGA ("tonTaiSga": true) -> Tạo phiếu in gộp SLDD MST + SGA
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu, Quản lý dinh dưỡng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 17.3. KDD16 - Thêm mới báo cáo thanh toán chế phẩm dinh dưỡng (SAKURA-93945, SAKURA-94118)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: KDD16 - Thêm mới báo cáo thanh toán chế phẩm dinh dưỡng
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 18. Quản lý tiêm chủng

### 18.1. [Chi tiết khám sàng lọc] Cập nhật cách lấy thông tin mã tiêm chủng (SAKURA-95589)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Để có thể cập nhật thông tin lịch sử tiêm chủng của người bệnh
- Nội dung cập nhật:
  + Hiện tại: Người bệnh được tiếp đón từ màn hình tiếp đón chung và vào chỉnh sửa thêm mã tiêm chủng vào thì hệ thống chưa lấy được mã tiêm chủng để call được API.
  + Sau khi cập nhật: Cập nhật thông tin mã tiêm chủng vào API cũng như URL trường hợp khi người bệnh được tiếp đón từ màn hình tiếp đón chung và vào chỉnh sửa thêm mã tiêm chủng; Nếu chưa có mã tiêm chủng thì cứ để API và URL như logic hiện tại
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Nghiêm trọng | Phân hệ: Tiêm chủng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 18.2. [Danh mục vắc xin] Cải thiện tính năng cập nhật thông tin trong Danh mục vắc xin (SAKURA-94505)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Khi cập nhật thông tin trường gói thầu, nhóm thầu, thông tin thầu, quyết định thầu trong Danh mục vắc xin, phần mềm báo "Cập nhật dữ liệu thành công" nhưng giá trị không đổi; Link:* Danh mục
  + Sau khi cập nhật: Sửa lại cách lấy và lưu dữ liệu giống với Danh mục thuốc; Sửa cả trường đường dùng để có thể chọn nhiều giá trị
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Rất nghiêm trọng | Phân hệ: Danh mục | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 18.3. [Form phiếu] Bổ sung trường Nghề nghiệp ở Phiếu khám sàng lọc tiêm chủng người lớn (SAKURA-95557)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: QLYC-5991
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: [Form phiếu] Bổ sung trường Nghề nghiệp ở Phiếu khám sàng lọc tiêm chủng người lớn
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 18.4. [TUDU0403] Thêm Thiết lập THEM_DICH_VU_VAO_DON_THUOC_VAC_XIN (SAKURA-94859)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bệnh viện cần hiển thị dịch vụ công tiêm vào phiếu in đơn thuốc vắc xin.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: Thêm các thông tin từ API tổng hợp dịch vụ kỹ thuật sang API in phiếu don_thuoc_vacxin.; Cách làm:; Thêm mới 1 thiết lập chung để quy định dịch vụ. Giá trị thiết lập là mã dịch vụ trong Bảng danh mục dịch vụ. Cho phép điền nhiều giá trị, mỗi giá trị cách nhau bởi dấu phẩy.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Thiết lập | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 19. Báo cáo vận hành và báo cáo quản trị

### 19.1. [BV198] Thêm mới báo cáo : [K58.2] Báo cáo tổng hợp xuất kho cho người bệnh (SAKURA-94317, SAKURA-94320)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Kho dược yêu cầu tạo mới mẫu báo cáo tổng hợp xuất hàng hóa sử dụng cho bệnh nhân
- Nội dung cập nhật:
  + Hiện tại: Chưa có báo cáo
  + Sau khi cập nhật: Thêm mới báo cáo : [K58.2] Báo cáo tổng hợp xuất kho cho người bệnh
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.2. [BVVĐ] BC KHTH22 sửa dữ liệu in chi tiết báo cáo, Loại BC và sử dụng giường tại khoa (SAKURA-95615)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: [BVVĐ] BC KHTH22 sửa dữ liệu in chi tiết báo cáo, Loại BC và sử dụng giường tại khoa
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Ready for Stable | Ưu tiên: Bình thường | Phân hệ: baocao | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.3. [BVVĐ] FE BC KHTH22 thêm checkbox NB có thời gian điều trị > 4 tiếng (SAKURA-95104)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: BC KHTH22; NB cấp cứu quy trình vào thẳng nội trú - đối tượng nội trú -> vào số liệu bc thống kê hoạt động điều trị
  + Sau khi cập nhật: Them checkbox NB > 4 tiếng; cache checkbox; True - Lọc và xử lý toàn bộ các ds và key loại bỏ các NB có thời gian ra viện (chưa ra viện - lây thời gian hiện tại) - thời gian vào viện < =4h . False - lấy all như hiện tại
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm lựa chọn mới trên màn hình liên quan khi thao tác.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Ready for Stable | Ưu tiên: Bình thường | Phân hệ: baocao | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.4. [Báo cáo tc28] Trường hợp sinh phiếu chi lên số tiền thu thêm, trả lại số dương theo công thức mới (SAKURA-95018)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: yêu cầu của bệnh viện E; /ảnh chụp MH/
- Nội dung cập nhật:
  + Hiện tại: Đang hiển thị các cột số thu thêm và trả lại lên số âm để đối ứng với phiếu thu
  + Sau khi cập nhật: Trường hợp sinh phiếu chi lên số tiền thu thêm, trả lại hiển thị số tiền là số dương theo công thức ngược lại với line phiếu thu ( các cột số tiền của Nb vẫn giữ nguyên hiển thị số tiền âm); Với line chi: Số thu thêm/ trả lại=* tổng chi phí nb trả - tài trợ bn cùng chi trả - tài trợ không bh - miễn giảm - tổng tiền tạm...
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Ready for Stable | Ưu tiên: Cao | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.5. [Báo cáo/EMR_HSDD094] Bổ sung và chỉnh sửa phiếu chăm sóc cấp 2, 3 mẫu Thủ Đức (SAKURA-92107)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: bổ sung theo yêu cầu nghiệp vụ bệnh viện Thủ Đức, giúp hiển thị đầy đủ thông tin chăm sóc.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: [Báo cáo/EMR_HSDD094] Bổ sung và chỉnh sửa phiếu chăm sóc cấp 2, 3 mẫu Thủ Đức
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.6. [Báo cáo] Bổ sung thêm các trường dữ liệu phục vụ AI làm báo cáo (SAKURA-95494)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Bổ sung thêm các trường dữ liệu phục vụ AI làm báo cáo; Trạng thái NB: enum trangThaiNb; Thời gian vào khoa: thời gian vào khoa của NB
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.7. [Báo cáo] Thêm tiêu chí lọc theo phòng thực hiện KHTH29 (SAKURA-93649)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Thêm tiêu chí lọc theo phòng thực hiện, droplist cho chọn nhiều
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.8. [Báo cáo] Xử lý logic đếm báo cáo và thêm tiêu chí lọc KHTH29 (SAKURA-93648)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Xử lý logic đếm báo cáo: key ds.ds.slTong, ds.slTong, slTong; TS Lượt khám: Đếm tổng số lượt khám (theo MHS) của bsi/ phòng khám; Từ TS lượt khám, xử lý các cột khác dựa vào TS lượt khám
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.9. [Danh sách giấy đẩy cổng] Thêm tính năng xuất Excel Danh sách đẩy cổng bệnh truyền nhiễm và thêm param lọc thời gian ra viện (SAKURA-93959)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Cải thiện giao diện, cho phép lọc thời gian ra viện
- Nội dung cập nhật:
  + Hiện tại: Chỉ lọc được thời gian vào viện
  + Sau khi cập nhật: Thêm param lọc thời gian ra viện cho API trên; tuThoiGianRaVien; denThoiGianRaVien
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Danh sách giấy đẩy cổng | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.10. [DKTD][Danh mục/Báo cáo khóa dữ liệu kho] Lỗi phân trang danh mục khóa bc dữ liệu kho (SAKURA-95605)

- Loại cập nhật: Sửa lỗi
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Đang không có line khóa do UI chưa xử lý phân trang
  + Sau khi cập nhật: Rà soát và fix lỗi này
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Lỗi | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Danh mục | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.11. [Form phiếu/EMR_BA406] Bổ sung config Hiển thị can thiệp gợi ý 2 (theo site BV Tim) (SAKURA-94890)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: viện không chấp nhận sử dụng mẫu hiện tại
- Nội dung cập nhật:
  + Hiện tại: chưa có phần Can thiệp gợi ý
  + Sau khi cập nhật: Bổ sung config *Hiển thị can thiệp 2; =true:; Vị trí: Trên hàng Người đánh giá (Ký tên), Khoa thực hiện
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Form Phiếu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.12. [K36] Thêm mới key Mã ký hiệu - Tên thương mại của hàng hóa khi in K36 (SAKURA-95518)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: theo yc lọc bc theo Mã ký hiệu - Tên thương mại; Xem theo thời gian duyệt phiếu nhập xuất kho_++_:_+
- Nội dung cập nhật:
  + Hiện tại: chưa có
  + Sau khi cập nhật: Hiển thị thêm key *Mã ký hiệu - Tên thương mại* của hàng hóa khi in K36; Mã ký hiệu - Tên thương mại lấy trong Danh mục vật tư (với loaiDichVuKho có khai báo *Mã ký hiệu - Tên thương mại* trong Danh mục)
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.13. [MATTW] [TC01.6.1] Thêm cột mã chuẩn chi theo phương thức thanh toán (SAKURA-94263)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: [MATTW] [TC01.6.1] Thêm cột mã chuẩn chi theo phương thức thanh toán
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Thanh toán điện tử | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.14. [MVNHCM] Hiển thị toàn bộ tên bộ chỉ định ở trường bộ chỉ định báo cáo BC10.1 (SAKURA-93366)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Tên bộ chỉ định trong filter bị giới hạn chiều rộng và bị cắt bằng dấu ba chấm (...) -> Người dùng không xem được đầy đủ tên bộ chỉ định.; Mục đích/ Lý do cải thiện:* Bác sĩ mong muốn được xem đầy đủ thông tin bộ chỉ định
  + Sau khi cập nhật: Chuyển trường *"Bộ chỉ định"* xuống dưới bộ lọc Loại ngày.; Bộ lọc dài bằng 2 ô bộ lọc, đối với các dòng quá dài (vượt khỏi combobox) thì wrap xuống dòng cho bộ chỉ định.
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: baocao | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.15. [Mắt TW] [QLYC-5774] Bổ sung điều kiện lọc Khoa thực hiện, phòng thực hiện vào mẫu BC03 (SAKURA-93826)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: Bổ sung thêm cho BC03 2 điều kiện lọc:; Khoa thực hiện (Droplist*); Hiển thị tên khoa trong Danh mục Khoa có:
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: baocao | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.16. [PSHN] BC10.1 Thêm key hiển thị kết quả với dịch vụ có chỉ số con (SAKURA-90820)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: Thêm checkbox; Tên checkbox: Tách cột kết quả chỉ số con; params: tachChiSoCon
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm lựa chọn mới trên màn hình liên quan khi thao tác.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.17. [PSHN] K20.1 - thêm mới bộ lọc Mẫu báo cáo (SAKURA-95448, SAKURA-95449)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: BE Thêm bộ lọc mới; Tên bộ lọc*: Mẫu báo cáo; Giá trị:
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.18. [PSHN][Báo Cáo] Trả thêm trường ngày hợp đồng , ngày hết hạn hợp đồng vào báo cáo K36 (SAKURA-94235)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: PSHN muốn trả thêm key ngày hết hạn và ngày hợp đồng cho báo cáo K36
- Nội dung cập nhật:
  + Hiện tại: Chưa có thêm key ngày hợp đồng, ngày hết hạn hợp đồng
  + Sau khi cập nhật: Trả thêm key ngày hợp đồng, ngày hết hạn hợp đồng cho báo cáo K36; Ngày hợp đồng : Lấy dữ liệu từ trường ngày hợp đồng tại màn chi tiết thầu; Ngày hết hạn hợp đồng : Lấy dữ liệu từ trường ngày hết hạn hợp đồng tại màn chi tiết thầu
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.19. [QLYC-3977] Hiển thị dữ liệu lên BCKHTH08.1 (SAKURA-95562)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: FE hiển thị các trường sau lên BC KHTH08.1:; > Hiển thị thêm thông tin mẹ con (Checkbox); Khi thongTinMeCon = true: trả dữ liệu cho nhóm thông tin mẹ/con.
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra thêm lựa chọn mới trên màn hình liên quan khi thao tác.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: baocao | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.20. [QLYC-3977] Trả thêm các trường dữ liệu sau về Báo cáo KHTH08.1 (SAKURA-94137)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: BE trả thêm các trường sau về báo cáo:; ● Cân nặng lúc sinh; ● Tuổi thai
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.21. [SAKURA-92658] Sửa lỗi hiển thị cột trống ở PTTT trên BC28 (SAKURA-95700)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Đang hiển thị cột trống ở PTTT trên BC28
  + Sau khi cập nhật: Chỉ hiển thị các PTTT được sử dụng, PTTT nào không sử dụng --> Không hiển thị; | Mã BC | Tên BC | Mã quyền |; | bc_28 | Báo cáo chi tiết người bệnh sử dụng bộ chỉ định | 1500133 |
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.22. [TC01] Xử lý lỗi ko lấy lên được số hóa đơn (SAKURA-95719)

- Loại cập nhật: Sửa lỗi
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: [TC01] Xử lý lỗi ko lấy lên được số hóa đơn
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Thông tin tham chiếu: Loại: Lỗi | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.23. [ĐKBP] Thay đổi trường lấy lên 2 thiết lập trong báo cáo KHTH64 (SAKURA-95543)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Cột Đường huyết, HbA1c không có dữ liệu lấy lên báo cáo
- Nội dung cập nhật:
  + Hiện tại: KET_QUA_DUONG_HUYET_KHTH64 lấy dữ liệu từ trường; ket_qua_duong_huyet
  + Sau khi cập nhật: Thay đổi trường dữ liệu lấy lên từ thiết lập chung:; KET_QUA_DUONG_HUYET_KHTH64: lấy trường; ket_qua_duong_huyet_chi_so_con
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: baocao | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.24. Báo cáo TC06 chọn Loại thời gian = Theo thời gian thanh toán xử lý với NB sinh chi (SAKURA-94956)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: NB sinh chi đang hiển thị dữ liệu ở cả ngày thanh toán đầu, và ngày thanh toán sinh chi; NB 2512250548 - TÒNG HẢI ÂU
  + Sau khi cập nhật: Với trường hợp NB sinh chi đẩy hết dữ liệu vào ngày thanh toán đầu tiên theo thiết lập chung; XML_NGAY_TTOAN_THEO_PHIEU_CHI; Link nghiệp vụ:
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Thu ngân | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.25. Sắp xếp lại thứ tự trong XML tc98 (SAKURA-93344)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: <NGAY_VAO> đang xếp trước <STT>
  + Sau khi cập nhật: sắp xếp lại thứ tự trường trong file xml theo thứ tự; <STT>; <HO_TEN>
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: baocao | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.26. Sửa tc101 trường loại đối tượng lấy theo phiếu thu (SAKURA-95747)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Đang lấy dữ liệu theo dịch vụ
  + Sau khi cập nhật: Lấy dữ liệu theo phiếu thu
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Thu ngân | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 19.27. TC01.8 - Sửa lại báo cáo lấy lên theo phiếu thu (SAKURA-95607)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: tc01.8 vừa sửa lấy theo chi tiết dv để khớp với tc01.13, nhưng lại bị lệch tiền với báo cáo bên hoá đơn
  + Sau khi cập nhật: Sửa TC01.8 lấy lên tổng tiền theo phiếu thu
- Tác động tới người dùng: Người dùng khai thác báo cáo sẽ thấy thay đổi này khi tra cứu hoặc xuất dữ liệu.
- Hướng dẫn sử dụng: Người dùng kiểm tra lại tiêu chí lọc hoặc mẫu in trước khi xuất dữ liệu.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Báo cáo | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 20. Quản trị người dùng và phân quyền

### 20.1. [DMNV] Thêm tính năng khi chọn nhiều khoa quản lý sẽ đẩy đóng gói được nhiều khoa khi tài khoản được gán nhiều khoa quản lý (SAKURA-95609)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Khi 1 tài khoản gán nhiều khoa quản ký nhưng chỉ đẩy đóng gói được ở 1 khoa quản lý chọn ban đầu
  + Sau khi cập nhật: tài khoản chọn nhiều khoa quản lý thì sẽ đẩy đóng gói ở tất cả các khoa được gán; link nb:
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: nhiemvu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## 21. Khác hoặc chưa phân hệ

### 21.1. [BV Nội tiết] Ghép trường Bs trực theo trường dsNhanVienNhanId, sửa cấu trúc trường nhanVienNhan2Id thành dsNhanVienNhan2Id (SAKURA-95144)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: trong 1 tua trực có nhiều BS và điều dưỡng cùng trực nên cần thay đổi; Link nghiệp vụ:
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: trường dsNhanVienNhanId ghép cho trường BS trực; đổi cấu trúc dữ liệu trường Điều dưỡng trực (nhanVienNhan2Id) → thành dsNhanVienNhan2Id; Thực tế:
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Chốt sổ doanh thu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 21.2. [BV Nội tiết] thêm trường dsNhanVienNhanId, sửa cấu trúc trường nhanVienNhan2Id thành dsNhanVienNhan2Id (SAKURA-94283)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: trong 1 tua trực có nhiều BS và điều dưỡng cùng trực nên cần thay đổi; Link nghiệp vụ:
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: thêm trường dsNhanVienNhanId; đổi cấu trúc dữ liệu trường Điều dưỡng trực (nhanVienNhan2Id) → thành dsNhanVienNhan2Id; Thực tế:
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Chốt sổ doanh thu | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 21.3. [Chi tiết NB đã tiếp đón] Thêm bộ lọc Mã dịch vụ, Tên dịch vụ tại màn Chi tiết người bệnh đã tiếp đón (SAKURA-94352)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Đang chưa lọc được cột mã dịch vụ, tên dịch vụ.
  + Sau khi cập nhật: Thêm bộ lọc Mã dịch vụ, Tên dịch vụ tại Danh sách dịch vụ - Chi tiết người bệnh đã tiếp đón; | STT | Tên control | Loại control | Require | Mô tả/Ràng buộc khác |; | 1 | Lọc mã dịch vụ | Textbox | Không | - Nhập từ khóa để tìm kiếm theo *Mã dịch vụ* (cột "Mã dịch vụ")
- Hướng dẫn sử dụng: Người dùng có thể sử dụng thêm tiêu chí lọc mới trên màn hình sau khi cập nhật.
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: QLYC | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 21.4. [Chi tiết NB đã tiếp đón] Thêm param Tên dịch vụ tại màn Chi tiết người bệnh đã tiếp đón (SAKURA-94351)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: [Chi tiết NB đã tiếp đón] Thêm param Tên dịch vụ tại màn Chi tiết người bệnh đã tiếp đón
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: QLYC | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 21.5. [Chỉ định dịch vụ/Thu tạm ứng] Xử lý thiết lập DS_MA_BAO_CAO_IN_PHIEU_CHI_DINH thêm trên api/his/v1/nb-dv-xet-nghiem/phieu-chi-dinh (SAKURA-95687)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Chưa có
  + Sau khi cập nhật: [Chỉ định dịch vụ/Thu tạm ứng] Xử lý thiết lập DS_MA_BAO_CAO_IN_PHIEU_CHI_DINH thêm trên api/his/v1/nb-dv-xet-nghiem/phieu-chi-dinh
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Thiết lập | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 21.6. [Quản lý tạm ứng] Hiển thị thêm cột Lý do hủy tạm ứng ở tab Hủy tạm ứng (SAKURA-95083)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: chưa có hiển thị cột Lý do hủy tạm ứng
  + Sau khi cập nhật: / Thêm cột hiển thị Lý do hủy tạm ứng: trường lyDoHuyTamUng, mặc định cho hiển thị cột này; / Thêm cài đặt hiển thị bảng ở tab Hủy tạm ứng; Mục đích: cho ng dùng tùy chọn ẩn hiện các cột ở tab Hủy tạm ứng
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Thu ngân | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 21.7. [Tùy chỉnh giao diện] Hiển thị các trường trong popup chi tiết người bệnh đẩy cổng (SAKURA-95534)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Hiển thị đẩy đủ các trường cho người dùng tùy chỉnh theo nhu cầu từng viện
- Nội dung cập nhật:
  + Hiện tại: Đang thiếu nhiều trường
  + Sau khi cập nhật: Hiển thị đầy đủ các trường trong confluence mô tả lên tính năng tùy chỉnh giao diện phần đầy cổng bệnh truyền nhiễm; Chi tiết trong confluence
- Thông tin tham chiếu: Loại: Cải thiện, Nâng cấp | Trạng thái: Passed Stable | Ưu tiên: Cao | Phân hệ: Tùy chỉnh giao diện | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 21.8. [VNPT-Phát hành hóa đơn dịch vụ ngoài] Xử lý Tính năng điều chỉnh hóa đơn điện tử dịch ngoài từ màn hình Chi tiết hóa đơn điện tử dịch vụ ngoài (SAKURA-91363, SAKURA-94210)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Làm thêm tính năng cho BV E sử dụng để xuất hóa đơn cho các dịch vụ ngoài dịch vụ KCB, Cho các đối tượng không phải NB
- Nội dung cập nhật:
  + Hiện tại: Chưa ghi nhận mô tả hiện trạng chi tiết trên JIRA.
  + Sau khi cập nhật: Xử lý Tính năng điều chỉnh hóa đơn điện tử dịch ngoài từ màn hình Chi tiết hóa đơn điện tử dịch vụ ngoài
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Hóa đơn điện tử | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

### 21.9. Thực hiện làm tròn với tổng tiền ở Thông báo hóa đơn điện tử lập sai (SAKURA-87706)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Bổ sung hoặc điều chỉnh theo nhu cầu vận hành thực tế tại đơn vị sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Đang trả về số lẻ
  + Sau khi cập nhật: Trả về số nguyên, thực hiện quy tắc làm tròn; Với tiền lẻ sau dấu phẩy < 5 -> Làm tròn xuống; Với tiền lẻ sau dấu phẩy >=5 -> Làm tròn lên
- Thông tin tham chiếu: Loại: Tính năng | Trạng thái: Passed Stable | Ưu tiên: Bình thường | Phân hệ: Thu ngân | Phiên bản: Đẩy code 05/05/2026_HIS_SAKURA

## Người dùng cần lưu ý

- Người dùng nên tải lại trình duyệt sau thời điểm cập nhật để nhận đầy đủ thay đổi về giao diện, nút thao tác và quyền mới.
- Với các chức năng có phát sinh quyền mới, Quản trị hệ thống cần kiểm tra việc gán quyền trước khi đưa vào sử dụng.
- Một số thay đổi chỉ hiển thị khi Cơ sở y tế đã bật cấu hình hoặc đang sử dụng đúng biểu mẫu, báo cáo, quy trình liên quan.

## Giả định và giới hạn dữ liệu

- Version 23165 trên JIRA chưa khai báo ngày phát hành. Tài liệu này đang dùng ngày 05/05/2026 theo tên version: Đẩy code 05/05/2026_HIS_SAKURA.
- Một số ticket chỉ mô tả ngắn trên JIRA, vì vậy nội dung thông báo đã được biên tập và rút gọn theo summary cùng mô tả hiện có.
- Các ticket nội bộ hoặc chứa thông tin nhạy cảm như đổi secret key, thay đổi entity thuần kỹ thuật, ticket BA hoặc ticket kiểm thử đã được loại khỏi thông báo.

## Danh sách ticket tham chiếu

- SAKURA-95759: [Danh mục địa chỉ hành chính] Fix lỗi tab Quốc gia, Tỉnh/TP không đổi size được - Quản lý danh mục dùng chung | Sửa lỗi | Passed Stable
- SAKURA-94710, SAKURA-95243: [Kiosk tiếp đón] Thêm mới API get thông tin đợt điều trị - Tiếp đón và đăng ký khám | Cải tiến | Passed Stable
- SAKURA-94701: [Kiosk tiếp đón] Thêm thiết lập bắt trùng thông tin lấy số - Tiếp đón và đăng ký khám | Cải tiến | Passed Stable
- SAKURA-95517: [Kết nối TTTM- Viện E]Tiếp đón: Cho phép chỉ định nhiều DV khám với NB ngoại viện nếu CHI_DINH_NHIEU_CONG_KHAM_TAI_TIEP_DON = FALSE - Tiếp đón và đăng ký khám | Tính năng mới | Ready for Stable
- SAKURA-95460: [Mắt TW] [Tiếp đón] Xử lý không tự động tick ưu tiên theo lần khám trước của nb - Tiếp đón và đăng ký khám | Tính năng mới | Passed Stable
- SAKURA-94201: [PSHN][QMS] Sắp xếp lại giá trị các đối tượng ưu tiên - Tiếp đón và đăng ký khám | Cải tiến | Passed Stable
- SAKURA-94166: [Tiếp đón] Trả thêm trường Quân hàm, Chức vụ, Đơn vị vào API check trùng khi tiếp đón NB cũ - Tiếp đón và đăng ký khám | Cải tiến | Passed Stable
- SAKURA-94167: [Tiếp đón] Tự fill thông tin trường Quân hàm, Chức vụ, Đơn vị khi tiếp đón NB cũ - Tiếp đón và đăng ký khám | Cải tiến | Passed Stable
- SAKURA-95502: [BVT0101] Điều chỉnh thiết lập CHI_DINH_THUOC_KE_THEO_THU_VA_BUOI - Khám ngoại trú | Cải tiến | Passed Stable
- SAKURA-94295: [Form phiếu] Thêm key vào phiếu các phiếu bên dưới - Khám ngoại trú | Cải tiến | Passed Stable
- SAKURA-93638: [Form phiếu][EMR_BA259.1] Cải thiện Logic liên kết dữ liệu từ màn hình khám bệnh sang phiếu KSK lái xe theo TT36 BYT - Khám ngoại trú | Cải tiến | Passed Stable
- SAKURA-95579: [Khám bệnh] Cải thiện trường Ưu tiên nơi lấy mẫu bệnh phẩm - Khám ngoại trú | Cải tiến | Passed Stable
- SAKURA-93881: [Khám bệnh] Thêm trường vào Chuyên khoa sản - Khám ngoại trú | Cải tiến | Passed Stable
- SAKURA-94645: [Khám bệnh/Chỉ định] Trả thêm thông tin định mức chỉ định theo tuần vào dsPhongThucHien - Khám ngoại trú | Cải tiến | Test on Stable
- SAKURA-95702: [Khám bệnh] Bấm in Phân phòng không tải lại màn hình nên lưu sửa dịch vụ bị trả về phòng cũ - Khám ngoại trú | Sửa lỗi | Passed Stable
- SAKURA-91885: [Khám bệnh] Bổ sung Thiết lập chung bắt điều kiện tăng biến đếm số lượng tại Header trạng thái khám của người bệnh - Khám ngoại trú | Tính năng mới | Passed Stable
- SAKURA-94847: [Khám bệnh] Bổ sung Thiết lập chung không tự động gọi API chuyển trạng thái Đang khám tại thanh Header trạng thái khám người bệnh - Khám ngoại trú | Cải tiến | Passed Stable
- SAKURA-95554: [QLYC-5937] Đổi tên trường thông tin ở màn hình Khám bệnh - Khám ngoại trú | Tính năng mới | Passed Stable
- SAKURA-95458: [TUDU0464] Bổ sung thiết lập chung cho phép ẩn cột Giá trong popup chỉ định DVKT ở tất cả các màn hình - Khám ngoại trú | Cải tiến | Passed Stable
- SAKURA-95685: [TUDU0464] Lỗi ẩn cột đơn giá khi có thiết lập AN_COT_GIA_TIEN_CHI_DINH_DICH_VU_KB - Khám ngoại trú | Sửa lỗi | Passed Stable
- SAKURA-93748: [Chỉ định dịch vụ] Cải thiện chỉ định gói dịch vụ có dịch vụ đã tắt hiệu lực - Chỉ định dịch vụ | Cải tiến | Ready for Stable
- SAKURA-94326: [Từ dũ] [Tiếp đón giao diện 2] Lỗi bị duplicate khi chỉ định các dịch vụ khám - Chỉ định dịch vụ | Sửa lỗi | Passed Stable
- SAKURA-95585: [Kê thuốc] Lỗi nhấn F2 không quay lại vị trí tìm kiếm tên thuốc - Kê đơn ngoại trú | Sửa lỗi | Passed Stable
- SAKURA-95660: [BV PSTW] Số lượng thực dùng hiển thị sai định dạng thập phân - Quản lý nội trú | Cải tiến | Passed Stable
- SAKURA-95513: [BVP] Nb hẹn điều trị, lấy chẩn đoán đoán mô tả chi tiết từ thông tin ra viện của lượt điều trị gần nhất vào tờ điều trị của lượt điều trị hiện tại - Quản lý nội trú | Cải tiến | Passed Stable
- SAKURA-93911: [PSHN] Giường tự chọn: Lỗi cập nhật thời gian nằm đến của line trước khi cho ra viện - Quản lý nội trú | Cải tiến | Passed Stable
- SAKURA-93559: [TUDU0434] Thêm button Xuất danh sách vào màn hình danh sách tờ điều trị khám chuyên khoa - Quản lý nội trú | Cải tiến | Passed Stable
- SAKURA-93628: [TUDU0434] Thêm tính năng Xuất danh sách màn hình danh sách tờ điều trị khám chuyên khoa - Quản lý nội trú | Cải tiến | Passed Stable
- SAKURA-94277: [Báo cáo] Thêm các tiêu chí lọc vào báo cáo TC05 BÁO CÁO CHI TIẾT THU TIỀN THEO BÁC SĨ - Quản lý điều trị ngoại trú | Cải tiến | Passed Stable
- SAKURA-95446: [MVNHCM] [Báo cáo] Thêm logic lấy key Ký quỹ ngoại trú/nội trú ở báo cáo TC42 - Quản lý điều trị ngoại trú | Cải tiến | Passed Stable
- SAKURA-94854: [Ngoại trú] Thêm button Nguồn tài trợ cho bác sĩ ngoại trú - Quản lý điều trị ngoại trú | Cải tiến | Passed Stable
- SAKURA-94232, SAKURA-94375: [TC82.1] Thêm bộ lọc KSK hợp đồng cho TC82.1 - Quản lý khám sức khỏe hợp đồng | Cải tiến | Passed Stable
- SAKURA-89744, SAKURA-89795: [Tờ điều trị/Chỉ định thuốc] Thêm trường Ngày SD thuốc cho phép theo dõi STT ngày sử dụng thuốc với đơn nhà thuốc và đơn kê ngoài - Quản lý khám sức khỏe hợp đồng | Cải tiến | Passed Stable
- SAKURA-95535: [BVCR] Lọc các trường trong danh mục dịch vụ kĩ thuật không hoạt động đúng - Quản lý chẩn đoán hình ảnh, thăm dò, phục hồi chức năng | Cải tiến | Passed Stable
- SAKURA-93458: [BVE] Danh sách PTTT: cho hiển thị thông tin người PTV/TTV chính, Phụ 1/Phụ 2/phụ 3 - Quản lý chẩn đoán hình ảnh, thăm dò, phục hồi chức năng | Tính năng mới | Passed Stable
- SAKURA-95531: [BVP] Lọc "Không sinh báo cáo" không hoạt động đúng - Quản lý chẩn đoán hình ảnh, thăm dò, phục hồi chức năng | Cải tiến | Passed Stable
- SAKURA-95525: [Chỉ định dịch vụ/Thu tạm ứng] Thêm thiết lập DS_MA_BAO_CAO_IN_PHIEU_CHI_DINH - Quản lý chẩn đoán hình ảnh, thăm dò, phục hồi chức năng | Cải tiến | Passed Stable
- SAKURA-93923: [CĐHA] - Bổ sung nút in trên màn chi tiết dịch vụ - Quản lý chẩn đoán hình ảnh, thăm dò, phục hồi chức năng | Cải tiến | Passed Stable
- SAKURA-95611: [EDITOR] Component Table: Sửa lỗi ô check box Ẩn viền - Quản lý chẩn đoán hình ảnh, thăm dò, phục hồi chức năng | Sửa lỗi | Passed Stable
- SAKURA-95638: [Form phiếu][cdha_ket_qua_chung] Cải thiện logic trả key chieuCao cho Biểu mẫu - Quản lý chẩn đoán hình ảnh, thăm dò, phục hồi chức năng | Cải tiến | Passed Stable
- SAKURA-93481, SAKURA-94604: [Tiếp nhận NB KSK] Thêm điều kiện lấy danh sách tiếp đón NB KSK - Quản lý chẩn đoán hình ảnh, thăm dò, phục hồi chức năng | Cải tiến | Passed Stable
- SAKURA-95550: Cho phép sửa thông tin người thực hiện khi dv hoàn thành khi dc gán quyền 2300113 - Quản lý chẩn đoán hình ảnh, thăm dò, phục hồi chức năng | Cải tiến | Passed Stable
- SAKURA-94385: [Nội tiết] EMR_BA153: xử lý trả về thông tin phiếu của hồ sơ mới nhất - Quản lý phẫu thuật - thủ thuật | Cải tiến | Passed Stable
- SAKURA-95588: [PSHN] Đổi popup thông báo khi TK bị chặn đổi khoa chỉ định dịch vụ ở màn hình danh sách phẫu thuật thủ thuật - Quản lý phẫu thuật - thủ thuật | Sửa lỗi | Passed Stable
- SAKURA-95564: [PSHN]EMR_BA332 và EMR_BA332.1/ Phiếu bàn giao người bệnh: Thêm trường để lưu thông tin Bác sĩ phẫu thuật, thời gian dự kiến, Phương pháp phẫu thuật - Quản lý phẫu thuật - thủ thuật | Cải tiến | Ready for Stable
- SAKURA-92911: [PTTT] Thêm cơ chế hoàn thành đồng thời các dịch vụ phẫu thuật con khi bấm "Hoàn thành tất cả PT" - Quản lý phẫu thuật - thủ thuật | Cải tiến | Passed Stable
- SAKURA-93372: [Nhà thuốc - Chi tiết đơn thuốc] Cải thiện tính năng sao chép đơn nhà thuốc - Nhà thuốc | Cải tiến | Passed Stable
- SAKURA-95484: [Thiết lập chung/Chi tiết đơn thuốc] Thêm mới thiết lập không cho phép Tư vấn đơn để sửa SL bán > SL đã kê - Nhà thuốc | Cải tiến | Passed Stable
- SAKURA-95524: Tờ điều trị: Ngày thực hiện đang không cập nhật chính xác (kho nhà thuốc và thuốc kê ngoài) - Nhà thuốc | Sửa lỗi | Passed Stable
- SAKURA-95526: [BC K05, KNT07] Sửa lại bộ lọc Tên hàng hóa - Quản lý kho | Cải tiến | Passed Stable
- SAKURA-90811, SAKURA-90812: [BVE-Chi tiết thầu] Màn hình chi tiết thầu thêm trường Tên hàng hóa theo NCC - Quản lý kho | Cải tiến | Passed Stable
- SAKURA-94618: [TUDU0474] Thêm nút Hủy gửi duyệt tại màn hình chi tiết phiếu lĩnh (PTTT) - Quản lý kho | Cải tiến | Passed Stable
- SAKURA-90266: API kê thuốc kê ngoài trả thêm dữ liệu đơn vị tính sơ cấp, hệ số định mức - Quản lý kho | Tính năng mới | Passed Stable
- SAKURA-95705: [BV19-8] Trả thêm key vào phiếu xuất kho máu - Ngân hàng máu hoặc tủ máu | Cải tiến | Passed Stable
- SAKURA-95716: [Hiến máu] Xử lý trường ngày sản xuất khi chiết xuất chế phẩm máu - Ngân hàng máu hoặc tủ máu | Cải tiến | Passed Stable
- SAKURA-95519: [Quản lý hiến máu] Thêm quyền chỉnh sửa thông tin Hiến máu có trạng thái Đã duyệt - Ngân hàng máu hoặc tủ máu | Cải tiến | Passed Stable
- SAKURA-91774: [DS yêu cầu hoàn đổi] Cho phép hoàn đổi khi số lượng trả thực tế < số lượng yêu cầu trả ( kể cả số lượng thực tế trả = 0) - Theo dõi sử dụng thuốc | Cải tiến | Passed Stable
- SAKURA-92880: [Ký vân tay] Chỉnh sửa thiết lập bắt ký vân tay theo phòng khám - Quyết toán BHYT | Cải tiến | Test on Stable
- SAKURA-94091: [MVN HCM] Thiết lập bộ lọc thời gian mặc định hôm nay ở các mh Duyệt BH/ Mẫu QĐ130 BH - Quyết toán BHYT | Tính năng mới | Passed Stable
- SAKURA-93519: [QLYC-5651/PSHN] – Giấy chứng sinh: Thêm mới trường Không có giấy tờ tuỳ thân (mới) ở GIẤY CHỨNG SINH - Quyết toán BHYT | Cải tiến | Passed Stable
- SAKURA-94886: [Tiếp đón] Sửa lại lấy trường thông tin từ ngày khi quét QR code các mã thẻ công an, quân đội, cơ yếu - Quyết toán BHYT | Cải tiến | Passed Stable
- SAKURA-94408, SAKURA-95567: Bảng kê chi phí khám bệnh, chữa bệnh 6556 thêm thiết lập BANG_KE_CHI_PHI_GROUP_THEO_NHOM_DICH_VU - Quyết toán BHYT | Tính năng mới | Passed Stable
- SAKURA-95095: [BV Nội tiết] Sổ giao ban Khoa phòng sửa số lượng tồn đầu kỳ loại những NB đang Chờ nhập viện - Hồ sơ bệnh án điện tử | Sửa lỗi | Passed Stable
- SAKURA-84517: [BV Nội tiết] Tờ điều trị ra viện màn hình Danh sách người bệnh nội trú, thêm thiết lập chung HIEN_THI_TO_DIEU_TRI_RA_VIEN - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-93963: [Danh sách giấy đẩy cổng] Chỉnh sửa tính năng Danh sách đẩy cổng bệnh truyền nhiễm - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-95497: [Danh sách giấy đẩy cổng] Thêm trạng thái Thất bại khi đẩy cổng người bệnh truyền nhiễm thất bại - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-94416, SAKURA-94418: [EMR_BA224] Cho phép sửa Thời gian thực hiện trên Phiếu thực hiện kỹ thuật phục hồi chức năng - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-94448: [EMR_BA733] Cải thiện tự động tính toán các trường Thời gian lọc máu Rút ml/giờ, Rút ml/24h - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-94581: [Form phiếu/EMR_BA111] Thêm config cho đổi text ở trường "Định nhóm máu người cho" và "Kết quả hòa hợp tại giường" - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-94825: [Form phiếu] Bổ sung key vào Phiếu tư vấn tiền sản - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-95542: [Form phiếu] Bổ sung trường STT ở tiêu chí in phiếu giải phẫu bệnh - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-93720: [Form phiếu] EMR_BA089 - Phiếu đếm gạc, mèche, củ ấu, dụng cụ: Bổ sung trường dữ liệu - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-93727: [Form phiếu] EMR_BA089 - Phiếu đếm gạc, mèche, củ ấu, dụng cụ: Setting form hiển thị thêm trường - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-95452: [Form phiếu] Hiển thi hhhv theo nv lấy tên bác sĩ kết luận - Hồ sơ bệnh án điện tử | Tính năng mới | Ready for Stable
- SAKURA-90928: [Form phiếu] Xử lý component Đống Đa theo form mới EMR_BA356 - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-94228: [Giấy tờ đẩy cổng/ký số] Cho ký số USB token XML giấy tờ khi gửi cổng _ Gửi hàng loạt giấy tờ - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-94230: [Giấy tờ đẩy cổng/ký số] Cho ký số USB token XML giấy tờ khi gửi cổng _ Gửi từng giấy tờ - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-94509: [Hồ sơ bệnh án] Cải thiện hiển thị Phiếu Medical Report - Kết quả khám - Hồ sơ bệnh án điện tử | Sửa lỗi | Passed Stable
- SAKURA-95641: [Kết nối TCKT] Cải thiện kết nối đồng bộ misa phiếu thu - Hồ sơ bệnh án điện tử | Tính năng mới | Ready for Stable
- SAKURA-92148: [Mắt TW] [QLYC-5591] Thêm tính năng cho component Image của form editor - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-94803: [Mắt TW] Bắt điều kiện Chọn mắt trước khi chọn chẩn đoán Chẩn đoán ở Mh Lập bệnh án - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-94522: [PSHN][Bệnh án] API nb-benh-an-chung trả thêm thông tin NB liên kết - Hồ sơ bệnh án điện tử | Cải tiến | Ready for Stable
- SAKURA-93496: [QLYC-5651/PSHN] – Giấy chứng sinh: Thêm mới trường Không có giấy tờ tuỳ thân (mới) ở GIẤY CHỨNG SINH - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-94148: [TUDU0434] Thêm bộ lọc theo khoa vào màn hình danh sách tờ điều trị khám chuyên khoa - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-93641: [TUDU0434] Thêm bộ lọc theo phòng vào màn hình danh sách tờ điều trị khám chuyên khoa - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-94444: [Đẩy cổng bệnh truyền nhiễm] Cải thiện API đẩy cổng bệnh truyền nhiễm - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-95576: Chỉnh sửa trường Số lượng; Ngày thực hiện tại Phiếu yêu cầu sử dụng dịch vụ yêu cầu - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-93655: Thêm mới Phiếu chỉ định lọc máu (EMR_BA733) - Hồ sơ bệnh án điện tử | Cải tiến | Passed Stable
- SAKURA-94151: [Báo cáo K29] Trả thêm tên đường dùng "tenDuongDung" để hiển thị đường dùng lên báo cáo - Quản lý dinh dưỡng | Cải tiến | Passed Stable
- SAKURA-94038, SAKURA-94453: [Sàng lọc dinh dưỡng/(Editor) Phiếu sàng lọc nguy cơ SDD MST + SGA ] Làm thêm mã phiếu EDITOR để in gộp Phiếu sàng lọc dinh dưỡng MST + Phiếu đánh giá SGA - Quản lý dinh dưỡng | Cải tiến | Passed Stable
- SAKURA-93945, SAKURA-94118: KDD16 - Thêm mới báo cáo thanh toán chế phẩm dinh dưỡng - Quản lý dinh dưỡng | Tính năng mới | Passed Stable
- SAKURA-95589: [Chi tiết khám sàng lọc] Cập nhật cách lấy thông tin mã tiêm chủng - Quản lý tiêm chủng | Cải tiến | Passed Stable
- SAKURA-94505: [Danh mục vắc xin] Cải thiện tính năng cập nhật thông tin trong Danh mục vắc xin - Quản lý tiêm chủng | Cải tiến | Passed Stable
- SAKURA-95557: [Form phiếu] Bổ sung trường Nghề nghiệp ở Phiếu khám sàng lọc tiêm chủng người lớn - Quản lý tiêm chủng | Cải tiến | Passed Stable
- SAKURA-94859: [TUDU0403] Thêm Thiết lập THEM_DICH_VU_VAO_DON_THUOC_VAC_XIN - Quản lý tiêm chủng | Cải tiến | Passed Stable
- SAKURA-94317, SAKURA-94320: [BV198] Thêm mới báo cáo : [K58.2] Báo cáo tổng hợp xuất kho cho người bệnh - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-95615: [BVVĐ] BC KHTH22 sửa dữ liệu in chi tiết báo cáo, Loại BC và sử dụng giường tại khoa - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Ready for Stable
- SAKURA-95104: [BVVĐ] FE BC KHTH22 thêm checkbox NB có thời gian điều trị > 4 tiếng - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Ready for Stable
- SAKURA-95018: [Báo cáo tc28] Trường hợp sinh phiếu chi lên số tiền thu thêm, trả lại số dương theo công thức mới - Báo cáo vận hành và báo cáo quản trị | Tính năng mới | Ready for Stable
- SAKURA-92107: [Báo cáo/EMR_HSDD094] Bổ sung và chỉnh sửa phiếu chăm sóc cấp 2, 3 mẫu Thủ Đức - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-95494: [Báo cáo] Bổ sung thêm các trường dữ liệu phục vụ AI làm báo cáo - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-93649: [Báo cáo] Thêm tiêu chí lọc theo phòng thực hiện KHTH29 - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-93648: [Báo cáo] Xử lý logic đếm báo cáo và thêm tiêu chí lọc KHTH29 - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-93959: [Danh sách giấy đẩy cổng] Thêm tính năng xuất Excel Danh sách đẩy cổng bệnh truyền nhiễm và thêm param lọc thời gian ra viện - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-95605: [DKTD][Danh mục/Báo cáo khóa dữ liệu kho] Lỗi phân trang danh mục khóa bc dữ liệu kho - Báo cáo vận hành và báo cáo quản trị | Sửa lỗi | Passed Stable
- SAKURA-94890: [Form phiếu/EMR_BA406] Bổ sung config Hiển thị can thiệp gợi ý 2 (theo site BV Tim) - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-95518: [K36] Thêm mới key Mã ký hiệu - Tên thương mại của hàng hóa khi in K36 - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-94263: [MATTW] [TC01.6.1] Thêm cột mã chuẩn chi theo phương thức thanh toán - Báo cáo vận hành và báo cáo quản trị | Tính năng mới | Passed Stable
- SAKURA-93366: [MVNHCM] Hiển thị toàn bộ tên bộ chỉ định ở trường bộ chỉ định báo cáo BC10.1 - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-93826: [Mắt TW] [QLYC-5774] Bổ sung điều kiện lọc Khoa thực hiện, phòng thực hiện vào mẫu BC03 - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-90820: [PSHN] BC10.1 Thêm key hiển thị kết quả với dịch vụ có chỉ số con - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-95448, SAKURA-95449: [PSHN] K20.1 - thêm mới bộ lọc Mẫu báo cáo - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-94235: [PSHN][Báo Cáo] Trả thêm trường ngày hợp đồng , ngày hết hạn hợp đồng vào báo cáo K36 - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-95562: [QLYC-3977] Hiển thị dữ liệu lên BCKHTH08.1 - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-94137: [QLYC-3977] Trả thêm các trường dữ liệu sau về Báo cáo KHTH08.1 - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-95700: [SAKURA-92658] Sửa lỗi hiển thị cột trống ở PTTT trên BC28 - Báo cáo vận hành và báo cáo quản trị | Tính năng mới | Passed Stable
- SAKURA-95719: [TC01] Xử lý lỗi ko lấy lên được số hóa đơn - Báo cáo vận hành và báo cáo quản trị | Sửa lỗi | Passed Stable
- SAKURA-95543: [ĐKBP] Thay đổi trường lấy lên 2 thiết lập trong báo cáo KHTH64 - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-94956: Báo cáo TC06 chọn Loại thời gian = Theo thời gian thanh toán xử lý với NB sinh chi - Báo cáo vận hành và báo cáo quản trị | Tính năng mới | Passed Stable
- SAKURA-93344: Sắp xếp lại thứ tự trong XML tc98 - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-95747: Sửa tc101 trường loại đối tượng lấy theo phiếu thu - Báo cáo vận hành và báo cáo quản trị | Tính năng mới | Passed Stable
- SAKURA-95607: TC01.8 - Sửa lại báo cáo lấy lên theo phiếu thu - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Passed Stable
- SAKURA-95609: [DMNV] Thêm tính năng khi chọn nhiều khoa quản lý sẽ đẩy đóng gói được nhiều khoa khi tài khoản được gán nhiều khoa quản lý - Quản trị người dùng và phân quyền | Cải tiến | Passed Stable
- SAKURA-95144: [BV Nội tiết] Ghép trường Bs trực theo trường dsNhanVienNhanId, sửa cấu trúc trường nhanVienNhan2Id thành dsNhanVienNhan2Id - Khác hoặc chưa phân hệ | Cải tiến | Passed Stable
- SAKURA-94283: [BV Nội tiết] thêm trường dsNhanVienNhanId, sửa cấu trúc trường nhanVienNhan2Id thành dsNhanVienNhan2Id - Khác hoặc chưa phân hệ | Cải tiến | Passed Stable
- SAKURA-94352: [Chi tiết NB đã tiếp đón] Thêm bộ lọc Mã dịch vụ, Tên dịch vụ tại màn Chi tiết người bệnh đã tiếp đón - Khác hoặc chưa phân hệ | Cải tiến | Passed Stable
- SAKURA-94351: [Chi tiết NB đã tiếp đón] Thêm param Tên dịch vụ tại màn Chi tiết người bệnh đã tiếp đón - Khác hoặc chưa phân hệ | Cải tiến | Passed Stable
- SAKURA-95687: [Chỉ định dịch vụ/Thu tạm ứng] Xử lý thiết lập DS_MA_BAO_CAO_IN_PHIEU_CHI_DINH thêm trên api/his/v1/nb-dv-xet-nghiem/phieu-chi-dinh - Khác hoặc chưa phân hệ | Cải tiến | Passed Stable
- SAKURA-95083: [Quản lý tạm ứng] Hiển thị thêm cột Lý do hủy tạm ứng ở tab Hủy tạm ứng - Khác hoặc chưa phân hệ | Cải tiến | Passed Stable
- SAKURA-95534: [Tùy chỉnh giao diện] Hiển thị các trường trong popup chi tiết người bệnh đẩy cổng - Khác hoặc chưa phân hệ | Cải tiến | Passed Stable
- SAKURA-91363, SAKURA-94210: [VNPT-Phát hành hóa đơn dịch vụ ngoài] Xử lý Tính năng điều chỉnh hóa đơn điện tử dịch ngoài từ màn hình Chi tiết hóa đơn điện tử dịch vụ ngoài - Khác hoặc chưa phân hệ | Tính năng mới | Passed Stable
- SAKURA-87706: Thực hiện làm tròn với tổng tiền ở Thông báo hóa đơn điện tử lập sai - Khác hoặc chưa phân hệ | Tính năng mới | Passed Stable

Mọi vướng mắc trong quá trình sử dụng vui lòng phản hồi lại đội triển khai để được hỗ trợ kịp thời.
