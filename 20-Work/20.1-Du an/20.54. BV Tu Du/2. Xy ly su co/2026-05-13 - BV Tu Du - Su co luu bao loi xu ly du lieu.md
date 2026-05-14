# Work Note - Sự cố lưu báo lỗi xử lý dữ liệu

## Mục tiêu

- Ghi nhận diễn biến sự cố lưu dữ liệu báo lỗi `Lỗi xử lý dữ liệu` tại Bệnh viện Từ Dũ.
- Lưu lại mốc thời gian phối hợp giữa Triển khai, DEV và System để phục vụ truy vết.

## Bối cảnh

- Tối 12/05/2026, Hà Nguyễn thấy hiện tượng bấm `Lưu` báo `Lỗi xử lý dữ liệu`, nhưng nhấn `F5` rồi thao tác lại thì không còn lỗi.
- Sáng 13/05/2026, nhiều Người dùng tiếp tục báo lỗi lưu dữ liệu.
- DEV cung cấp log cho thấy có lỗi kết nối Rabbit tại thời điểm phát sinh.
- Chiều 13/05/2026 sự cố tái xuất hiện, sau đó xác định có liên quan lỗi Rabbit trên `App2`.
- Sáng 14/05/2026 lỗi tiếp tục tái xuất hiện, System đang xử lý và chưa phản hồi nguyên nhân cụ thể.

## Phạm vi

- Bao gồm:

    - Ghi nhận diễn biến sự cố từ tối 12/05/2026 đến 11h47 ngày 14/05/2026.
    - Ghi nhận hướng xử lý tạm thời và phối hợp các bên liên quan.
- Không bao gồm:

    - Kết luận nguyên nhân gốc cuối cùng.
    - Biên bản RCA hoặc kế hoạch phòng ngừa tái diễn.

## Trạng thái

- Trạng thái hiện tại: Lỗi tiếp tục tái xuất hiện lúc 11h47 ngày 14/05/2026. System đã nhận thông tin và đang xử lý, nhưng chưa phản hồi nguyên nhân cụ thể.
- Người liên quan:

    - Hà Nguyễn
    - Team Triển khai
    - Hữu Ánh DEV
    - Anh Hậu System
- Mốc thời gian:

    - 12/05/2026 buổi tối: Ghi nhận lỗi rải rác.
    - 13/05/2026 06h00 - 07h00: Xảy ra lỗi trên nhiều máy và thực hiện xử lý.
    - 13/05/2026 17h04 - 17h15: Lỗi tái xuất hiện và được xử lý tạm thời.
    - 14/05/2026 11h47: Team Triển khai tiếp tục báo lỗi, System đang xử lý.
    - 14/05/2026 12h43: Chính System báo lỗi đã được xử lý triệt để, không bị lại.

## Khoanh vùng ảnh hưởng

- Ảnh hưởng tới NSD:

    - NSD thao tác không được, phải nhấn `F5` rồi thực hiện lại.
    - Trong thời điểm Người bệnh đông, thao tác lặp lại làm tăng nguy cơ ùn tắc tại các điểm sử dụng hệ thống.
- Ảnh hưởng nghiêm trọng đến luồng kết quả cận lâm sàng:

    - Có thời điểm `LIS`, `PACS` đẩy kết quả về bị lỗi.
    - Người bệnh quay lại Bác sĩ đọc kết quả nhưng trên HIS không có kết quả, dù thực tế đã có kết quả trên `LIS`, `PACS`.
    - Bác sĩ không thể kết luận khám ngay cho Người bệnh.
    - Nguy cơ gây ùn tắc chuỗi khám, chờ đọc kết quả và chốt khám.

## Nội dung chính

- Tối 12/05/2026:

    - Hà Nguyễn thấy hiện tượng bấm `Lưu` báo `Lỗi xử lý dữ liệu`.
    - Sau khi nhấn `F5` rồi thao tác lại thì không còn lỗi.
- 13/05/2026 06h00:

    - Hà Nguyễn thấy có quản lý yêu cầu Người dùng báo lỗi.
