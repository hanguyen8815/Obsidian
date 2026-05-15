# Thông báo cập nhật tính năng Ver.HIS_SAKURA26_5.3.0 và HIS_SAKURA26_5.4.0 ngày 14/05/2026

- Phiên bản: HIS_SAKURA26_5.3.0 và HIS_SAKURA26_5.4.0
- Đối tượng nhận: Người dùng hệ thống HIS/EMR SAKURA
- Ngày phát hành: 14/05/2026
- Kỳ cập nhật: Đợt cập nhật từ 12/05/2026 đến 14/05/2026
- Phạm vi: JIRA version 23189, 23187 - Đẩy code 12/05/2026 và 14/05/2026_HIS_SAKURA

Thông báo này tổng hợp các thay đổi chính của 2 đợt phát hành HIS_SAKURA26_5.3.0 và HIS_SAKURA26_5.4.0. Nội dung đã được biên tập theo góc nhìn vận hành, bổ sung rõ mục đích thay đổi, trường mới, quyền mới và phạm vi ảnh hưởng khi dữ liệu JIRA cho phép.

## Điểm nổi bật

- Hoàn thiện nhiều luồng thao tác tại Tiếp đón, Khám ngoại trú, Chỉ định dịch vụ và Quản lý nội trú để giảm sai sót khi thao tác.
- Bổ sung nhiều biểu mẫu, phiếu chăm sóc, phiếu lọc máu, phiếu dinh dưỡng và logic lấy dữ liệu cho Hồ sơ bệnh án điện tử.
- Cập nhật diện rộng cho Danh mục, Quản lý thầu, Kho, Nhà thuốc và dữ liệu phục vụ xuất XML Thông tư 12.
- Bổ sung mới và điều chỉnh nhiều báo cáo vận hành, tài chính, kho, phẫu thuật - thủ thuật và báo cáo phục vụ quyết toán.
- Hoàn thiện thêm các cơ chế phân quyền, bảo mật thông tin và hóa đơn điện tử theo nhu cầu vận hành đặc thù của từng Cơ sở y tế.

## Nội dung cập nhật theo Phân hệ

## 1. Quản lý danh mục dùng chung

### 1.1. Hoàn thiện danh mục và dữ liệu nền phục vụ vận hành, TT12 và quản lý thầu (SAKURA-93234, SAKURA-93256, SAKURA-93257, SAKURA-93258, SAKURA-95842, SAKURA-95862, SAKURA-95954, SAKURA-95955, SAKURA-96113, SAKURA-96116, SAKURA-96120, SAKURA-96221)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Chuẩn hóa dữ liệu danh mục để hỗ trợ vận hành nhiều Cơ sở y tế, tăng khả năng tìm kiếm và phục vụ các đầu ra theo Thông tư 12 cùng dữ liệu thầu. Giá trị mang lại: Giảm thao tác nhập tay, tăng độ đầy đủ dữ liệu và giúp các bộ phận Danh mục, Kho, KHTH đối chiếu thuận tiện hơn.
- Mục đích thay đổi: Chuẩn hóa dữ liệu nền để các danh mục dùng chung, dữ liệu thầu và dữ liệu phục vụ TT12 đồng nhất hơn giữa các phân hệ.
- Nội dung cập nhật:
  + Hiện tại: Nhiều danh mục thuốc, vật tư, dịch vụ kỹ thuật, nhân viên, khoa và dữ liệu thầu còn thiếu trường quản trị, thiếu bộ lọc nâng cao hoặc chưa thuận tiện khi xuất dữ liệu chuẩn.
  + Sau khi cập nhật: Bổ sung thêm các trường phục vụ TT12, trường hiệu lực, kệ lưu trữ, mã liên quan và tìm kiếm nâng cao; đồng thời hoàn thiện màn hình quản lý thầu, chi tiết thầu và các danh mục liên quan.
- Thêm trường:
  + Loại thầu
  + CSKCB chuyển thuốc
  + Từ ngày hiệu lực
  + Đến ngày hiệu lực
  + Kệ lưu trữ
- Tác động tới người dùng: Nhân viên quản trị Danh mục, Kho, KHTH và đội triển khai sẽ thấy thao tác tra cứu, chuẩn hóa và đối chiếu dữ liệu thuận tiện hơn.
- Ảnh hưởng thay đổi: Ảnh hưởng tới màn hình Quản lý thầu, Chi tiết thầu, các danh mục thuốc - vật tư - dịch vụ và các đầu ra phục vụ TT12, BHYT, báo cáo kho.
- Hướng dẫn sử dụng: Người dùng nên rà soát lại bộ lọc, cột hiển thị và dữ liệu khai báo tại các danh mục có liên quan sau cập nhật.
- Thông tin tham chiếu: Loại: Nhóm nhiều ticket | Trạng thái: Tổng hợp | Phân hệ: Danh mục, Quản lý thầu | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

## 2. Tiếp đón và đăng ký khám

### 2.1. Hoàn thiện luồng tiếp đón, tiếp đón cận lâm sàng tương lai và dữ liệu cấp cứu (SAKURA-95900, SAKURA-96387, SAKURA-96410, SAKURA-96426, SAKURA-96573)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Giảm sai sót ngay từ bước tiếp nhận Người bệnh và chỉ định ban đầu, đồng thời đảm bảo dữ liệu đầu vào đủ để các phân hệ sau tiếp tục xử lý. Giá trị mang lại: Hạn chế trùng dịch vụ, thiếu khoa chỉ định hoặc sai loại đối tượng; giúp thông tin cấp cứu và chỉ định đầu vào đầy đủ hơn.
- Mục đích thay đổi: Bảo đảm dữ liệu đầu vào được ghi nhận đầy đủ ngay từ bước tiếp đón, tránh trùng dịch vụ và giảm sai sót khi chuyển sang các bước khám, chỉ định, thu phí tiếp theo.
- Nội dung cập nhật:
  + Hiện tại: Một số luồng tiếp đón cận lâm sàng tương lai có thể thiếu khoa chỉ định, trùng dịch vụ, chưa bắt buộc chọn phòng hoặc lưu loại đối tượng chưa đúng mong muốn vận hành.
  + Sau khi cập nhật: Bổ sung ràng buộc chọn phòng, sửa truyền khoa chỉ định, xử lý trùng dịch vụ, thêm thông tin phương tiện cấp cứu và hoàn thiện logic lưu dữ liệu tại Tiếp đón.
- Thêm trường:
  + Phương tiện cấp cứu
