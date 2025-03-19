「Jpa」Hướng dẫn sử dụng @OneToOne
=============================================

The Wayback Machine - https://web.archive.org/web/20231001012931/https://loda.me/articles/jpahuong-dan-su-dung-onetoone

![Logo](https://web.archive.org/web/20231001012931im_/https://super-static-assets.s3.amazonaws.com/8a72ee8e-d4aa-4a06-985f-e92802c5bc44/uploads/logo/36872858-1bc0-4117-bb6b-81d9934b5275.svg)

- Home
- Khóa học
- #dalog

Created

Oct 28, 2021 6:49 AM

- Giới thiệu
- Tạo project
- Tạo Table
- Chạy thử
- Thêm dữ liệu

### **Giới thiệu**

Cách biểu thị quan hệ 1-1 trong cơ sở dữ liệu là rất phổ biến, ví dụ một người sẽ có một địa chỉ duy nhất (giả sử).

Bình thường, khi các bạn tạo table trong csdl để biểu thị mối quan hệ này, thì sẽ có một bảng chứa khóa ngoại của bảng còn lại.

!image

Thể hiện mỗi quan hệ này trong `code` bằng `Hibernate` thì chúng ta sẽ dùng `@OneToOne`.

Trong bài sử dụng các kiến thức:

1. Hibernate là gì?
2. Cách sử dụng Lombok để tiết kiệm thời gian code

### **Tạo project**

Toàn bộ bài viết được up tại `Github`: github.com/loda-kun/java-all

Chúng ta sẽ sử dụng `Gradle` để tạo một project có khai báo `Spring Boot` và `Jpa` để hỗ trợ cho việc demo `@OneToOne`.

Các bạn có thể tự tạo 1 project Spring-boot với gradle đơn giản tại: https://start.spring.io

```
plugins {
    id 'org.springframework.boot' version '2.1.4.RELEASE'
    id 'java'
}
apply plugin: 'io.spring.dependency-management'

group 'me.loda.java'
version '1.0-SNAPSHOT'

sourceCompatibility = 1.8

configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    runtimeOnly 'com.h2database:h2'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

Trong ứng dụng trên bạn sẽ thấy có `com.h2database:h2`. Đây là một **database**, tuy nhiên nó chỉ tồn tại trong bộ nhớ. Tức làm mỗi khi chạy chương trình này, nó sẽ tạo database trong `RAM`, và tắt chương trình đi nó sẽ mất.

Chúng ta sẽ sử dụng `H2` thay cho `MySql` để cho.. tiện!

Khi tạo xong project, sẽ có thư mục như sau:

!image

### **Tạo Table**

Để tạo table, chúng ta tạo ra các `Class` tương ứng.

```
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.OneToOne;

import lombok.Builder;
import lombok.Data;

@Entity // Hibernate entity
@Data // Lombok
@Builder // Lombok
public class Person { //Table person

    @Id // Đánh dấu trường này là primary key
    @GeneratedValue // Tự động tăng giá trị id
    private Long id;
    private String name;
}
```

```
@Entity
@Data
@Builder
public class Address { // Table address
    @Id
    @GeneratedValue
    private Long id;

    private String city;
    private String province;

    @OneToOne // Đánh dấu có mỗi quan hệ 1-1 với Person ở phía dưới
    @JoinColumn(name = "person_id") // Liên kết với nhau qua khóa ngoại person_id
    private Person person;
}
```

Nếu chúng ta chưa tạo ra các table trong cơ sở dữ liệu, thì mặc định `Hibernate` sẽ bind dữ liệu từ class xuống và tạo table cho chúng ta.

Bạn phải tạo file config `src\main\resources\application.properties` như sau để kết nối tới `H2` database nhé:

```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
// Không có password, vào thẳng luôn
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
# Cho phép vào xem db thông qua web
spring.h2.console.enabled=true
```

### **Chạy thử**

Bạn tạo file `OneToOneExampleApplication` và cấu hình `Spring Boot` và khởi chạy chương trình.

```
@SpringBootApplication
@RequiredArgsConstructor
public class OneToOneExampleApplication {
    public static void main(String[] args) {
        SpringApplication.run(OneToOneExampleApplication.class, args);
    }
}
```

Sau khi chạy xong, hãy truy cập vào `http://localhost:8080/h2-console/` để vào xem database có gì nhé.

!image

Bạn sẽ thấy nó tạo table giống với mô tả ở đầu bài. Với khóa ngoại `person_id` ở bảng `address`.

### **Thêm dữ liệu**

Để thêm dữ liệu vào database, chúng ta sẽ dùng tới `Jpa`.

```
import org.springframework.data.jpa.repository.JpaRepository;

public interface AddressRepository extends JpaRepository<Address,Long> {
}
public interface PersonRepository extends JpaRepository<Person, Long> {
}
```

Chúng ta sẽ tạo một chương trình `Spring Boot` đơn giản bằng cách sử dụng `CommandLineRunner` để chạy code ngay khi khởi động.

```
import javax.transaction.Transactional;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import com.google.common.collect.Lists;

import lombok.RequiredArgsConstructor;

@SpringBootApplication
@RequiredArgsConstructor
public class OneToOneExampleApplication implements CommandLineRunner {
    public static void main(String[] args) {
        SpringApplication.run(OneToOneExampleApplication.class, args);
    }

    // Sử dụng @RequiredArgsConstructor và final để thay cho @Autowired
    private final PersonRepository personRepository;
    private final AddressRepository addressRepository;

    @Override
    public void run(String... args) throws Exception {
        // Tạo ra đối tượng person
        Person person = Person.builder()
                              .name("loda")
                              .build();
        // Lưu vào db
        personRepository.save(person);

        // Tạo ra đối tượng Address có tham chiếu tới person
        Address address = Address.builder()
                .city("Hanoi")
                .person(person)
                .build();

        // Lưu vào db
        addressRepository.save(address);

        // Vào:http://localhost:8080/h2-console/ để xem dữ liệu đã insert
    }
}
```

Kết quả trong database lúc này:

!image

Vậy là thằng `Address` đã liên kết tới `Person` có `id=1`. Đúng như ta mong đợi.

Bài viết của mình không còn gì để ngắn hơn được nữa :((( thật hổ thẹn, mình có up code lên đây, bạn chạy code cái là hiểu liền à:

Chúc các bạn học tập tốt! ahuu

1. [🚅\
\
「Jpa」Hướng dẫn @OneToMany và @ManyToOne](https://web.archive.org/web/20231001012931/https://loda.me/articles/jpahuong-dan-onetomany-va-manytoone)
2. [🛵\
\
「Jpa」Hướng dẫn @ManyToMany](https://web.archive.org/web/20231001012931/https://loda.me/articles/jpahuong-dan-manytomany)

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

_Nếu bạn phát hiện bài viết có lỗi hoặc outdated, hãy báo lại giúp mình theo email:__loda.namnh@gmail.com__hoặc qua__Nam Hoàng Nguyễn (facebook.com)_





