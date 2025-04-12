# Bài 1: Giới thiệu Java, JVM và Hellooo world

Đọc bài gốc của Loda đi nhá, cái này chỉ ghi lại một số ý để cho mình quay lại tham khảo thôi.

### Cài đặt môi trường

`Java` hoạt động như vậy, nó chỉ nói 1 ngôn ngữ duy nhất thôi, tuy nhiên nó có một thằng anh bá đạo, tên ông ý nôm na là môi trường ảo hay tên chuẩn là `Java virtual machine (JVM)`. Nhiệm vụ của `JVM` là nó phụ đề (thuyết minh) cho từng loại OS khác nhau rằng thằng `Java` đang làm gì, nói gì, làm gì.

Vì chúng ta là Developer nên sẽ cài gói `JDK` (`Java Development Kit`), nó chứa các công cụ giúp lập trình `Java`. Ngoài ra trong quá trình cài, nó sẽ cài môi trường `JRE` (`Java Runtime Enviroment`, bao gồm cả thằng `JVM` ở trên) luôn.

### Hello World

Tạo Project trong Intellij chẳng hạn, xong rồi thì cùng nhìn vào cấu trúc của project thì sẽ thấy có 3 thư mục:

- `.idea`: Thằng này là thư mục do `Intellij` tự tạo ra để chứa các file config của phần mềm này, bạn sẽ k cần quan tâm đến.
- `src`: Đây là thư mục chính bạn sẽ làm việc, tất cả `code` bạn để trong này
- `{project-name}.iml`: File này cũng do `Intellj` tạo ra và quản lý module, bạn không cần quan tâm nó.

### Một số thông tin khác

- `public static void main(String[] args)`: (Gọi tắt là `psvm` nhé) Cái thằng này sẽ là nơi `Java` tìm tới đầu tiên, và đọc toàn bộ các đoạn code trong cái thằng tên là `psvm` này. Dù nó ở bất cứ đâu, nó sẽ được tìm tới.
- 2 cái dấu `{``}`: Đánh dấu đoạn bắt đầu và kết thúc của cái `public static void main(String[] args)` kia.

Vậy là thằng `Java` sẽ đi lùng tìm, xem cái thằng `psvm` xem nó ở đâu. Rồi đọc hết tất cả những thứ nằm trong cái 2 dấu `{``}` của thằng này.

