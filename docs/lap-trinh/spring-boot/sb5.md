# Component Scan là gì?

Chúng ta sẽ tìm hiểu thêm về cách Spring Boot tìm kiếm `Bean` trong project của bạn như thế nào.


### Component Scan
Trong bài 1 tôi có đề cập một lần về việc Spring Boot khi chạy sẽ dò tìm toàn bộ các _Class cùng cấp_ hoặc ở trong các _package thấp hơn_ và tạo ra Bean từ các Class tìm thấy.

Trong trường hợp bạn muốn tuỳ chỉnh cấu hình cho Spring Boot chỉ tìm kiếm các bean trong một package nhất định thì có các cách sau đây:

1. Sử dụng `@ComponentScan`
2. Sử dụng `scanBasePackages` tromg `@SpringBootApplication`.

Bạn có thể cấu hình cho Spring Boot Tìm kiếm các Bean ở nhiều package khác nhau bằng cách

### Cách 1:`@ComponentScan`

```java
@ComponentScan("me.loda.spring.componentscan.others")
// Tìm nhiều package khác nhau
// @ComponentScan({"me.loda.spring.componentscan.others2","me.loda.spring.componentscan.others"})

@SpringBootApplication
public class App {
    ...
}
```

### Cách 2:`scanBasePackages`

```java
@SpringBootApplication(scanBasePackages = "me.loda.spring.componentscan.others")
// Tìm nhiều package khác nhau
// @SpringBootApplication(scanBasePackages = {"me.loda.spring.componentscan.others", "me.loda.spring.componentscan.others2"})
public class App {
  ...
}
```

Cả 2 cách đều cho kết quả in ra màn hình như sau:

```
Bean Girl không tồn tại
Bean: OtherGirl.java
```

Lúc này, Spring Boot chỉ tìm kiếm các bean trong package `others` mà thôi. Nên khi lấy ra `Girl` thì nó không tồn tại trong `Context`.