- Tác động tới người dùng: Nhân viên Tiếp đón sẽ thao tác chính xác hơn khi tiếp nhận Người bệnh, đặc biệt ở các luồng có phát sinh chỉ định dịch vụ ngay từ đầu.
- Ảnh hưởng thay đổi: Ảnh hưởng tới màn hình Tiếp đón, luồng tiếp đón cận lâm sàng tương lai, dữ liệu khoa chỉ định và thông tin cấp cứu đi theo hồ sơ tiếp nhận.
- Hướng dẫn sử dụng: Người dùng cần kiểm tra lại các bước chọn phòng và thông tin cấp cứu khi tạo mới hồ sơ hoặc tiếp nhận dịch vụ tương lai.
- Thông tin tham chiếu: Loại: Nhóm nhiều ticket | Trạng thái: Tổng hợp | Phân hệ: Tiếp đón, KIOSK Tiếp đón | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

## 3. Quản lý lịch hẹn

### 3.1. Hiển thị đúng thông tin lịch hẹn và giảm lỗi khi tiếp nhận từ lịch hẹn (SAKURA-96139, SAKURA-96632)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Đảm bảo dữ liệu lời dặn, ghi chú và dịch vụ theo lịch hẹn được dùng lại đúng khi tiếp nhận Người bệnh. Giá trị mang lại: Giúp tổng đài, Tiếp đón và bộ phận nhận lịch hẹn tra cứu rõ hơn và giảm lỗi thao tác khi chuyển từ lịch hẹn sang hồ sơ tiếp nhận.
- Mục đích thay đổi: Đảm bảo thông tin lịch hẹn, ghi chú và lời dặn được dùng lại đúng khi tiếp nhận Người bệnh từ lịch hẹn sang hồ sơ thực tế.
- Nội dung cập nhật:
  + Hiện tại: Thông tin ghi chú, lời dặn có thể lỗi định dạng; một số luồng tiếp nhận cận lâm sàng tương lai từ lịch hẹn có thể sinh trùng dịch vụ.
  + Sau khi cập nhật: Hiển thị đúng nội dung ghi chú, lời dặn và xử lý ổn định hơn việc tiếp nhận dịch vụ theo lịch hẹn trong tương lai.
- Tác động tới người dùng: Nhân viên Tổng đài, Tiếp đón và các bộ phận đặt lịch sẽ theo dõi thông tin hẹn rõ ràng hơn.
- Ảnh hưởng thay đổi: Ảnh hưởng tới màn hình Quản lý lịch hẹn, nội dung ghi chú - lời dặn và luồng tiếp nhận dịch vụ tương lai từ lịch hẹn.
- Hướng dẫn sử dụng: Nên kiểm tra lại lời dặn và danh sách dịch vụ ngay sau khi chọn lịch hẹn để tiếp nhận.
- Thông tin tham chiếu: Loại: Nhóm 2 ticket | Trạng thái: Tổng hợp | Phân hệ: Quản lý lịch hẹn | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

## 4. Khám ngoại trú

### 4.1. Tối ưu thao tác khám, kê đơn, xem kết quả và cập nhật form khám (SAKURA-95921, SAKURA-95818, SAKURA-96083, SAKURA-96091, SAKURA-96093, SAKURA-96112, SAKURA-96206, SAKURA-96303, SAKURA-96366, SAKURA-96430)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Rút ngắn thao tác cho Bác sĩ và nhân viên hỗ trợ khám, đồng thời làm rõ hơn các thông tin chi trả, lý do đến khám và trạng thái theo dõi trên hành trình khám. Giá trị mang lại: Giảm chỉnh tay lặp lại, giúp kê đơn chính xác hơn và hiển thị rõ hơn các thông tin cần quyết định trong quá trình khám.
- Mục đích thay đổi: Giảm thao tác cho Bác sĩ khi khám và kê đơn, đồng thời hiển thị rõ hơn các thông tin chi phí, dữ liệu khám và trạng thái cần theo dõi trong quá trình khám ngoại trú.
- Nội dung cập nhật:
  + Hiện tại: Một số form khám còn thiếu trường; khi sửa số ngày đơn thuốc chưa tự động cập nhật số lượng; popup chỉ định thuốc chưa hiển thị đủ thông tin chi trả; một số trạng thái và thao tác trên màn khám chưa thuận tiện.
  + Sau khi cập nhật: Bổ sung thiết lập tự động cập nhật số lượng thuốc, hiển thị thêm tiền BHYT và phần Người bệnh cần chi trả, cho phép sửa trực tiếp một số trường ngay trên màn hình và hoàn thiện thêm các form khám theo yêu cầu vận hành.
- Thêm trường:
  + Tiền BHYT chi trả
  + Tiền cần trả
  + Tổng tiền
- Tác động tới người dùng: Bác sĩ và nhân viên hỗ trợ khám ngoại trú sẽ thấy thao tác khám, kê đơn và theo dõi kết quả mạch lạc hơn.
- Ảnh hưởng thay đổi: Ảnh hưởng tới màn hình Khám bệnh, popup chỉ định thuốc, logic cập nhật số lượng thuốc theo số ngày cho đơn và một số form khám liên quan.
- Hướng dẫn sử dụng: Người dùng nên kiểm tra lại thiết lập kê đơn, popup chỉ định thuốc và các trường mới trên form khám sau cập nhật.
- Thông tin tham chiếu: Loại: Nhóm nhiều ticket | Trạng thái: Tổng hợp | Phân hệ: Khám bệnh, Chỉ định thuốc | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

## 5. Chỉ định dịch vụ

### 5.1. Hoàn thiện luồng điều chuyển dịch vụ và tự động in phiếu tại Chỉ định dịch vụ (SAKURA-94415, SAKURA-96396, SAKURA-96531)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Hỗ trợ các Cơ sở y tế có nhu cầu điều chuyển nhiều dịch vụ và dùng phiếu in theo nhiều mẫu báo cáo khác nhau tại cùng một luồng thao tác. Giá trị mang lại: Giảm bước xử lý thủ công khi điều chuyển dịch vụ và hạn chế nhầm mẫu phiếu in trong các màn hình có thu tạm ứng đi kèm.
- Mục đích thay đổi: Ổn định luồng điều chuyển dịch vụ và tự động in phiếu để người dùng không phải xử lý tay ở những tình huống có nhiều dịch vụ hoặc nhiều mẫu phiếu.
- Nội dung cập nhật:
  + Hiện tại: Luồng điều chuyển nhiều dịch vụ của nhiều Người bệnh còn hạn chế; màn hình Chỉ định dịch vụ và Thu tạm ứng chưa truyền đủ tham số để lọc đúng mẫu phiếu hoặc tự động in theo nhu cầu.
  + Sau khi cập nhật: Bổ sung khả năng hỗ trợ điều chuyển dịch vụ ở phạm vi rộng hơn và hoàn thiện tham số tự động in, lọc phiếu theo mã báo cáo tại màn Chỉ định dịch vụ và Thu tạm ứng.
