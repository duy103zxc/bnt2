[SB5] Component Scan là gì?
=============================

- Giới thiệu
- Cài đặt
- Component Scan
- Component Scan
- Cách 1: @ComponentScan
- Cách 2: scanBasePackages
- Multiple package scan
- Kết

### **Giới thiệu**

Trong bài trước tôi đã đề cập tới 2 trong số 3 Annotation cơ bản trong thiết kế Layer của **Spring Boot**.

1. [🌟SB4] @Component vs @Service vs @Repository

Trong bài hôm này, chúng ta sẽ tìm hiểu thêm về cách **Spring Boot** tìm kiếm `Bean` trong project của bạn như thế nào.

### **Cài đặt**

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <packaging>pom</packaging>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.5.RELEASE</version>
        <relativePath /> <!-- lookup parent from repository -->
    </parent>
    <groupId>me.loda.spring</groupId>
    <artifactId>spring-boot-learning</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-boot-learning</name>
    <description>Everything about Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>

        <!--spring mvc, rest-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
        </plugins>
    </build>

</project>
```

### **Component Scan**

Trong bài 1 tôi có đề cập một lần về việc **Spring Boot** khi chạy sẽ dò tìm toàn bộ các _Class cùng cấp_ hoặc ở trong các _package thấp hơn_ và tạo ra Bean từ các Class tìm thấy.

Bây giờ chúng ta sẽ nói sâu hơn một chút!

Thử ví dụ này nhé:

Chúng ta có một project có cấu trúc thư mục như này:

!image

Tôi tạo ra 2 Bean:

1. `Girl`. Nằm cùng package với `App`
2. `OtherGirl`. Nằm ở package con `others`. `others` cùng cấp với `App`

_Girl.java_

```
@Component
public class Girl {

    @Override
    public String toString() {
        return "Girl.java";
    }
}
```

_OtherGirl.java_

```
@Component
public class OtherGirl {
    @Override
    public String toString() {
        return "OtherGirl.java";
    }
}
```

_App.java_

```
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(App.class, args);
        try {
            Girl girl = context.getBean(Girl.class);
            System.out.println("Bean: " + girl.toString());
        } catch (Exception e) {
            System.out.println("Bean Girl không tồn tại");
        }

        try {
            OtherGirl otherGirl = context.getBean(OtherGirl.class);
            if (otherGirl != null) {
                System.out.println("Bean: " + otherGirl.toString());
            }
        } catch (Exception e) {
            System.out.println("Bean Girl không tồn tại");
        }
    }
}
```

Chạy chương trình:

```
Bean: Girl.java
Bean: OtherGirl.java
```

Kết quả in ra màn hình là cả 2 bean `Girl` và `OtherGirl` đều được tạo ra trong `Context`.

Điều này chứng tỏ **Spring Boot** đã đi tìm các `Bean` bên cạnh class `App` và những package con bên cạnh `App`

### **Component Scan**

Trong trường hợp bạn muốn tuỳ chỉnh cấu hình cho **Spring Boot** chỉ tìm kiếm các bean trong một package nhất định thì có các cách sau đây:

1. Sử dụng `@ComponentScan`
2. Sử dụng `scanBasePackages` tromg `@SpringBootApplication`.

### **Cách 1:**`@ComponentScan`

Sửa file _App.java_ thành:

```
@ComponentScan("me.loda.spring.componentscan.others")
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(App.class, args);
        try {
            Girl girl = context.getBean(Girl.class);
            System.out.println("Bean: " + girl.toString());
        } catch (Exception e) {
            System.out.println("Bean Girl không tồn tại");
        }

        try {
            OtherGirl otherGirl = context.getBean(OtherGirl.class);
            if (otherGirl != null) {
                System.out.println("Bean: " + otherGirl.toString());
            }
        } catch (Exception e) {
            System.out.println("Bean Girl không tồn tại");
        }
    }
}
```

### **Cách 2:**`scanBasePackages`

```
@SpringBootApplication(scanBasePackages = "me.loda.spring.componentscan.others")
public class App {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(App.class, args);
        try {
            Girl girl = context.getBean(Girl.class);
            System.out.println("Bean: " + girl.toString());
        } catch (Exception e) {
            System.out.println("Bean Girl không tồn tại");
        }

        try {
            OtherGirl otherGirl = context.getBean(OtherGirl.class);
            if (otherGirl != null) {
                System.out.println("Bean: " + otherGirl.toString());
            }
        } catch (Exception e) {
            System.out.println("Bean Girl không tồn tại");
        }
    }
}
```

Cả 2 cách đều cho kết quả in ra màn hình như sau:

```
Bean Girl không tồn tại
Bean: OtherGirl.java
```

Lúc này, **Spring Boot** chỉ tìm kiếm các bean trong package `others` mà thôi. Nên khi lấy ra `Girl` thì nó không tồn tại trong `Context`.

### **Multiple package scan**

Bạn có thể cấu hình cho **Spring Boot** Tìm kiếm các Bean ở nhiều package khác nhau bằng cách:

```
@ComponentScan({"me.loda.spring.componentscan.others2","me.loda.spring.componentscan.others"})
```

hoặc

```
@SpringBootApplication(scanBasePackages = {"me.loda.spring.componentscan.others", "me.loda.spring.componentscan.others2"})
```

### **Kết**

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

