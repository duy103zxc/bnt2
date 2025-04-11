# @Autowired - @Primary - @Qualifier

Trong bài viết này chúng ta sẽ cùng tìm hiểu cách `@Autowỉed` vận hành và cách sử dụng 2 Annotation `@Primary`, `@Qualifier`.


### Cách inject Bean của Spring

`@Autowired` đánh dấu cho Spring biết rằng sẽ tự động inject bean tương ứng vào vị trí được đánh dấu.

```
@Component
public class Girl {
    // Đánh dấu để Spring inject một đối tượng Outfit vào đây
    @Autowired
    Outfit outfit;

    // public Girl(Outfit outfit) {
    //     this.outfit = outfit;
    // }

    // GET
    // SET
}
```

Sau khi tìm thấy một class đánh dấu `@Component`. thì quá trình inject `Bean` xảy ra theo cách như sau:

1. Nếu `Class` không có hàm Constructor hay Setter. Thì sẽ sử dụng Java Reflection để đưa đối tượng vào thuộc tính có đánh dấu `@Autowired`.
2. Nếu có hàm Constructor thì sẽ inject Bean vào bởi tham số của hàm
3. Nếu có hàm Setter thì sẽ inject Bean vào bởi tham số của hàm

Như ví dụ ở trên tôi đã sử dụng cách Java Reflection để inject `Bean` vào class `Girl`. 

Bạn cũng có thể gắn `@Autowired` lên trên method, thay vì thuộc tính, chức năng cũng vẫn tương tự, nó sẽ tìm Bean phù hợp với method đó và truyền vào.

```java
@Component
public class Girl {

    // Đánh dấu để Spring inject một đối tượng Outfit vào đây
    @Autowired
    Outfit outfit;

    // Spring sẽ inject outfit thông qua Constructor trước
    public Girl() { }

    // Nếu không tìm thấy Constructor thoả mãn, nó sẽ thông qua setter
    public void setOutfit(Outfit outfit) {
        this.outfit = outfit;
    }

    @Autowired
    // Nếu không tìm thấy Constructor thoả mãn, nó sẽ thông qua setter, 
    // nếu v thì đừng gắn Autowired ở properties ở trên nha
    public void setOutfit(Outfit outfit) {
        this.outfit = outfit;
    }

    // GET
    // SET
}
```

Nếu không sử dụng `@Autowired` thì bạn phải có một Constructor thay thế, hoặc một Setter tương ứng. Khá nhiều các đồng chí gợi ý không dùng `@Autowired` và thay thế bằng:

```java
@Component
public class Calculator {
   private Multiplier multiplier;
   // Cái lày là constructor
   public Calculator(Multiplier multiplier) {
      this.multiplier = multiplier;
   }
}
```

### Vấn đề của @Autowired

Trong thực tế, sẽ có trường hợp chúng ta sử dụng `@Autowired` khi **Spring Boot** có chứa 2 Bean cùng loại trong Context. Lúc này thì **Spring** sẽ bối rối và không biết sử dụng Bean nào để inject vào đối tượng.

### **@Primary**

Cách giải quyết thứ nhất là sử dụng Annotation `@Primary`. `@Primary` là annotation đánh dấu trên một Bean, giúp nó **luôn được ưu tiên lựa** chọn trong trường hợp có nhiều Bean cùng loại trong Context.

Trong ví dụ ở trên, nếu chúng ta để `Suit` là primary. Thì chương trình sẽ chạy bình thường. Và hiển nhiên `Outfit` bên trong `Girl` sẽ là `Suit`.


Chạy thử chương trình:

```java
// ===========================================================================
@Component
@Primary
public class Suit implements Outfit {
    @Override
    public void wear() {
        System.out.println("Đang mặc âu phục");
    }
}
// ===========================================================================
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        // ApplicationContext chính là container, chứa toàn bộ các Bean
        ApplicationContext context = SpringApplication.run(App.class, args);
        // Khi chạy xong, lúc này context sẽ chứa các Bean có đánh
        // dấu @Component.
        Girl girl = context.getBean(Girl.class);
        System.out.println("Girl Instance: " + girl);
        System.out.println("Girl Outfit: " + girl.outfit);
        girl.outfit.wear();
    }
}
// ===========================================================================
// Output:
// Girl Instance: me.loda.spring.helloprimaryqualifier.Girl@eb9a089
// Girl Outfit: me.loda.spring.helloprimaryqualifier.Naked@1688653c
// Đang mặc âu phục
```


### **@Qualifier**

Cách thứ hai, là sử dụng Annotation `@Qualifier`. `@Qualifier` xác định tên của một Bean mà bạn muốn chỉ định inject.

Ví dụ:

```java
@Component("suit")
public class Suit implements Outfit {
    @Override
    public void wear() {
        System.out.println("Mặc âu phục");
    }
}

// ==============================================
@Component
public class Girl {

    Outfit outfit;

    // Đánh dấu để Spring inject một đối tượng Outfit vào đây
    public Girl(@Qualifier("suit") Outfit outfit) {
        // Spring sẽ inject outfit thông qua Constructor đầu tiên
        // Ngoài ra, nó sẽ tìm Bean có @Qualifier("naked") trong context để ịnject
        this.outfit = outfit;
    }
    // GET
    // SET
}
```