- Tác động tới người dùng: Nhân viên thao tác chỉ định và thu tạm ứng sẽ tiết kiệm thời gian hơn ở các tình huống chuyển phòng, đổi mẫu phiếu hoặc in phiếu ngay tại màn hình.
- Ảnh hưởng thay đổi: Ảnh hưởng tới màn hình Chỉ định dịch vụ, luồng điều chuyển dịch vụ, tham số tự động in và cơ chế lọc đúng mẫu phiếu theo mã báo cáo.
- Hướng dẫn sử dụng: Người dùng nên kiểm tra lại mẫu phiếu in và bộ lọc phiếu sau khi cập nhật cấu hình liên quan.
- Thông tin tham chiếu: Loại: Nhóm 3 ticket | Trạng thái: Tổng hợp | Phân hệ: Chỉ định dịch vụ | Phiên bản: Đẩy code 14/05/2026_HIS_SAKURA

## 6. Quản lý nội trú

### 6.1. Nâng cao theo dõi Người bệnh nội trú, cảnh báo xét nghiệm và quản lý thông tin liên kết (SAKURA-92235, SAKURA-93004, SAKURA-93035, SAKURA-96376, SAKURA-96503, SAKURA-95663, SAKURA-95899, SAKURA-96036, SAKURA-96060, SAKURA-96114, SAKURA-96311, SAKURA-96318, SAKURA-96383, SAKURA-96461, SAKURA-96571)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Tăng khả năng theo dõi nhanh tình trạng Người bệnh nội trú, đồng thời làm rõ hơn dữ liệu liên kết hồ sơ và các tình huống đặc thù trong quá trình điều trị. Giá trị mang lại: Giúp Bác sĩ và Điều dưỡng nhận diện nhanh ca cần chú ý, giảm lỗi khi liên kết hồ sơ và làm rõ hơn dữ liệu theo dõi trong nội trú.
- Mục đích thay đổi: Giúp Bác sĩ và Điều dưỡng nhìn nhanh những ca nội trú cần chú ý, đồng thời quản lý đúng dữ liệu liên kết giữa các hồ sơ và các trường theo dõi điều trị.
- Nội dung cập nhật:
  + Hiện tại: Danh sách Người bệnh nội trú chưa có cơ chế đủ rõ để cảnh báo xét nghiệm bất thường; luồng liên kết hồ sơ, theo dõi chuyển dạ, cấp phát thuốc và một số trường hiển thị nội trú còn thiếu hoặc chưa ổn định.
  + Sau khi cập nhật: Bổ sung và chuẩn hóa mức cảnh báo xét nghiệm, icon và bộ lọc; thêm giá trị liên kết Người bệnh, sửa ổn định dữ liệu liên kết, bổ sung cột hiển thị và điều chỉnh thêm các logic theo dõi, chuyển khoa, kê thuốc và tờ điều trị.
- Thêm trường:
  + Cảnh báo xét nghiệm
  + Thông tin Người bệnh liên kết
  + Mối quan hệ liên kết
- Thêm quyền:
  + 2100365 - Hiển thị bộ lọc Kết quả nghiệm bất thường và hiển thị icon cảnh báo kết quả xét nghiệm bất thường
- Tác động tới người dùng: Bác sĩ, Điều dưỡng và bộ phận quản lý nội trú theo dõi danh sách Người bệnh nhanh hơn, rõ rủi ro hơn và giảm lỗi dữ liệu liên kết.
- Ảnh hưởng thay đổi: Ảnh hưởng tới danh sách Người bệnh nội trú, API trả dữ liệu cảnh báo, bộ lọc - icon cảnh báo xét nghiệm, luồng liên kết hồ sơ và một số cột hiển thị trong nội trú.
- Hướng dẫn sử dụng: Nên kiểm tra lại quyền hiển thị cảnh báo, thiết lập liên quan và các trường mới khi theo dõi Người bệnh nội trú.
- Thông tin tham chiếu: Loại: Nhóm nhiều ticket | Trạng thái: Tổng hợp | Phân hệ: Quản lý nội trú, Điều trị nội trú, Nội trú | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

## 7. Quản lý phẫu thuật - thủ thuật

### 7.1. Hoàn thiện thông tin người thực hiện và thao tác tại Phẫu thuật - Thủ thuật (SAKURA-96271, SAKURA-96277, SAKURA-96332, SAKURA-96433, SAKURA-96447)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Đảm bảo màn hình và dữ liệu phục vụ phẫu thuật - thủ thuật phản ánh đúng tổ chức ca thực hiện và giảm thao tác thừa. Giá trị mang lại: Giúp khoa phẫu thuật cập nhật hồ sơ ca mổ rõ hơn và giảm bước xử lý không cần thiết trên giao diện.
- Mục đích thay đổi: Ghi nhận đầy đủ nhân sự thực hiện ca thủ thuật và rút gọn thao tác cập nhật hồ sơ ngay tại màn hình phẫu thuật - thủ thuật.
- Nội dung cập nhật:
  + Hiện tại: Chi tiết Phẫu thuật - Thủ thuật còn thiếu trường người thực hiện, một số enum và thao tác tại tab Thuốc - Vật tư hoặc phòng thực hiện chưa tối ưu.
  + Sau khi cập nhật: Bổ sung thêm trường Thủ thuật viên chính 2, hỗ trợ tùy chỉnh giao diện trường mới, hoàn thiện enum nhóm phẫu thuật, cho phép sửa thông tin phòng thực hiện và tinh gọn thao tác tại tab Thuốc - Vật tư.
- Thêm trường:
  + Thủ thuật viên chính 2
  + Nhóm phẫu thuật
- Tác động tới người dùng: Bác sĩ phẫu thuật, Điều dưỡng phòng mổ và thư ký y khoa thao tác thuận hơn khi cập nhật thông tin ca thủ thuật.
- Ảnh hưởng thay đổi: Ảnh hưởng tới màn hình Chi tiết Phẫu thuật - Thủ thuật, tab Thuốc - Vật tư, thông tin phòng thực hiện và dữ liệu lưu người thực hiện ca thủ thuật.
- Hướng dẫn sử dụng: Người dùng nên kiểm tra lại bố cục trường mới, phân quyền chỉnh sửa và thao tác tại các tab chi tiết ca thủ thuật.
- Thông tin tham chiếu: Loại: Nhóm 5 ticket | Trạng thái: Tổng hợp | Phân hệ: Quản lý PTTT | Phiên bản: Đẩy code 14/05/2026_HIS_SAKURA

## 8. Quản lý kho

