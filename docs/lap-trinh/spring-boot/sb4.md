# @Component vs @Service vs @Repository

### Kiến trúc trong Spring Boot

Kiến trúc MVC trong Spring Boot được xây dựng dựa trên tư tưởng "độc lập" kết hợp với các nguyên lý thiết kế hướng đối tượng (một đại diện tiêu biểu là Dependency Inversion). 

Độc lập ở đây ám chỉ việc các layer phục vụ các mục đích nhất định, khi muốn thực hiện một công việc ngoài phạm vi thì sẽ đưa công việc xuống các layer thấp hơn.

Kiến trúc Controller - Service - Repository chia project thành 3 lớp:

- Consumer Layer hay Controller: là tầng giao tiếp với bên ngoài và handler các request từ bên ngoài tới hệ thống.
- Service Layer: Thực hiện các nghiệp vụ và xử lý logic
- Repository Layer: Chịu trách nhiệm giao tiếp với các DB, thiết bị lưu trữ, xử lý query và trả về các kiểu dữ liệu mà tầng Service yêu cầu.

### @Controller vs @Service vs @Repository

Để phục vụ cho kiến trúc ở trên, Spring Boot tạo ra 3 Annotation là `@Controller`, `@Service`, `@Repository` để chúng ta có thể đánh dấu các tầng với nhau.

- `@Service` Đánh dấu một Class là tầng Service, phục vụ các logic nghiệp vụ.
- `@Repository` Đánh dấu một Class Là tầng Repository, phục vụ truy xuất dữ liệu.

### Implement

Chạy chương trình:

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class App {

    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(App.class, args);

        // Lấy ra bean GirlService
        GirlService girlService = context.getBean(GirlService.class);
        // Lấu ra random một cô gái từ tầng service
        Girl girl = girlService.getRandomGirl();
        // In ra màn hình
        System.out.println(girl);

    }
}
```

### Giải thích

Về bản chất `@Service` và `@Repository` cũng chính là `@Component`. Nhưng đặt tên khác nhau để giúp chúng ta phân biệt các tầng với nhau.

Cùng nhìn vào source code của 2 Annotation này:

_Service.java_

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component // Cũng là một @Component
public @interface Service {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
```

_Repository.java_

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Repository {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
```

Trong các bài đầu tiên chúng ta đã biết `@Component` đánh dấu cho Spring Boot biết Class đó là `Bean`. Và hiển nhiên `@Service` và `@Repository` cũng vậy. Vì thế ở ví dụ trên chúng ta có thể lấy `GirlService` từ `ApplicationContext`. Về bản chất thì bạn có thể sử dụng thay thế 3 Annotation `@Component`, `@Service` và `@Repository` cho nhau mà không ảnh hưởng gì tới code của bạn cả. Nó vẫn sẽ hoạt động.

Tuy nhiên từ góc độ thiết kế thì chúng ta cần phân rõ 3 Annotation này cho các Class đảm nhiệm đúng nhiệm vụ của nó.

- `@Service` gắn cho các `Bean` đảm nhiệm xử lý logic
- `@Repository` gắn cho các `Bean` đảm nhiệm giao tiếp với DB
- `@Component` gắn cho các `Bean` khác.

