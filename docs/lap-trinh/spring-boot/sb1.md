# Hướng dẫn @Component và @Autowired

### Cách chạy ứng dụng Spring Boot

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

Spring Boot khi chạy sẽ dò tìm toàn bộ các _Class cùng cấp_ hoặc ở trong các _package thấp hơn_ so với class `App` mà bạn cung cấp cho Spring (Chúng ta có thể cấu hình việc tìm kiếm này, sẽ đề cập sau). Trong quá trình dò tìm này, khi gặp một class được đánh dấu `@Component` thì nó sẽ tạo ra một instance và đưa vào `ApplicationContext` để quản lý.

### @Autowired

```
@Component
public class BanhMi {

    @Autowired
    NguyenLieu pate;

    public BanhMi (NguyenLieu pate) {
        this.pate = pate;
    }

    // GET
    // SET
}
```

Tôi đánh dấu thuộc tính `NguyenLieu` của `BanhMi` bởi Annotation `@Autowired`. Điều này nói với Spring Boot hãy tự _inject_ (tiêm) một instance của `NguyenLieu` vào thuộc tính này khi khởi tạo `BanhMi`.


### Singleton

Điều đặc biệt là các `Bean` được quản lý bên trong `ApplicationContext` đều là singleton.

Tất cả những `Bean` được quản lý trong `ApplicationContext` đều chỉ được tạo ra một lần duy nhất và khi có `Class` yêu cầu `@Autowired` thì nó sẽ lấy đối tượng có sẵn trong `ApplicationContext` để _inject_ vào.

Trong trường hợp bạn muốn mỗi lần sử dụng là một instance hoàn toàn mới. Thì hãy đánh dấu `@Component` đó bằng `@Scope("prototype")`
