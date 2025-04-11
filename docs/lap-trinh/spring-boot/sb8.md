# @Controller + Thymeleaf

### **@Controller**

Trong bài viết số 4, khi nói về `@Service` và `@Repository` tôi đã đề cập tới kiến trúc trong **Spring Boot**.

Để xây dựng một trang web với **Spring Boot**, bạn sẽ cần tuân theo quy trình như hình dưới đây:

![](https://images.viblo.asia/95ff7fc2-b915-419a-bb14-38bfdf5bf250.png)

`@Controller` là nơi tiếp nhận các thông tin request từ phía người dùng. Nó có nhiệm vụ đón nhận các yêu cầu (kèm theo thông tin request) và chuyển các yêu cầu này xuống cho tầng `@Serivce` xử lý logic.

### **HTML**

Để tạo ra một trang Web, bạn sẽ cần tạo ra các trang html để trả về cho người dùng. Mặc định trong **Spring Boot**, các file html này sẽ được lưu trữ trong thư mục `resources/templates`. **Spring Boot + Thymeleaf** sẽ tìm kiếm các file này theo tên. Ví dụ "index" sẽ tương ứng với "index.html".

### Giải thích 1

Bản thân `@Controller` Cũng là một `@Component` nên nó sẽ được **Spring Boot** quản lý.

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Controller {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
```

**Spring Boot** sẽ lắng nghe các request từ phía người dùng. và tùy theo đường dẫn `path` là gì, nó sẽ mapping tới hàm xử lý tương ứng trong `@Controller`.

Như ví dụ trên, tôi sử dụng `GET` vào địa chỉ `localhost:8080/` ( đường dẫn là `/`). **Spring Boot** sẽ gọi tới hàm có gắn `@GetMapping("/")` và yêu cầu hàm này xử lý request này.

### Giải thích 2

Tới đây bạn hãy tham chiếu đường dẫn request với hàm xử lý nó:

```java
// http://localhost:8080/hello?name=Loda
    @GetMapping("/hello")
    public String hello(
            // Request param "name" sẽ được gán giá trị vào biến String
            @RequestParam(name = "name", required = false, defaultValue = "") String name,
            // Model là một object của Spring Boot, được gắn vào trong mọi request.
            Model model
    ) {
        // Gắn vào model giá trị name nhận được
        model.addAttribute("name", name);

        return "hello"; // trả về file hello.html cùng với thông tin trong object Model
    }
```

Khi request lên, chúng ta nhận được giá trị của `name` và tiếp tục gán nó vào `Model`.

![](https://images.viblo.asia/c1490e7d-3093-430e-b22e-32ab1395c199.png)

`Model` ở đây là một object được **Spring Boot** đính kém trong mỗi response. `Model` chứa các thông tin mà bạn muốn trả về và **Template Engine** sẽ trích xuất thông tin này ra thành html và đưa cho người dùng. Trong file `hello.html` tôi lấy giá trị của `name` trong `Model` ra bằng cách sử dụng cú pháp của `Thymeleaf`

```
<h1 th:text="'Hello, ' + ${name}"></h1>
```

