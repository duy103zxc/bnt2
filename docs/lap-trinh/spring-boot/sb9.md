# Giải thích cách Thymeleaf vận hành + Expression

### Thymeleaf

Thymeleaf là một Java Template Engine. Có nhiệm vụ xử lý và generate ra các file HTML, XML, v.v.. Các file HMTL do Thymeleaf tạo ra là nhờ kết hợp dữ liệu và template + quy tắc để sinh ra một file HTML chứa đầy đủ thông tin. Việc của bạn là cung cấp dữ liệu và quy định template như nào, còn việc dùng các thông tin đó để render ra HTML sẽ do Thymeleaf giải quyết.

### Cú pháp

Cú pháp của Thymeleaf sẽ là một attributes (Thuộc tính) của thẻ HTML và bắt đầu bằng chữ `th:`.

Ví dụ: Để truyền dữ liệu từ biến `name` trong Java vào một thẻ `H1` của HTML.

```html
<h1 th:text="${name}"></h1>
```

Chúng ta viết thẻ h1 như bình thường, nhưng không chứa bất cứ text nào trong thẻ. Mà sử dụng cú pháp `th:text="${name}"` để Thymeleaf lấy thông tin từ biến `name` và đưa vào thẻ `H1`.

Kết quả khi render ra, thuộc tính `th:text` biến mất và giá trị biến `name` được đưa vào trong thẻ `H1`. Đó là cách Thymeleaf hoạt động:

```java
// Giả sử String name = "loda"
<h1>Loda</h1>
```

### Model & View Trong Spring Boot

`Model` là đối tượng lưu giữ thông tin và được sử dụng bởi Template Engine để generate ra webpage. Có thể hiểu nó là `Context` của Thymeleaf. `Model` lưu giữ thông tin dưới dạng key-value.

Trong template thymeleaf, để lấy các thông tin trong `Model`. bạn sẽ sử dụng `Thymeleaf Standard Expression`.

1. `${...}`: Giá trị của một biến.
2. `*{...}`: Giá trị của một biến được chỉ định.

Ngoài ra, để lấy thông tin đặc biệt hơn:

1. `#{...}`: Lấy message
2. `@{...}`: Lấy đường dẫn URL dựa theo context của server

Nói tới đây có lẽ hơi khó hiểu, chúng ta sẽ dùng ví dụ để hiểu rõ từng loại Expression.

### `${...}` - Variables Expressions

Trên Controller bạn đưa vào một số giá trị:

```
model.addAttribute("today", "Monday");
```

Để lấy giá trị của biến `today`, tôi sử dụng `${...}`

```
<p>Today is: <span th:text="${today}"></span>.</p>
```

### `*{...}` - Variables Expressions on selections

Dấu `*` còn gọi là `asterisk syntax`. Chức năng của nó giống với `${...}` là lấy giá trị của một biến. Điểm khác biệt là nó sẽ lấy ra giá trị của một biến cho trước bởi `th:object`

```html
<div th:object="${session.user}"><!-- th:object tồn tại trong phạm vi của thẻ div này -->
    <!-- Lấy ra tên của đối tượng session.user -->
    <p>Name: <spanth:text="*{firstName}"></span>.</p><!-- Lấy ra lastName của đối tượng session.user -->
    <p>Surname: <spanth:text="*{lastName}"></span>.</p>
</div>
```

Vậy đoạn code ở trên tương đương với:

```
<div><p>Name: <spanth:text="${session.user.firstName}"></span>.</p><p>Surname: <spanth:text="${session.user.lastName}"></span>.</p></div>
```

Còn `${...}` sẽ lấy ra giá trị cục bộ trong `Context` hay `Model`.

### `#{...}` - Message Expression

Ví dụ, trong file config `.properties` của tôi có một message chào người dùng bằng nhiều ngôn ngữ.

```
home.welcome=¡Bienvenido a nuestra tienda de comestibles!
```

Thì cách lấy nó ra nhanh nhất là:

```
<p th:utext="#{home.welcome}">Xin chào các bạn!</p>
```

Đoạn text tiếng việt bên trong thẻ `p` sẽ bị thay thế bởi thymeleaf khi render `#{home.welcome}`.

### `@{...}` - URL Expression

`@{...}` xử lý và trả ra giá trị URL theo context của máy chủ cho chúng ta.

Ví dụ:

```
// <a th:href="@{/order/list}">
// If our app is installed at http://localhost:8080/myapp, this URL will output:
// <a href="/myapp/order/list">
<a href="details.html" th:href="@{/order/details(orderId=${o.id})}">view</a>
<ahref="details.html"th:href="@{/order/{orderId}/details(orderId=${o.id})}">view</a>
```

Nếu bắt dầu bằng dấu `/` thì nó sẽ là Relative URL và sẽ tương ứng theo context của máy chủ của bạn.