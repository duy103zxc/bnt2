# @Component và @Autowired

[Đường dẫn bài viết](https://viblo.asia/p/spring-boot-1-huong-dan-atcomponent-va-atautowired-E375zXvJZGW)

Thường thì giờ đã có [Spring Initializr](https://start.spring.io/) rồi nên thêm dự án mới rất dễ!

### Việc chạy ứng dụng Spring Boot

Chương trình chạy ở hàm `main()` và cần thêm annotation `@SpringBootApplication` trên class chính và gọi `SpringApplication.run(App.class, args);` để chạy project (Cho Spring Boot biết `main()` ở chỗ nào). `SpringApplication.run(App.class, args)` chính là câu lệnh để tạo ra _container_. Sau đó nó tìm toàn bộ các _dependency_ trong project của bạn và đưa vào đó.


```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class App {
    public static void main(String[] args) {
        // ApplicationContext chứa toàn bộ dependency trong project.
        SpringApplication.run(App.class, args);
    }
}
```

Spring đặt tên cho:

- _container_ là `ApplicationContext`
- _dependency_ là `Bean` (Để biết cái nào là *dependency* thì dùng `@Component`)

### @Component

@Component là một Annotation (chú thích) đánh dấu trên các Class để giúp Spring biết nó là một Bean. 

Trong quá trình dò các *classes*, khi gặp một class được đánh dấu `@Component` thì nó sẽ tạo ra một instance và đưa vào `ApplicationContext` để quản lý.

```java
// ApplicationContext chính là container, chứa toàn bộ các Bean
ApplicationContext context = SpringApplication.run(App.class, args);
// Khi chạy xong, lúc này context sẽ chứa các Bean có đánh
// dấu @Component.
// Lấy Bean ra bằng cách
Outfit outfit = context.getBean(Outfit.class);
// In ra để xem thử nó là gì
System.out.println("Instance: " + outfit);
// xài hàm wear()
outfit.wear();
// Output
// [1] Instance: me.loda.spring.helloworld.Bikini@1e1f6d9d
// [2] Mặc bikini
```

Danh sách các bước đơn giản là:

1. Chạy `SpringApplication.run(App.class, args);`
2. Dò tìm các Classes cùng cấp.
3. Thấy Class được đánh dấu bằng `@Component` thì tạo `new Customer()` chẳng hạn và đưa vào Context (Hay `ApplicationContext`)

### @Autowired

```
@Component
public class TuiXach {

    @Autowired
    Money tien;

    public TuiXach (Money tien) {
        this.tien = tien;
    }
}
```

Tôi đánh dấu thuộc tính `Money` (Tiền) của `TuiXach` (Túi xách) bởi Annotation `@Autowired`. Điều này nói với Spring Boot hãy tự _inject_ (tiêm) một instance của `Money` vào thuộc tính này khi khởi tạo `TuiXach` (Thuộc tính được đánh dấu là `@Autowired` trong Class được đánh dấu là `Component`).


### Singleton

Điều đặc biệt là các `Bean` được quản lý bên trong `ApplicationContext` đều là singleton.

Tất cả những `Bean` được quản lý trong `ApplicationContext` đều chỉ được tạo ra một lần duy nhất và khi có `Class` yêu cầu `@Autowired` thì nó sẽ lấy đối tượng có sẵn trong `ApplicationContext` để _inject_ vào.

Trong trường hợp bạn muốn mỗi lần sử dụng là một instance hoàn toàn mới. Thì hãy đánh dấu `@Component` đó bằng `@Scope("prototype")`

### Inject là cái quái gì vậy?
Đây không phải là phần hướng dẫn của loda, nhưng mình vẫn chưa hiểu *inject* (về cơ bản nghĩa là tiêm) là gì? Thử lấy ví dụ về truyền dịch.

*Truyền nước biển còn gọi là truyền dịch là phương pháp đưa nhỏ giọt muối và các chất điện giải vào cơ thể bằng đường tĩnh mạch khi có chỉ định của bác sĩ.*

Khi đó thành các *giọt muối và các chất điện giải* sẽ được *inject* và trở thành *thuộc tính* trong bạn. Mỗi một Class trong Java cũng như bạn, là một *đối tượng* (dm giải thích ngu quá mà tính lấy ví dụ Peter Park được nhện chích và thành Spider Man thì có phải hay không???)

*Dependency Injection* cho phép "inject" (tiêm) những dependencies vào một đối tượng. Thay vì đối tượng phải tự tạo ra những Dependencies của chính nó. Điều này sẽ giúp giảm khả năng phụ thuộc giữa những thành phần. 