### 8.1. Cải thiện quản lý kho, nhà thuốc và duyệt dược lâm sàng (SAKURA-84636, SAKURA-84752, SAKURA-95646, SAKURA-96045, SAKURA-96153, SAKURA-96159, SAKURA-96189, SAKURA-96408, SAKURA-96518, SAKURA-96563, SAKURA-95672)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Hỗ trợ các nghiệp vụ phiếu trả, nhập kho, nhà thuốc và duyệt dược lâm sàng diễn ra đúng logic hơn theo vận hành thực tế. Giá trị mang lại: Giảm sai lệch dữ liệu kho, hạn chế thao tác lặp và giúp các bộ phận Dược, Kho theo dõi chứng từ rõ ràng hơn.
- Mục đích thay đổi: Chuẩn hóa logic kho, nhà thuốc và duyệt dược lâm sàng để giảm sai lệch chứng từ, giảm thao tác lặp và hỗ trợ tốt hơn cho công việc hằng ngày của Dược sĩ và thủ kho.
- Nội dung cập nhật:
  + Hiện tại: Một số nghiệp vụ kho và nhà thuốc còn thiếu thiết lập vận hành, thiếu mã phiếu, thiếu bộ lọc hoặc thao tác in và tìm kiếm chưa thuận tiện.
  + Sau khi cập nhật: Bổ sung thêm thiết lập cho phiếu trả lẻ theo ngày trả, chỉnh nguyên tắc sinh mã đơn thuốc điện tử, hoàn thiện mã phiếu nhập kho, tìm kiếm theo đơn vị tính, duyệt dược lâm sàng và thêm một số tiện ích tại nhà thuốc, nhập kho và báo cáo kho.
- Tác động tới người dùng: Dược sĩ, thủ kho và nhân sự nhà thuốc thao tác nhanh hơn và kiểm soát chứng từ rõ hơn.
- Ảnh hưởng thay đổi: Ảnh hưởng tới thiết lập phiếu trả, màn hình nhập xuất kho, màn hình Nhà thuốc, các điều kiện sinh mã phiếu và luồng duyệt dược lâm sàng.
- Hướng dẫn sử dụng: Người dùng nên rà soát lại thiết lập kho, màn hình nhập xuất và điều kiện duyệt dược lâm sàng sau cập nhật.
- Thông tin tham chiếu: Loại: Nhóm nhiều ticket | Trạng thái: Tổng hợp | Phân hệ: Kho, Nhà thuốc | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

## 9. Ngân hàng máu hoặc tủ máu

### 9.1. Hoàn thiện quy trình dự trù máu, phiếu máu và thao tác theo dõi liên quan (SAKURA-94512, SAKURA-95986, SAKURA-96136, SAKURA-96183, SAKURA-96208, SAKURA-96219, SAKURA-96327)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Làm rõ vòng đời phiếu dự trù máu và giúp người dùng thao tác đầy đủ hơn trong quá trình tạo, in, hoàn thành và theo dõi phiếu. Giá trị mang lại: Giảm thiếu sót khi xử lý phiếu dự trù máu và tăng khả năng kiểm soát trạng thái thực tế.
- Mục đích thay đổi: Làm rõ vòng đời phiếu dự trù máu để người dùng có thể tạo, theo dõi, in và kết thúc phiếu theo đúng trạng thái thực tế.
- Nội dung cập nhật:
  + Hiện tại: Luồng dự trù máu còn thiếu trạng thái kết thúc, thao tác in và giao diện tạo dự trù chưa đầy đủ ở một số nơi sử dụng.
  + Sau khi cập nhật: Bổ sung trạng thái Hoàn thành, nút thao tác liên quan, nút in phiếu, cải thiện modal tạo dự trù và cấu hình thêm cho các phiếu truyền máu liên quan.
- Thêm trường:
  + Trạng thái Hoàn thành
- Tác động tới người dùng: Khoa lâm sàng, kho máu và bộ phận theo dõi truyền máu sẽ quản lý phiếu dự trù rõ ràng hơn.
- Ảnh hưởng thay đổi: Ảnh hưởng tới màn hình Dự trù máu, các nút thao tác hoàn thành - in phiếu, luồng theo dõi sử dụng máu và các chứng từ liên quan.
- Hướng dẫn sử dụng: Người dùng cần kiểm tra lại các trạng thái và nút thao tác mới tại màn hình dự trù máu sau cập nhật.
- Thông tin tham chiếu: Loại: Nhóm nhiều ticket | Trạng thái: Tổng hợp | Phân hệ: Truyền phát máu, Nhập máu | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

## 10. Quyết toán BHYT

### 10.1. Hoàn thiện dữ liệu và điều kiện phục vụ xuất XML, quyết toán và gửi cổng BHYT (SAKURA-95695, SAKURA-93537, SAKURA-95504, SAKURA-95956, SAKURA-95961)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Đảm bảo các dữ liệu danh mục, hồ sơ XML và điều kiện đẩy quyết toán bám sát hơn với yêu cầu của cổng BHYT và cách vận hành từng đơn vị. Giá trị mang lại: Giảm lỗi khi tạo hồ sơ quyết toán và tăng khả năng đối soát dữ liệu danh mục, dịch vụ, Người bệnh.
- Mục đích thay đổi: Giảm lỗi khi tạo và gửi hồ sơ quyết toán, đồng thời giúp bộ phận BHYT có đủ dữ liệu để kiểm tra và đối soát trước khi gửi cổng.
- Nội dung cập nhật:
  + Hiện tại: Dữ liệu XML và điều kiện quyết toán ở một số trường hợp chưa phản ánh đầy đủ thông tin danh mục, phòng khám hoặc giá trị log cần theo dõi.
  + Sau khi cập nhật: Bổ sung trường và logic cho dữ liệu XML, hoàn thiện điều kiện đẩy quyết toán và cập nhật thêm dữ liệu log phục vụ kiểm tra sau gửi cổng BHYT.
- Tác động tới người dùng: Nhân viên BHYT, KHTH và đội hỗ trợ dữ liệu quyết toán sẽ theo dõi, kiểm tra và gửi hồ sơ ổn định hơn.
- Ảnh hưởng thay đổi: Ảnh hưởng tới dữ liệu XML, điều kiện tạo hồ sơ quyết toán, log kiểm tra sau gửi cổng và việc đối soát dữ liệu danh mục - dịch vụ - Người bệnh.
- Hướng dẫn sử dụng: Bộ phận BHYT nên kiểm tra lại cấu hình XML, dữ liệu danh mục và luồng tạo hồ sơ quyết toán sau cập nhật.
- Thông tin tham chiếu: Loại: Nhóm 5 ticket | Trạng thái: Tổng hợp | Phân hệ: XML, Quyết toán | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

