# @Configuration và @Bean

### @Configuration và @Bean

`@Configuration` là một Annotation đánh dấu trên một `Class` cho phép Spring Boot biết được đây là nơi định nghĩa ra các _Bean_.

`@Bean` là một Annotation được đánh dấu trên các `method` cho phép Spring Boot biết được đây là _Bean_ và sẽ thực hiện đưa _Bean_ này vào `Context`.

`@Bean` sẽ nằm trong các class có đánh dấu `@Configuration`.

Ví dụ:

_SimpleBean.java_

```java
// Một class cơ bản, không sử dụng `@Component`

public class SimpleBean {
    private String username;

    public SimpleBean(String username) {
        setUsername(username);
    }

    @Override
    public String toString() {
        return "This is a simple bean, name: " + username;
    }
}
```

_AppConfig.java_

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {
    @Bean
    SimpleBean simpleBeanConfigure(){
        // Khởi tạo một instance của SimpleBean và trả ra ngoài
        return new SimpleBean("loda");
    }
}
```

_App.java_

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class App {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(App.class, args);
        // Lấy ra bean SimpleBean trong Context
        SimpleBean simpleBean = context.getBean(SimpleBean.class);
        // In ra màn hình
        System.out.println("Simple Bean Example: " + simpleBean.toString());
        // OUTPUT: Simple Bean Example: This is a simple bean, name: loda
    }
}
```

Bạn sẽ thấy là `SimpleBean` là một object được quản lý trong `Context` của Spring Boot, mặc dù trong bài này, chúng ta không hề sử dụng tới các khái niệm `@Component`.

### Hoạt động nền

Đằng sau chương trình, Spring Boot lần đầu khởi chạy, ngoài việc đi tìm các `@Component` thì nó còn làm một nhiệm vụ nữa là tìm các class `@Configuration`.

1. Đi tìm class có đánh dấu `@Configuration`
2. Tạo ra đối tượng từ class có đánh dấu `@Configuration`
3. tìm các method có đánh dấu `@Bean` trong đối tượng vừa tạo
4. Thực hiện gọi các method có đánh dấu `@Bean` để lấy ra các _Bean_ và đưa vào \`Context.

Ngoài ra, về bản chất, `@Configuration` cũng là `@Component`. Nó chỉ khác ở ý nghĩa sử dụng. (Giống với việc class được đánh dấu `@Service` chỉ nên phục vụ logic vậy).

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component // Nó được đánh dấu là Component
public @interface Configuration {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
```

### Có ý nghĩa gì?

Nhiều bạn sẽ tự hỏi rằng `@Configuration` và `@Bean` sẽ có ý nghĩa gì khi chúng ta đã có `@Component`? Sao không đánh dấu `SimpleBean` là `@Component` cho nhanh?

Các bạn thắc mắc rất đúng, và việc sử dụng `@Component` cũng hoàn toàn ổn.

Thông thường thì các class được đánh dấu `@Component` đều có thể tạo tự động và inject tự động được.

Tuy nhiên trong thực tế, nếu một `Bean` có quá nhiều logic để khởi tạo và cấu hình, thì chúng ta sẽ sử dụng `@Configuration` và `@Bean` để tự tay tạo ra `Bean`. Việc tự tay tạo ra `Bean` như này có thể hiểu phần nào là chúng ta đang _config_ cho chương trình.

### Ví dụ

[Đọc trong trang bài viết](https://viblo.asia/p/spring-boot-6-atconfiguration-va-atbean-bJzKmyprK9N#_vi-du-5)

### @Bean có tham số

Nếu method được đánh dấu bởi `@Bean` có tham số truyền vào, thì Spring Boot sẽ tự inject các Bean đã có trong `Context` vào làm tham số.

Ví dụ:

_AppConfig.java_

```java
@Configuration
public class AppConfig {

    @Bean
    SimpleBean simpleBeanConfigure(){
        // Khởi tạo một instance của SimpleBean và trả ra ngoài
        return new SimpleBean("loda");
    }

    @Bean("mysqlConnector")
    DatabaseConnector mysqlConfigure(SimpleBean simpleBean) { // SimpleBean được tự động inject vào.
        DatabaseConnector mySqlConnector = new MySqlConnector();
        mySqlConnector.setUrl("jdbc:mysql://host1:33060/" + simpleBean.getUsername());
        // Set username, password, format, v.v...
        return mySqlConnector;
    }
}
```

### Thực tế

Trong thực tế, việc sử dụng `@Configuration` là hết sức cần thiết, và nó đóng vai trò là nơi cấu hình cho toàn bộ ứng dụng của bạn. Một Ứng dụng sẽ có nhiều class chứa `@Configuration` và mỗi class sẽ đảm nhận cấu hình một bộ phận gì đó trong ứng dụng.
