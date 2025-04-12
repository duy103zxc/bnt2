# 「Java 8」Functional Interfaces & Lambda Expressions cực dễ hiểu

### Giới thiệu

Khái niệm `Functional Interfaces` được `Java` đưa ra cùng với phiên bản `Java 8`. về cơ bản, có thể hiểu:

> Functional Interfaces là interface nhưng chỉ có một 1 abstract function duy nhất.

Ví dụ:

```java
interface Runable {
    public void run(); // Chỉ có duy nhất một abstract function.
}
```

### Functional Programming

Trước khi đi vào chi tiết, chúng ta cùng tìm hiểu khái niệm `Lập trình hướng hàm`.

Cùng xem ví dụ dưới đây:

```java
public static void main(String[] args) {
    // Mình muốn xử lý dữ liệu trước khi ỉn ra màn hình.
    System.out.println(process("Hey Loda!!!"));
}

public static String process(String input){
    // Cho tất cả viết hoa lên.
    return input.toUpperCase();
}

// Output:
HEY LODA!!!
```

Tuy nhiên bạn sẽ thấy cách làm này không `flexible`, vì các bạn chỉ có thể xử lý cho chữ thành `UPPER CASE`. Muốn làm gì đó khác, như `toLowerCase` chẳng hạn, mình sẽ phải viết một `function` mới.

Chúng ta giải quyết cách cách này bằng `Anonymous function (Hàm ẩn danh)`

Sửa code chút:

```java
public interface StringProcessor{
    public String process(String input);
}

// StringProcessor ở đây là một Interface, hay Functional Interface
public static String getStr(String input, StringProcessor processor){
    return processor.process(input);
}

public static void main(String[] args) {
    // In ra chữ hoa
    System.out.println(getStr("Hello Loda!", new StringProcessor() {
        @Override
        public String process(String input) {
            return input.toUpperCase();
        }
    }));

    // In ra chữ thường
    System.out.println(getStr("Hey Loda!", new StringProcessor() {
        @Override
        public String process(String input) {
            return input.toLowerCase();
        }
    }));
}
// Output:
// HELLO LODA!
// hey loda!
```

### Lambda Expressions

Quay lại ví dụ ở trên, chúng ta thấy là `StringProcessor` chỉ có duy nhất một `function process(x)`. Nên mọi đoạn code đều sẽ giống hệt nhau ở việc `implement function` này.

```java
new StringProcessor() {
    @Override
    public String process(String input) {
        // Do something here
        // Chỉ khác nhau đoạn code ở giữa
        return x;
    }
}
```

Thực ra cái chúng ta quan tâm là: `Input -> Process -> Output`. Hãy thử nhìn ở ví dụ dưới cho Lambda Expressions:


```java
// (input) -> input.toUpperCase()
// đầu vào -> đầu ra
System.out.println(getStr("Hello Loda!", input -> input.toUpperCase()));

// Cấu trúc của một lambda như sau:
// parameter -> expression body
```

Trong đó:

- `parameter` là những tham số đầu vào của hàm (một hoặc nhiều)
- `expression body` là phần xử lý `parameter`, bạn cần trả ra đúng kiểu dữ liệu đã khai báo trong `Functional Interface`

Nếu `code` bạn chỉ cần 1 thao tác, thì không cần `return` giống ví dụ ở trên. Còn nếu `code` yêu cầu xử lý nhiều, thì dạng đầy đủ của nó như sau:

```
parameter -> {
    expression body
    [return] // (không trả về nếu là void)
}
```

ví dụ:

```java
System.out.println(getStr("Hello Loda!", input -> {
    String temp =  input + " Oke!!!";
    return temp.toLowerCase();
}));
```

### Functional Interface

Tới đây, bạn đã hiểu ý nghĩa của việc cho ra đời khái niệm `Functional Interface`, nó là một quy định chung phải có để có thể viết code dưới dạng biểu thức `Lambda`. Một số điều cần lưu ý với `Functional Interface` như sau:

#### @FunctionalInterface

`Annotation` này chỉ để bổ sung, nó đánh dấu một `interface` là `Functional Interface`. Lúc này bạn khai báo 2 `abtract function` bên trong `interface` thì sẽ báo lỗi.

```java
@FunctionalInterface // Gắn cái này lên interface, nó đánh dấu interface chỉ được phép có 1 funtion thôi
public interface StringProcessor{
    public String process(String input);
    public String preProcess(String input); // lỗi
}
```

#### default function & static funtion

`Java 8` cải tiến cho phép `interface` được khai báo `code` bên trong nó, với điều kiện `code` phải nằm trong `default` hoặc `static`. `default` và `static` không phá vỡ quy luật của `@FunctionInterfaces`

```java
@FunctionalInterface // Gắn cái này lên interface, nó đánh dấu interface chỉ được phép có 1 funtion thôi
public interface StringProcessor{
    public String process(String input);

    // Mọi class implement StringProcessor đều có thể gọi hàm này để sử dụng luôn
    public default void printf(Object t){
        System.out.println(t);
    }

    // Là hàm static, gọi từ class cũng được.         
    // StringProcessor.concat(a,b)
    public static String concat(String a, String b){
        return a + b;
    }
}
```

### Method reference

Phần này chỉ để bổ sung, không có nó, bạn vẫn có thể sử dụng `Lambda Expressions` bình thường. Nhưng với `Method reference`, code của bạn sẽ còn sạch sẽ hơn nữa.

Ví dụ:

```java
System.out.println(getStr("Hello Loda!", input -> input.toUpperCase()));
// Tương đương với việc viết như này:
System.out.println(getStr("Hello Loda!", String::toUpperCase));
```

hoặc


```java
System.out.println(getStr("Hello Loda!", input -> new String(input));
// Tương đương với việc viết như này:
System.out.println(getStr("Hello Loda!", String::new));
```

`Method reference` là cách viết ngắn gọn, sẽ bỏ qua luôn cả phần `parameter` vì bản thân tên hàm đã biết nó sẽ nhận vào gì và trả ra cái gì rồi. Việc còn lại để `Compiler` lo thôi kakaka. Có các cách để gọi `Method reference` như sau:

- `[Tên Class]::[Tên method]`: Giống với ví dụ ở trên `String::toUpperCase`.
- `[Tên Class]::new`: Tạo ra một đối tượng mới, từ tham số được truyền vào