- 13/05/2026 06h15:

    - Bắt đầu ghi nhận một số máy khi bấm `Lưu` bị báo lỗi `Lỗi xử lý dữ liệu`.
    - Hà Nguyễn hướng dẫn team Triển khai tạm thời hướng dẫn Người dùng nhấn `F5` rồi thao tác lại.
- 13/05/2026 06h17:

    - Hà Nguyễn báo System và DEV kiểm tra.
- 13/05/2026 06h25:

    - Hữu Ánh DEV gửi log thời điểm lỗi, trong đó có báo lỗi Rabbit với thông tin `Connection refused`.
    - Hà Nguyễn gọi anh Hậu System để kiểm tra.
    - Sau thời điểm này, tiếp tục có thêm Người dùng báo lỗi.
- Log DEV cung cấp:

```text
2026-05-13 06:01:33.463 DEBUG [his-service,6a03b14d8b18c4b68bed50b761a65bd8,c348a1f8bbcb335b] 14 --- [o-2329-exec-116] v.i.c.c.WebRestControllerAdvice          : Response: {"data":null,"code":500,"message":"Lỗi xử lý dữ liệu!","trace":"BVTUDUCS1-APP02:2329-6a03b14d8b18c4b68bed50b761a65bd8-2026-05-13 06:01:33"}, (Connection refused)
2026-05-13 06:01:33.458 ERROR [his-service,6a03b14d8b18c4b68bed50b761a65bd8,c348a1f8bbcb335b] 14 --- [o-2329-exec-116] v.i.c.c.WebRestControllerAdvice          : java.net.ConnectException: Connection refused

org.springframework.amqp.AmqpConnectException: java.net.ConnectException: Connection refused
        at org.springframework.amqp.rabbit.support.RabbitExceptionTranslator.convertRabbitAccessException(RabbitExceptionTranslator.java:61)
        at org.springframework.amqp.rabbit.connection.AbstractConnectionFactory.createBareConnection(AbstractConnectionFactory.java:622)
        at org.springframework.amqp.rabbit.connection.CachingConnectionFactory.createConnection(CachingConnectionFactory.java:726)
        at org.springframework.amqp.rabbit.connection.ConnectionFactoryUtils.createConnection(ConnectionFactoryUtils.java:257)
        at org.springframework.amqp.rabbit.core.RabbitTemplate.doExecute(RabbitTemplate.java:2249)
        at org.springframework.amqp.rabbit.core.RabbitTemplate.execute(RabbitTemplate.java:2222)
        at org.springframework.amqp.rabbit.core.RabbitTemplate.send(RabbitTemplate.java:1122)
        at org.springframework.amqp.rabbit.core.RabbitTemplate.convertAndSend(RabbitTemplate.java:1234)
        at org.springframework.amqp.rabbit.core.RabbitTemplate.convertAndSend(RabbitTemplate.java:1223)
        at vn.isofh.common.client.RabbitServiceImpl.convertAndSend(RabbitServiceImpl.java:53)
        at vn.isofh.his.service.NbDotDieuTriServiceImpl.save(NbDotDieuTriServiceImpl.java:8947)
        at vn.isofh.his.service.NbDotDieuTriServiceImpl.save(NbDotDieuTriServiceImpl.java:258)
        at vn.isofh.common.service.AbstractBaseService.save(AbstractBaseService.java:259)
        at vn.isofh.his.service.base.AbstractService.save(AbstractService.java:22)
```

- 13/05/2026 06h40:

    - Hà Nguyễn gọi nhắc lại System.
    - Đưa hướng xử lý tạm thời: nếu còn ảnh hưởng thì bỏ `App2` ra khỏi `HA`, xử lý xong thì cho vào lại.
- 13/05/2026 06h57:

    - Anh Hậu System báo đã cho `App2` vào lại `HA`.
- 13/05/2026 07h00:

    - Team theo dõi không thấy phát sinh thêm.
    - Hoàn thành xử lý ban đầu.
- 13/05/2026 17h04:

    - Team Triển khai tiếp tục báo lỗi.
    - Nội dung lỗi ghi nhận:

```json
{
  "data": null,
  "code": 500,
  "message": "Lỗi xử lý dữ liệu!",
  "trace": "BVTUDUCS1-APP02:2329-6a044cd92ca8aa740671a18e01a213ba-2026-05-13 17:05:13"
}
```

