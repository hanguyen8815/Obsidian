# Báo cáo sự cố "Lỗi xử lý dữ liệu" liên quan đến Rabbit trên App2

## 1. Thông tin chung

- Đơn vị: Bệnh viện Từ Dũ
- Nội dung sự cố: Một số thao tác `Lưu` trên HIS báo `Lỗi xử lý dữ liệu`
- Thời gian ghi nhận đầu tiên: Tối 12/05/2026
- Thời gian tái diễn nhiều lần: Ngày 13/05/2026 và sáng 14/05/2026
- Thời gian System xác nhận xử lý triệt để: 12h43 ngày 14/05/2026
- Trạng thái hiện tại: Đã xử lý triệt để theo xác nhận của Chính System, chưa ghi nhận lỗi lặp lại

## 2. Tóm tắt điều hành

- Trong thời gian từ tối 12/05/2026 đến trưa 14/05/2026, hệ thống ghi nhận hiện tượng một số người dùng thao tác `Lưu` bị báo `Lỗi xử lý dữ liệu`.
- Lỗi tái diễn nhiều đợt, có lúc người dùng phải nhấn `F5` rồi thao tác lại mới thực hiện được.
- Mức ảnh hưởng nghiêm trọng nhất là NSD thao tác không được, phải nhấn `F5` để thao tác lại, làm tăng nguy cơ mất dữ liệu đã nhập, phát sinh thao tác nhiều lần, gây chậm trễ và mất thời gian xử lý.
- Mức ảnh hưởng nghiêm trọng nhất là luồng nhận kết quả từ `LIS`, `PACS` về HIS có thời điểm bị lỗi, dẫn tới Bác sĩ không đọc được kết quả trên HIS dù thực tế đã có kết quả trên hệ thống cận lâm sàng.
- DEV đã cung cấp log cho thấy lỗi liên quan Rabbit trên `App2`, gồm các dạng `Connection refused` và `ACCESS_REFUSED`.
- Team System đã phối hợp xử lý nhiều đợt. Đến 12h43 ngày 14/05/2026, Chính System báo đã xử lý triệt để và không bị lại.

## 3. Mức ảnh hưởng

- Ảnh hưởng tới NSD:
  - NSD có thời điểm thao tác không được, phải nhấn `F5` rồi thực hiện lại.
  - Có nguy cơ mất dữ liệu đã nhập nếu NSD chưa kịp ghi nhận lại đầy đủ sau khi lỗi phát sinh.
  - Việc phải thao tác nhiều lần làm chậm trễ xử lý, mất thời gian và tăng áp lực vận hành.
  - Trong thời điểm Người bệnh đông, thao tác lặp lại làm tăng nguy cơ ùn tắc tại các điểm sử dụng hệ thống.
- Ảnh hưởng tới luồng trả kết quả cận lâm sàng:
  - Có thời điểm `LIS`, `PACS` đẩy kết quả về HIS bị lỗi.
  - Người bệnh quay lại để Bác sĩ đọc kết quả nhưng trên HIS không có kết quả, dù thực tế đã có kết quả trên `LIS`, `PACS`.
  - Bác sĩ không thể kết luận khám ngay cho Người bệnh.
  - Nguy cơ gây ùn tắc chuỗi khám, chờ đọc kết quả và chốt khám.

## 4. Nội dung chính

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

## 5. Nhận định kỹ thuật hiện tại

- DEV đã khoanh vùng lỗi liên quan Rabbit trên `App2`.
- Các log chính ghi nhận 2 dạng lỗi:
  - `Connection refused`
  - `ACCESS_REFUSED`
- Team System đã thực hiện xử lý nhiều đợt và xác nhận đã xử lý triệt để lúc 12h43 ngày 14/05/2026.
- Tại thời điểm lập báo cáo này, chưa ghi nhận lỗi lặp lại sau xác nhận của System.

## 6. Đánh giá rủi ro vận hành

- Nếu lỗi tái diễn, mức ảnh hưởng không chỉ dừng ở thao tác lưu chậm mà còn có thể ảnh hưởng trực tiếp đến luồng trả kết quả cận lâm sàng.
- Tình huống Người bệnh đã có kết quả trên `LIS`, `PACS` nhưng HIS chưa nhận được là rủi ro vận hành cao, vì làm chậm kết luận khám và tăng nguy cơ ùn tắc.
- Trong khung giờ đông Người bệnh, lỗi kiểu này có thể lan rộng thành ảnh hưởng dây chuyền tới tiếp đón, khám, đọc kết quả và chốt khám.

## 7. Kiến nghị sau xử lý

- System gửi xác nhận chính thức nguyên nhân gốc và nội dung đã xử lý triệt để để các team cùng nắm.
- System rà lại cơ chế cảnh báo sớm khi app mất kết nối hoặc lỗi xác thực Rabbit.
- Theo dõi thêm sau xử lý để xác nhận hệ thống đã ổn định hoàn toàn trong vận hành thực tế.
- Thực hiện xử lý và hậu kiểm theo đúng quy trình xử lý sự cố của công ty.

## 8. Kết luận

- Sự cố đã ảnh hưởng trực tiếp đến thao tác của NSD và có thời điểm ảnh hưởng tới luồng trả kết quả cận lâm sàng.
- Team Triển khai, DEV và System đã phối hợp xử lý liên tục trong nhiều mốc thời gian.
- Theo xác nhận của Chính System lúc 12h43 ngày 14/05/2026, lỗi đã được xử lý triệt để và không bị lại.
- Đề nghị các team tiếp tục theo dõi sau xử lý và chốt lại nguyên nhân gốc bằng thông tin chính thức từ System.
