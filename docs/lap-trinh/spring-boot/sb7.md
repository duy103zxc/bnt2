# Application Config và @Value

Trong thực tế không phải lúc nào chúng ta cũng nên để mọi thứ trong code của mình. Có những thông số tốt hơn hết nên được truyền từ bên ngoài vào ứng dụng, để giúp ứng dụng của bạn dễ dàng thay đổi giữa các môi trường khác nhau. Để phục vụ điều này, chúng ta sẽ tìm hiểu về khái niệm config ứng dụng Spring Boot với `application.properties`


### application.properties

Trong Spring Boot, các thông tin cấu hình mặc định được lấy từ file `resources/applications.properties`. Ví dụ, bạn muốn Spring Boot chạy trên port 8081 thay vì 8080 thì `server.port = 8081`

Hoặc bạn muốn log của chương trình chi tiết hơn. Hãy chuyển nó sang dậng Debug bằng cách config như sau: `logging.level.root=DEBUG`

### @Value

Trong trường hợp, bạn muốn tự config những giá trị của riêng mình, thì Spring Boot hỗ trợ bạn với annotation `@Value`

Ví dụ, tôi muốn cấu hình cho thông tin database của tôi từ bên ngoài ứng dụng

_application.properties_

```
loda.mysql.url=jdbc:mysql://host1:33060/loda
```

`@Value` được sử dụng trên thuộc tính của class, Có nhiệm vụ lấy thông tin từ file properties và gán vào biến.

```
public class AppConfig {
    // Lấy giá trị config từ file application.properties
    @Value("${loda.mysql.url}")
    String mysqlUrl;
}
```

Thông tin truyền vào annottaion `@Value` chính là tên của cấu hình đặt trong dấu `${name}`

### Ví dụ và đọc thêm
[Phần ví dụ đầy đủ cho bài viết này](https://viblo.asia/p/spring-boot-7-spring-boot-application-config-va-atvalue-RQqKLwr657z#_vi-du-4)

Sau bài này bạn có thể xem thêm nội dung sau:

1. [Hướng dẫn sử dụng Spring Properties với `@ConfigurationProperties`](https://www.baeldung.com/configuration-properties-in-spring-boot)
2. Hướng dẫn sử dụng Spring Profiles