## 11. Hồ sơ bệnh án điện tử

### 11.1. Bổ sung và hoàn thiện nhiều Form phiếu, biểu mẫu chuyên khoa và logic lấy dữ liệu hồ sơ (SAKURA-94386, SAKURA-94399, SAKURA-94865, SAKURA-95681, SAKURA-96092, SAKURA-96109, SAKURA-96140, SAKURA-96305, SAKURA-96317, SAKURA-96319, SAKURA-96499, SAKURA-96540, SAKURA-96543, SAKURA-96566, SAKURA-96572, SAKURA-96607, SAKURA-96053, SAKURA-95909)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Đáp ứng nhu cầu ghi chép, in ấn và ký số hồ sơ điện tử tại nhiều chuyên khoa, đồng thời giảm nhập lại dữ liệu từ các phân hệ lâm sàng. Giá trị mang lại: Giúp hồ sơ điện tử đầy đủ hơn, in đúng mẫu hơn và liên kết dữ liệu giữa các phiếu tốt hơn.
- Mục đích thay đổi: Hoàn thiện biểu mẫu hồ sơ điện tử, giảm nhập lại dữ liệu và hỗ trợ in ấn, ký số, liên kết dữ liệu giữa các phiếu sát hơn với nhu cầu thực tế của từng chuyên khoa.
- Nội dung cập nhật:
  + Hiện tại: Nhiều biểu mẫu còn thiếu mẫu mới, thiếu key dữ liệu, thiếu bộ lọc in hoặc chưa hỗ trợ tốt cho trường hợp tạo nhiều phiếu, link phiếu và lấy dữ liệu chuyên khoa.
  + Sau khi cập nhật: Bổ sung thêm nhiều phiếu mới như phiếu theo dõi lọc máu, phiếu chăm sóc, phiếu chuyên khoa; hoàn thiện key dữ liệu, logic tạo nhiều bản ghi, bộ lọc in, tự ngắt trang, hỗ trợ ký số và lấy dữ liệu từ các nguồn liên quan lên biểu mẫu.
- Thêm trường:
  + Người đánh giá
  + Khoa đánh giá
  + Củng mạc mô tả mắt phải
  + Củng mạc mô tả mắt trái
  + ID phiếu ký
- Tác động tới người dùng: Bác sĩ, Điều dưỡng, thư ký y khoa và bộ phận hồ sơ sẽ có thêm biểu mẫu đúng nhu cầu thực tế và giảm thao tác điền lặp dữ liệu.
- Ảnh hưởng thay đổi: Ảnh hưởng tới các form EMR, logic lấy dữ liệu từ hồ sơ điều trị - phiếu liên quan, bộ lọc in, thao tác tạo nhiều bản ghi và luồng ký số trên biểu mẫu.
- Hướng dẫn sử dụng: Người dùng nên kiểm tra lại mã phiếu, bộ lọc in, logic link phiếu và quyền ký số trên các biểu mẫu đang sử dụng.
- Thông tin tham chiếu: Loại: Nhóm nhiều ticket | Trạng thái: Tổng hợp | Phân hệ: Form Phiếu, Hồ sơ bệnh án | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

## 12. Quản lý dinh dưỡng

### 12.1. Hoàn thiện mẫu sàng lọc dinh dưỡng và chọn phiếu đánh giá dinh dưỡng (SAKURA-94763, SAKURA-95618, SAKURA-96207, SAKURA-96407)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Đáp ứng nhu cầu dùng mẫu dinh dưỡng riêng theo từng Cơ sở y tế và giúp người dùng chọn đúng loại phiếu khi đánh giá. Giá trị mang lại: Đảm bảo biểu mẫu dinh dưỡng đúng loại Người bệnh, đúng bối cảnh sử dụng và dễ theo dõi nhiều bản ghi hơn.
- Mục đích thay đổi: Cho phép từng đơn vị dùng đúng mẫu sàng lọc dinh dưỡng theo đối tượng áp dụng và chọn đúng loại phiếu khi đánh giá dinh dưỡng.
- Nội dung cập nhật:
  + Hiện tại: Mẫu sàng lọc dinh dưỡng Nhi nội trú và danh sách chọn phiếu đánh giá chưa đủ để phản ánh đúng nghiệp vụ thực tế tại viện.
  + Sau khi cập nhật: Bổ sung mẫu sàng lọc dinh dưỡng Nhi nội trú mới, hỗ trợ xem nhiều bản ghi trong hồ sơ và hiển thị thêm loại sàng lọc suy dinh dưỡng khi chọn phiếu đánh giá.
- Thêm trường:
  + Loại sàng lọc suy dinh dưỡng
- Tác động tới người dùng: Điều dưỡng và bộ phận Dinh dưỡng sẽ chọn đúng mẫu và theo dõi hồ sơ đánh giá dinh dưỡng thuận tiện hơn.
- Ảnh hưởng thay đổi: Ảnh hưởng tới màn hình Sàng lọc dinh dưỡng, danh sách phiếu đánh giá, điều kiện hiển thị mẫu phiếu và thao tác in - xem lại phiếu.
- Hướng dẫn sử dụng: Người dùng nên kiểm tra lại thiết lập mẫu phiếu và loại sàng lọc trước khi tạo mới hoặc in phiếu.
- Thông tin tham chiếu: Loại: Nhóm 4 ticket | Trạng thái: Tổng hợp | Phân hệ: Quản lý dinh dưỡng | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

## 13. Báo cáo vận hành và báo cáo quản trị

### 13.1. Bổ sung mới nhiều báo cáo vận hành, tài chính, kho và phẫu thuật - thủ thuật (SAKURA-82351, SAKURA-82352, SAKURA-84308, SAKURA-84312, SAKURA-86559, SAKURA-86560, SAKURA-91876, SAKURA-94419, SAKURA-92584, SAKURA-92586, SAKURA-94682, SAKURA-94883, SAKURA-95973, SAKURA-95974, SAKURA-95855, SAKURA-95856, SAKURA-96126, SAKURA-96259, SAKURA-96340)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Mở rộng bộ báo cáo phục vụ quản trị bệnh viện, theo dõi tài chính, kho, phẫu thuật và các nhu cầu kiểm soát dữ liệu đặc thù theo từng Cơ sở y tế. Giá trị mang lại: Giúp các phòng nghiệp vụ có thêm báo cáo phục vụ theo dõi vận hành, đối chiếu và ra quyết định.
- Mục đích thay đổi: Bổ sung các báo cáo mới để các phòng nghiệp vụ có số liệu theo dõi vận hành, tài chính, kho và chuyên môn ngay trên hệ thống.
- Nội dung cập nhật:
  + Hiện tại: Một số báo cáo chưa có trên hệ thống hoặc chưa đủ màn hình, bộ lọc và API để sử dụng trọn vẹn.
  + Sau khi cập nhật: Bổ sung thêm nhiều báo cáo mới như KHTH27.1, KHTH30.1, K82.1, TC28.2, PC10, BC51, K70.1 cùng các báo cáo theo nhu cầu đặc thù về kho, máu, phẫu thuật và quản trị vận hành.