- 13/05/2026 17h12:

    - Lỗi tái xuất hiện.
    - Nguyễn Hữu Ánh DEV gửi thông tin lỗi cho anh Hậu System.
- 13/05/2026 17h15:

    - Anh Hậu xử lý và báo kiểm tra lại thì không còn lỗi.
- Cập nhật hiện trạng sau xử lý:

    - Đã phát hiện lỗi có liên quan Rabbit trên `App2`.
    - System chưa báo nguyên nhân cụ thể.
    - System chưa xác nhận đã xử lý triệt để hay chưa.
    - Chính, Trưởng nhóm System, giao cho Nguyễn Lâm Bảo Di cập nhật thêm config cho container.
    - Đến thời điểm cập nhật note, chưa có thông tin báo đã hoàn tất việc này.
- 14/05/2026 11h47:

    - Team Triển khai tiếp tục báo lỗi.
    - Nội dung lỗi ghi nhận:

```json
{
  "data": null,
  "code": 500,
  "message": "Lỗi xử lý dữ liệu!",
  "trace": "BVTUDUCS1-APP02:2329-6a05536c7faac6591a583e8565b0e9dc-2026-05-14 11:45:32"
}
```

    - Hữu Ánh DEV cung cấp log:

```text
2026-05-14 11:45:32.926 ERROR [his-service,6a05536c7faac6591a583e8565b0e9dc,c2284a446cea1121] 14 --- [io-2329-exec-37] v.i.c.c.WebRestControllerAdvice          : com.rabbitmq.client.AuthenticationFailureException: ACCESS_REFUSED - Login was refused using authentication mechanism PLAIN. For details see the broker logfile.

org.springframework.amqp.AmqpAuthenticationException: com.rabbitmq.client.AuthenticationFailureException: ACCESS_REFUSED - Login was refused using authentication mechanism PLAIN. For details see the broker logfile.
        at org.springframework.amqp.rabbit.support.RabbitExceptionTranslator.convertRabbitAccessException(RabbitExceptionTranslator.java:64)
        at org.springframework.amqp.rabbit.connection.AbstractConnectionFactory.createBareConnection(AbstractConnectionFactory.java:622)
        at org.springframework.amqp.rabbit.connection.CachingConnectionFactory.createConnection(CachingConnectionFactory.java:726)
        at org.springframework.amqp.rabbit.connection.ConnectionFactoryUtils.createConnection(ConnectionFactoryUtils.java:257)
        at org.springframework.amqp.rabbit.core.RabbitTemplate.doExecute(RabbitTemplate.java:2249)
        at org.springframework.amqp.rabbit.core.RabbitTemplate.execute(RabbitTemplate.java:2222)
        at org.springframework.amqp.rabbit.core.RabbitTemplate.send(RabbitTemplate.java:1122)
        at org.springframework.amqp.rabbit.core.RabbitTemplate.convertAndSend(RabbitTemplate.java:1234)
        at org.springframework.amqp.rabbit.core.RabbitTemplate.convertAndSend(RabbitTemplate.java:1223)
        at vn.isofh.common.client.RabbitServiceImpl.convertAndSend(RabbitServiceImpl.java:53)
        at vn.isofh.his.service.NbDvKhamServiceImpl.save(NbDvKhamServiceImpl.java:430)
        at vn.isofh.his.service.NbDvKhamServiceImpl.save(NbDvKhamServiceImpl.java:204)
        at vn.isofh.common.service.AbstractBaseService.lambda$save$4(AbstractBaseService.java:277)
        at vn.isofh.common.service.TransactionService.execute(TransactionService.java:25)
        at java.base/jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:103)
        at java.base/java.lang.reflect.Method.invoke(Method.java:580)
        at org.springframework.aop.support.AopUtils.invokeJoinpointUsingReflection(AopUtils.java:354)
```

    - Đã báo System xử lý.
    - Đến thời điểm cập nhật note, System đang xử lý và chưa rõ nguyên nhân.