- Thêm quyền:
  + 1500968 - KHTH30.1 Báo cáo chi tiết Tai nạn thương tích
  + 1500345 - K82.1 Báo cáo phiếu xuất theo danh mục
  + 1500970 - KHTH27.1 Báo cáo chi tiết hoạt động khám chữa bệnh ngoại trú
- Tác động tới người dùng: KHTH, Tài chính kế toán, Quản lý kho, Ban giám đốc và các bộ phận điều hành có thêm công cụ báo cáo để theo dõi số liệu.
- Ảnh hưởng thay đổi: Ảnh hưởng tới menu Báo cáo, màn hình lọc báo cáo, tab preview - tải về và các API dữ liệu của những báo cáo mới được bổ sung.
- Hướng dẫn sử dụng: Người dùng nên kiểm tra lại quyền báo cáo, tiêu chí lọc và mẫu báo cáo áp dụng tại đơn vị mình.
- Thông tin tham chiếu: Loại: Nhóm nhiều ticket | Trạng thái: Tổng hợp | Phân hệ: Báo cáo, baocao | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

### 13.2. Điều chỉnh bộ lọc, cách tính và dữ liệu đầu ra cho nhiều báo cáo hiện có (SAKURA-93604, SAKURA-93805, SAKURA-93807, SAKURA-94292, SAKURA-94571, SAKURA-94575, SAKURA-94578, SAKURA-95704, SAKURA-95756, SAKURA-95805, SAKURA-95806, SAKURA-95895, SAKURA-95956, SAKURA-95961, SAKURA-95963, SAKURA-96000, SAKURA-96042, SAKURA-96054, SAKURA-96058, SAKURA-96364, SAKURA-96377, SAKURA-96403, SAKURA-96409, SAKURA-96427, SAKURA-96506, SAKURA-96588, SAKURA-96625)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Làm rõ dữ liệu, bộ lọc và công thức tính trên các báo cáo đang dùng để giảm chênh lệch giữa số liệu hệ thống và số liệu mong muốn của đơn vị vận hành. Giá trị mang lại: Giúp người dùng tin cậy hơn vào số liệu báo cáo và giảm thời gian kiểm tra, đối soát lại bằng tay.
- Mục đích thay đổi: Làm cho số liệu, bộ lọc và công thức của các báo cáo đang dùng sát hơn với nghiệp vụ thực tế, giảm thời gian đối chiếu thủ công.
- Nội dung cập nhật:
  + Hiện tại: Nhiều báo cáo còn vướng về bộ lọc, điều kiện lấy dữ liệu, công thức tính hoặc dữ liệu đầu ra chưa sát với tình huống vận hành thực tế.
  + Sau khi cập nhật: Hoàn thiện thêm bộ lọc, key dữ liệu, cách tính tiền, tiêu chí lấy số liệu và hiệu năng xuất báo cáo ở nhiều báo cáo tài chính, kho, phẫu thuật - thủ thuật, XML và đối soát.
- Tác động tới người dùng: Người dùng thường xuyên khai thác báo cáo sẽ thấy số liệu nhất quán hơn và có thêm tiêu chí lọc sát với nghiệp vụ thực tế.
- Ảnh hưởng thay đổi: Ảnh hưởng tới dữ liệu đầu ra, bộ lọc, công thức tính tiền, điều kiện lấy số liệu và hiệu năng chạy của nhiều báo cáo hiện có.
- Hướng dẫn sử dụng: Nên đối chiếu lại các tiêu chí lọc quen dùng sau cập nhật vì một số báo cáo đã thay đổi cách lấy dữ liệu hoặc thêm điều kiện mới.
- Thông tin tham chiếu: Loại: Nhóm nhiều ticket | Trạng thái: Tổng hợp | Phân hệ: Báo cáo, baocao | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

## 14. Quản trị người dùng và phân quyền

### 14.1. Bổ sung phân quyền đặc thù và tăng kiểm soát bảo mật thông tin nhạy cảm (SAKURA-95761, SAKURA-96399, SAKURA-95764)

- Loại cập nhật: Tính năng mới
- Lý do thực hiện và giá trị mang lại: Tăng khả năng kiểm soát truy cập theo vai trò và đáp ứng yêu cầu bảo mật dữ liệu nhạy cảm tại một số đơn vị. Giá trị mang lại: Giảm rủi ro lộ thông tin và giúp quản trị hệ thống chủ động cấp quyền đúng phạm vi cần thiết.
- Mục đích thay đổi: Kiểm soát chặt hơn việc xem, sửa dữ liệu nhạy cảm và phân quyền thao tác chéo khoa, chéo phạm vi theo đúng vai trò sử dụng.
- Nội dung cập nhật:
  + Hiện tại: Một số quyền mới chưa được gắn lại vào vai trò cũ; một số nghiệp vụ đặc thù chưa có cơ chế kiểm soát đủ chặt với dữ liệu nhạy cảm.
  + Sau khi cập nhật: Bổ sung quyền chỉnh sửa thông tin phòng giường của khoa khác, cập nhật vai trò có quyền cũ sang quyền mới và xây dựng thêm cơ chế bảo mật thông tin cho nhóm hồ sơ nhạy cảm.
- Tác động tới người dùng: Quản trị hệ thống và lãnh đạo khoa có thêm công cụ kiểm soát quyền và dữ liệu chặt chẽ hơn.
- Ảnh hưởng thay đổi: Ảnh hưởng tới cấu hình vai trò, phạm vi truy cập dữ liệu nhạy cảm và quyền chỉnh sửa thông tin phòng giường hoặc dữ liệu đặc thù tại một số đơn vị.
- Hướng dẫn sử dụng: Quản trị hệ thống cần rà soát lại vai trò, quyền mới và phạm vi người dùng được phép truy cập trước khi áp dụng rộng.
- Thông tin tham chiếu: Loại: Nhóm 3 ticket | Trạng thái: Tổng hợp | Phân hệ: PHÂN QUYỀN, Phòng giường nội trú | Phiên bản: Đẩy code 14/05/2026_HIS_SAKURA

## 15. Khác hoặc chưa phân hệ