- 14/05/2026 11h53:

    - Chính System báo đã xử lý.
    - Team Triển khai kiểm tra và tiếp tục theo dõi.
- 14/05/2026 12h43:

    - Chính System báo lại lỗi này đã được xử lý triệt để.
    - Theo thông tin System phản hồi, lỗi không bị lại.

## Việc cần làm

- [ ] System làm rõ nguyên nhân cụ thể gây lỗi Rabbit trên `App2`.
- [ ] System làm rõ nguyên nhân lỗi xác thực Rabbit `ACCESS_REFUSED` ghi nhận lúc 11h45 ngày 14/05/2026.
- [ ] System cập nhật kết quả thêm config cho container do Nguyễn Lâm Bảo Di đang thực hiện.
- [ ] System xác nhận đã xử lý triệt để hay mới chỉ xử lý tạm thời.
- [ ] Xác nhận có hay không trường hợp lưu thất bại nhưng dữ liệu đã ghi một phần.
- [ ] Chốt nguyên nhân gốc và biện pháp phòng ngừa tái diễn.

## Business rules hoặc lưu ý thực thi

- Khi Người dùng gặp lỗi trong thời gian chưa xử lý dứt điểm, hướng dẫn tạm thời là nhấn `F5` rồi thao tác lại.
- Đã có căn cứ bước đầu cho thấy lỗi liên quan Rabbit trên `App2`.
- Chưa đủ căn cứ để kết luận nguyên nhân gốc cuối cùng chỉ từ log ứng dụng và thông tin vận hành hiện có.
- Cần đối chiếu thêm log hạ tầng, cấu hình container, thông tin tài khoản kết nối Rabbit và trạng thái Rabbit trước khi chốt nguyên nhân chính thức.

## Rủi ro và điểm cần làm rõ

- Rủi ro:
  - Nếu lỗi kết nối Rabbit tái diễn, thao tác lưu dữ liệu có thể tiếp tục bị gián đoạn trên nhiều máy.
  - Nếu lỗi xác thực Rabbit chưa được xử lý dứt điểm, thao tác lưu có thể tiếp tục lỗi trên `App2`.
  - Nếu luồng trả kết quả từ `LIS`, `PACS` tiếp tục lỗi, Bác sĩ có thể không đọc được kết quả trên HIS dù hệ thống cận lâm sàng đã có kết quả.
  - Nếu Người bệnh không được kết luận khám đúng thời điểm, nguy cơ ùn tắc tại khu khám và khu chờ đọc kết quả tăng cao.
  - Nếu có trường hợp lưu một phần, có thể phát sinh lệch dữ liệu cần đối soát.
  - Nếu cấu hình container chưa cập nhật xong hoặc cập nhật chưa đúng, lỗi có thể tái diễn.
- Open question:
  - Nguyên nhân trực tiếp gây `Connection refused` ngày 13/05/2026 và `ACCESS_REFUSED` ngày 14/05/2026 trên `App2` là từ Rabbit, cấu hình container, tài khoản kết nối hay `HA`?
  - Có cần bổ sung cơ chế cảnh báo sớm khi app mất kết nối Rabbit không?
  - Khi nào System hoàn tất phần cập nhật config cho container và xác nhận kết quả cuối cùng?

## Tài liệu và note liên quan

- [[2026-05-13 - BV Tu Du - Su co luu bao loi xu ly du lieu]]

## Nhật ký cập nhật

- 2026-05-14: Tạo note sự cố từ thông tin Hà Nguyễn cung cấp.
- 2026-05-14: Cập nhật thêm diễn biến tái lỗi lúc 17h04 - 17h15 ngày 13/05/2026 và trạng thái xử lý từ System.
- 2026-05-14: Cập nhật thêm diễn biến lỗi lúc 11h47 ngày 14/05/2026, log `ACCESS_REFUSED` và trạng thái System đang xử lý.
- 2026-05-14: Cập nhật thêm mốc 11h53 ngày 14/05/2026 khi System báo đã xử lý và Team Triển khai tiếp tục theo dõi.
- 2026-05-14: Cập nhật thêm mốc 12h43 ngày 14/05/2026 khi Chính System xác nhận lỗi đã được xử lý triệt để và không bị lại.