### 15.1. Hoàn thiện thao tác Thu ngân, hóa đơn điện tử, hóa đơn tài trợ và in chứng từ (SAKURA-95619, SAKURA-96224, SAKURA-96234, SAKURA-96239, SAKURA-96288, SAKURA-96293, SAKURA-96576)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Đáp ứng nhu cầu phát hành, in lại, chuyển đổi và cảnh báo trạng thái hóa đơn theo nhiều mô hình viện và nhiều đối tượng thanh toán. Giá trị mang lại: Giảm thao tác vòng, tăng khả năng tự kiểm soát trạng thái hóa đơn và hỗ trợ tốt hơn cho các luồng thanh toán đặc thù.
- Mục đích thay đổi: Hạn chế sai sót khi in, chuyển đổi và theo dõi trạng thái hóa đơn, đồng thời hỗ trợ tốt hơn cho các mô hình hóa đơn đặc thù tại đơn vị.
- Nội dung cập nhật:
  + Hiện tại: Một số luồng Thu ngân và hóa đơn điện tử còn thiếu quyền thao tác, thiếu nút in hoặc chưa hiển thị rõ trạng thái hóa đơn chuyển đổi, hóa đơn tài trợ.
  + Sau khi cập nhật: Bổ sung quyền in lại hóa đơn chuyển đổi, hiển thị nút in ở các vị trí cần thiết, hỗ trợ cảnh báo hóa đơn tài trợ đã chuyển đổi và hoàn thiện thêm các luồng hóa đơn điện tử cho từng loại hình sử dụng.
- Tác động tới người dùng: Nhân viên Thu ngân và kế toán theo dõi, in và phát hành chứng từ thuận tiện hơn, giảm bỏ sót hóa đơn cần chuyển đổi hoặc cần in lại.
- Ảnh hưởng thay đổi: Ảnh hưởng tới màn hình Thu ngân, Hóa đơn điện tử, luồng in lại chứng từ, cảnh báo hóa đơn tài trợ đã chuyển đổi và các thao tác phát hành hóa đơn.
- Hướng dẫn sử dụng: Quản trị hệ thống nên rà soát lại quyền, mẫu hóa đơn và thiết lập phát hành hóa đơn trước khi dùng chính thức.
- Thông tin tham chiếu: Loại: Nhóm nhiều ticket | Trạng thái: Tổng hợp | Phân hệ: Thu ngân, Hóa đơn điện tử | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

### 15.2. Tối ưu thao tác tạm ứng, thanh toán và xử lý phiếu thu trong vận hành hằng ngày (SAKURA-95915, SAKURA-95988, SAKURA-95989, SAKURA-96107, SAKURA-96429, SAKURA-96574)

- Loại cập nhật: Cải tiến
- Lý do thực hiện và giá trị mang lại: Giảm bước thao tác khi thu tạm ứng, hạn chế luồng thanh toán bị ảnh hưởng bởi tích hợp ngoại vi và làm rõ hơn dữ liệu liên quan tới sinh chi, hoàn tạm ứng. Giá trị mang lại: Giúp nhân viên thao tác nhanh hơn tại quầy và giảm nguy cơ chặn luồng thanh toán do điều kiện kỹ thuật hoặc trạng thái trung gian.
- Mục đích thay đổi: Giảm thao tác tại quầy và tránh gián đoạn thanh toán do thiết bị ngoại vi hoặc logic tạm ứng chưa tối ưu.
- Nội dung cập nhật:
  + Hiện tại: Popup tạm ứng chưa tối ưu vị trí nhập tiền; một số tình huống vân tay, sinh chi hoặc tạm ứng chưa phản ánh đúng mong muốn vận hành tại quầy.
  + Sau khi cập nhật: Tự động trỏ vào ô số tiền khi tạo mới phiếu, điều chỉnh dữ liệu thống kê phiếu tạm ứng, ổn định luồng thanh toán khi không có kết nối vân tay và hoàn thiện thêm xử lý báo cáo, sinh chi theo thời gian thanh toán.
- Tác động tới người dùng: Nhân viên Thu ngân giảm thao tác thừa và hạn chế bị gián đoạn quy trình thanh toán tại quầy.
- Ảnh hưởng thay đổi: Ảnh hưởng tới popup Đề nghị tạm ứng - Thu tạm ứng, luồng sinh chi, xử lý thiết bị vân tay và một số báo cáo, thống kê liên quan tới thanh toán.
- Hướng dẫn sử dụng: Người dùng nên kiểm tra lại thiết bị ngoại vi, trạng thái phiếu tạm ứng và cách phát sinh phiếu sau khi cập nhật.
- Thông tin tham chiếu: Loại: Nhóm nhiều ticket | Trạng thái: Tổng hợp | Phân hệ: Tạm ứng, Thu ngân | Phiên bản: Đẩy code 12/05/2026_HIS_SAKURA, Đẩy code 14/05/2026_HIS_SAKURA

## Người dùng cần lưu ý

- Người dùng nên tải lại trình duyệt sau thời điểm cập nhật để nhận đầy đủ thay đổi về giao diện, nút thao tác và cấu hình mới.
- Với các chức năng có phát sinh quyền mới hoặc phụ thuộc thiết lập, Quản trị hệ thống cần kiểm tra phân quyền và cấu hình trước khi áp dụng chính thức.
- Một số thay đổi chỉ hiển thị khi Cơ sở y tế đang dùng đúng biểu mẫu, báo cáo, quy trình hoặc cấu hình liên quan.

## Giả định và giới hạn dữ liệu

- Tài liệu đã được gom theo nhóm thay đổi để người đọc dễ theo dõi. Với các nhóm có đủ dữ liệu trên JIRA, nội dung đã bổ sung rõ mục đích thay đổi, trường mới, quyền mới và phạm vi ảnh hưởng.
- Metadata version trên JIRA chưa trả rõ release date. Tài liệu đang suy ra mốc 12/05/2026 và 14/05/2026 theo tên version Đẩy code 12/05/2026_HIS_SAKURA và Đẩy code 14/05/2026_HIS_SAKURA.
- Các ticket nội bộ, ticket thuần kỹ thuật hoặc có nội dung nhạy cảm như thay đổi secret key, công cụ build dữ liệu hoặc hỗ trợ kỹ thuật nền đã được loại khỏi thông báo.
- Một số mô tả trên JIRA còn ngắn hoặc thiên về kỹ thuật nên nội dung dưới đây đã được biên tập lại theo góc nhìn người dùng cuối.

## Nhóm ticket tham chiếu

- SAKURA-93234, SAKURA-93256, SAKURA-93257, SAKURA-93258, SAKURA-95842, SAKURA-95862, SAKURA-95954, SAKURA-95955, SAKURA-96113, SAKURA-96116, SAKURA-96120, SAKURA-96221: Hoàn thiện danh mục và dữ liệu nền phục vụ vận hành, TT12 và quản lý thầu - Quản lý danh mục dùng chung | Cải tiến | Tổng hợp
- SAKURA-95900, SAKURA-96387, SAKURA-96410, SAKURA-96426, SAKURA-96573: Hoàn thiện luồng tiếp đón, tiếp đón cận lâm sàng tương lai và dữ liệu cấp cứu - Tiếp đón và đăng ký khám | Cải tiến | Tổng hợp
- SAKURA-96139, SAKURA-96632: Hiển thị đúng thông tin lịch hẹn và giảm lỗi khi tiếp nhận từ lịch hẹn - Quản lý lịch hẹn | Cải tiến | Tổng hợp
- SAKURA-95921, SAKURA-95818, SAKURA-96083, SAKURA-96091, SAKURA-96093, SAKURA-96112, SAKURA-96206, SAKURA-96303, SAKURA-96366, SAKURA-96430: Tối ưu thao tác khám, kê đơn, xem kết quả và cập nhật form khám - Khám ngoại trú | Cải tiến | Tổng hợp
- SAKURA-94415, SAKURA-96396, SAKURA-96531: Hoàn thiện luồng điều chuyển dịch vụ và tự động in phiếu tại Chỉ định dịch vụ - Chỉ định dịch vụ | Cải tiến | Tổng hợp
- SAKURA-92235, SAKURA-93004, SAKURA-93035, SAKURA-96376, SAKURA-96503, SAKURA-95663, SAKURA-95899, SAKURA-96036, SAKURA-96060, SAKURA-96114, SAKURA-96311, SAKURA-96318, SAKURA-96383, SAKURA-96461, SAKURA-96571: Nâng cao theo dõi Người bệnh nội trú, cảnh báo xét nghiệm và quản lý thông tin liên kết - Quản lý nội trú | Cải tiến | Tổng hợp
- SAKURA-94386, SAKURA-94399, SAKURA-94865, SAKURA-95681, SAKURA-96092, SAKURA-96109, SAKURA-96140, SAKURA-96305, SAKURA-96317, SAKURA-96319, SAKURA-96499, SAKURA-96540, SAKURA-96543, SAKURA-96566, SAKURA-96572, SAKURA-96607, SAKURA-96053, SAKURA-95909: Bổ sung và hoàn thiện nhiều Form phiếu, biểu mẫu chuyên khoa và logic lấy dữ liệu hồ sơ - Hồ sơ bệnh án điện tử | Cải tiến | Tổng hợp
- SAKURA-94763, SAKURA-95618, SAKURA-96207, SAKURA-96407: Hoàn thiện mẫu sàng lọc dinh dưỡng và chọn phiếu đánh giá dinh dưỡng - Quản lý dinh dưỡng | Cải tiến | Tổng hợp
- SAKURA-94512, SAKURA-95986, SAKURA-96136, SAKURA-96183, SAKURA-96208, SAKURA-96219, SAKURA-96327: Hoàn thiện quy trình dự trù máu, phiếu máu và thao tác theo dõi liên quan - Ngân hàng máu hoặc tủ máu | Cải tiến | Tổng hợp
- SAKURA-84636, SAKURA-84752, SAKURA-95646, SAKURA-96045, SAKURA-96153, SAKURA-96159, SAKURA-96189, SAKURA-96408, SAKURA-96518, SAKURA-96563, SAKURA-95672: Cải thiện quản lý kho, nhà thuốc và duyệt dược lâm sàng - Quản lý kho | Cải tiến | Tổng hợp
- SAKURA-96271, SAKURA-96277, SAKURA-96332, SAKURA-96433, SAKURA-96447: Hoàn thiện thông tin người thực hiện và thao tác tại Phẫu thuật - Thủ thuật - Quản lý phẫu thuật - thủ thuật | Cải tiến | Tổng hợp
- SAKURA-82351, SAKURA-82352, SAKURA-84308, SAKURA-84312, SAKURA-86559, SAKURA-86560, SAKURA-91876, SAKURA-94419, SAKURA-92584, SAKURA-92586, SAKURA-94682, SAKURA-94883, SAKURA-95973, SAKURA-95974, SAKURA-95855, SAKURA-95856, SAKURA-96126, SAKURA-96259, SAKURA-96340: Bổ sung mới nhiều báo cáo vận hành, tài chính, kho và phẫu thuật - thủ thuật - Báo cáo vận hành và báo cáo quản trị | Tính năng mới | Tổng hợp
- SAKURA-93604, SAKURA-93805, SAKURA-93807, SAKURA-94292, SAKURA-94571, SAKURA-94575, SAKURA-94578, SAKURA-95704, SAKURA-95756, SAKURA-95805, SAKURA-95806, SAKURA-95895, SAKURA-95956, SAKURA-95961, SAKURA-95963, SAKURA-96000, SAKURA-96042, SAKURA-96054, SAKURA-96058, SAKURA-96364, SAKURA-96377, SAKURA-96403, SAKURA-96409, SAKURA-96427, SAKURA-96506, SAKURA-96588, SAKURA-96625: Điều chỉnh bộ lọc, cách tính và dữ liệu đầu ra cho nhiều báo cáo hiện có - Báo cáo vận hành và báo cáo quản trị | Cải tiến | Tổng hợp
- SAKURA-95695, SAKURA-93537, SAKURA-95504, SAKURA-95956, SAKURA-95961: Hoàn thiện dữ liệu và điều kiện phục vụ xuất XML, quyết toán và gửi cổng BHYT - Quyết toán BHYT | Cải tiến | Tổng hợp
- SAKURA-95619, SAKURA-96224, SAKURA-96234, SAKURA-96239, SAKURA-96288, SAKURA-96293, SAKURA-96576: Hoàn thiện thao tác Thu ngân, hóa đơn điện tử, hóa đơn tài trợ và in chứng từ - Khác hoặc chưa phân hệ | Cải tiến | Tổng hợp
- SAKURA-95915, SAKURA-95988, SAKURA-95989, SAKURA-96107, SAKURA-96429, SAKURA-96574: Tối ưu thao tác tạm ứng, thanh toán và xử lý phiếu thu trong vận hành hằng ngày - Khác hoặc chưa phân hệ | Cải tiến | Tổng hợp
- SAKURA-95761, SAKURA-96399, SAKURA-95764: Bổ sung phân quyền đặc thù và tăng kiểm soát bảo mật thông tin nhạy cảm - Quản trị người dùng và phân quyền | Tính năng mới | Tổng hợp

Nếu cần đối chiếu sâu theo từng ticket, vui lòng phản hồi lại đội triển khai hoặc đội sản phẩm để được cung cấp danh sách chi tiết theo từng version.
