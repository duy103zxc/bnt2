# Java Concurrency (Phần 1): Thread

Source: [Java Concurrency (Phần 1): Thread](https://viblo.asia/p/java-concurrency-phan-1-thread-GAWVpevY405)

## 1\. Giới thiệu

Lập trình đồng thời (concurrency) trong Java đề cập đến khả năng của một chương trình Java thực thi nhiều tác vụ đồng thời hoặc song song, tận dụng tối đa các bộ xử lý (CPU) đa lõi (core) hiện đại. Khi các ứng dụng ngày càng trở nên phức tạp và đòi hỏi hiệu suất cao hơn, lập trình đồng thời trở thành yếu tố thiết yếu để cải thiện hiệu năng, khả năng phản hồi và khả năng mở rộng.
Java cung cấp một bộ công cụ và các thư viện phong phú giúp các nhà phát triển tạo ra các ứng dụng đồng thời, quản lý nhiều luồng (threads) và điều phối các tác vụ một cách hiệu quả. Trong bài viết này, chúng sẽ khám phá các khái niệm cơ bản về lập trình đồng thời trong Java.

![image.png](https://images.viblo.asia/full/dab66ffc-6006-45b9-a745-5e5a9f09d9da.png)

## 2\. Định nghĩa Thread

Một thread là một đơn vị thực thi nhỏ hơn một process. **Một process có thể tạo ra nhiều thread trong quá trình thực thi**. Tất cả các thread trong cùng một process sẽ chia sẻ, **dùng chung một số vùng nhớ với nhau** (heap memory, static variables, metaspace, … phần này mình sẽ chia sẻ cụ thể hơn ở một bài viết khác). Vì vậy, việc giao tiếp giữa các thread khá đơn giản và dễ dàng hơn so với giao tiếp giữa các process. Ngoài ra, việc tạo mới/hủy thread đơn giản và tốn ít công hơn so với việc tạo mới/hủy một process. Vì các lý do này, thread còn được gọi là lightweight process.

![image.png](https://images.viblo.asia/full/cb4fb02e-990b-4290-bdb3-c7b3e9d030a1.png)

## 3\. Cách khởi tạo thread

Đây là một câu hỏi thường hay gặp trong phỏng vấn. Bạn có thể tham khảo hoặc trả lời như sau:
Ta có thể phân loại các cách khởi tạo thread như sau:

### 3.1. Tạo trực tiếp thread

sử dụng `new Thread().start()`.

```
new Thread(() -> resource.counter++).start();

```

### 3.2. Khai báo Thread execution method

#### 3.2.1. Kế thừa class Thread

Đây là một cách phổ biến. Chúng ta tạo ra một class mới kế thừa class Thread và ghi đè method run như sau:

```
public class ExtendsThread extends Thread {
    @Override
    public void run() {
        System.out.println("Do something");
    }

    public static void main(String[] args) {
        new ExtendsThread().start();
    }
}

```

#### 3.2.2. Triển khai interface Runnable

Đây cũng là một cách phổ biến, implement Runnable interface và override method run, như sau:

```
public class ImplementsRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Do something");
    }

    public static void main(String[] args) {
        ImplementsRunnable runnable = new ImplementsRunnable();
        new Thread(runnable).start();
    }
}

```

#### 3.2.3. Triển khai interface Callable

Tương tự như method trước, ngoại trừ method này có thể nhận giá trị trả về sau khi Thread được thực thi, như sau:

```
public class ImplementsCallable implements Callable<String> {
    @Override
    public String call() throws Exception {
        System.out.println("Do something");
        return "test";
    }

    public static void main(String[] args) throws Exception {
        ImplementsCallable callable = new ImplementsCallable();
        FutureTask<String> futureTask = new FutureTask<>(callable);
        new Thread(futureTask).start();
        System.out.println(futureTask.get());
    }
}

```

#### 3.2.4. Sử dụng class ẩn danh hoặc biểu thức Lambda

```
public class UseAnonymousClass {
    public static void main(String[] args) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("AnonymousClass");
            }
        }).start();

        new Thread(() ->
                System.out.println("Lambda")
        ).start();
    }
}

```

### 3.3. Tạo gián tiếp thread

#### 3.3.1. Sử dụng thread pool của ExecutorService

```
public class UseExecutorService {
    public static void main(String[] args) {
        ExecutorService poolA = Executors.newFixedThreadPool(2);
        poolA.execute(() -> {
            System.out.println("do something");
        });
}

```

#### 3.3.2. Sử dụng thread pool hoặc Stream song song (parallel stream)

```
public class UseForkJoinPool {
    public static void main(String[] args) {
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        forkJoinPool.execute( () -> {
            System.out.println("Do something");
        });

        List<String> list = Arrays.asList("e1");
        list.parallelStream().forEach(System.out::println);
    }
}

```

#### 3.3.3. Sử dụng CompletableFuture

```
public class UseCompletableFuture {
    public static void main(String[] args) throws InterruptedException {
        CompletableFuture<String> cf = CompletableFuture.supplyAsync(() -> {
            System.out.println("5......");
            return "test";
        });
        Thread.sleep(1000);
    }
}

```

#### 3.3.4. Sử dụng class Timer

```
public class UseTimer {
    public static void main(String[] args) {
        Timer timer = new Timer();

        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                System.out.println("9......");
            }
        }, 0, 1000);
    }
}

```

Java chỉ có một cách để tạo thread một cách trực tiếp, đó là thông qua việc tạo new Thread().start(). Do đó, cho dù sử dụng phương thức nào thì cuối cùng nó cũng phụ thuộc vào new Thread().start(). Các đối tượng Runnable, Callable, … chỉ là phần thân của Thread, tức là tác vụ được cung cấp cho Thread để thực thi.

## 4\. Trạng thái của thread

![thread-status.png](https://images.viblo.asia/full/9e0a1831-55d3-48d9-8f04-21ab8bb96b91.png)

Tại một thời điểm, một thread trong Java chỉ có thể ở một trong sáu trạng thái trong vòng đời của nó:

- `NEW`: Khi đối tượng thread được tạo, nó sẽ chuyển sang trạng thái NEW, chẳng hạn như: Thread t = new MyThread();
- `RUNNABLE`: Trạng thái sẵn sàng để chạy. Ta có thể hiểu, nó sẽ được chia thành 2 trường hợp nhỏ hơn: **đang chạy hoặc đang chờ để chạy**. Ví dụ, khi sau, ta gọi method start(), thread đó có thể chưa chạy được ngay mà phải đợi CPU schedule để chạy.
- `BLOCKED`: Trạng thái bị chặn, thread A đang cố giành khóa (lock) nhưng khoá đang giữa bởi thread B, thread A phải đợi, bị blocked cho đến khi khoá được giải phóng.
- `TIME_WAITING`: Trạng thái chờ có thời gian chờ, có thể tự động quay trở lại trạng thái RUNNABLE sau khoảng thời gian xác định.
- `WAITING`: Trạng thái chờ, biểu thị rằng thread A đang chờ các thread khác thực hiện một số hành động cụ thể, như (notification) thông báo cho thread A hoặc (interruption) ngắt thread A. Khác với TIME\_WAITING, trạng thái WAITING không có thời gian timeout, chỉ được wakeup khi có thông báo từ thread khác.
- `TERMINATED`: Trạng thái kết thúc, biểu thị rằng thread đã hoàn thành công việc hoặc dừng lai do gặp exception.

## 5\. Các method cơ bản của thread

### 5.1. start()

Method start() khởi tạo việc thực thi một thread. Nó gọi phương thức run() được xác định trong class thread hoặc runnable object. Thread sẽ chuyển từ trạng thái NEW sang trạng thái RUNNABLE sau khi method này được gọi.

```
public class Main {
    public static void main(String[] args) {
        Thread myThread = new Thread(new MyRunnable());
        myThread.start();
    }
}

```

### 5.2. run()

Method run() chứa mã sẽ được thực thi trong luồng.

```
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("This is a runnable.");
    }
}

```

### 5.3. sleep() và wait()

Method `sleep()` làm cho thread hiện đang thực thi ở chế độ ngủ (TIMED\_WAITING) trong 1 khoảng thời gian được chỉ định (tính bằng milliseconds).

Method wait() khiến thread hiện tại đợi cho đến khi một thread khác gọi notify() hoặc notifyAll() trên cùng một object. Thread sẽ chuyển từ trạng thái RUNNABLE sang trạng thái WAITING nếu dùng wait() không truyền thêm thời gian timeout, còn nếu truyền thêm thời gian timeout - wait(timeout) thì thread sẽ ở trạng thái `TIMED_WAITING`.

Sự khác biệt giữa 2 method:

- **Method wait() cần được đặt trong synchronized code, còn sleep() thì không.**
- Method sleep() không giải phóng khóa, trong khi method wait() sẽ giải phóng khóa.
- Method wait() thường được sử dụng cho tương tác/giao tiếp giữa các thread, còn sleep() thường được sử dụng để tạm dừng thực thi.
- Sau khi method wait() được gọi, thread sẽ không tự động thức dậy; cần một luồng khác gọi method notify() hoặc notifyAll() trên cùng một đối tượng để đánh thức luồng đó. Sau khi method sleep() được thực thi, thread sẽ tự động thức dậy (RUNNABLE).
- sleep() là một method static của class Thread, còn wait() là một method của class Object.

### 5.4. notify() và notifyAll()

notify(): đối với tất cả các thread đang chờ object monitor bằng cách sử dụng bất kỳ method wait() nào, method notify() thông báo cho một trong số các thread đó thức dậy. **Việc lựa chọn chính xác thread nào được đánh thức là mẫu nhiên và chúng ta không thể kiểm soát được** thread được đánh thức.
notifyAll(): Phương pháp này chỉ đơn giản đánh thức tất cả các thread đang chờ trên object monitor.
Mình sẽ nói chi tiết hơn về các method này trong bài giao tiếp giữa các threads.

### 5.5. yield()

Method yield() làm cho thread hiện đang thực thi tạm dừng và cho phép các thread khác thực thi.
Mọi người lưu ý, đây chỉ là hint cho scheduler tạm dừng thread, scheduler có thể bỏ qua cái hint này.
Method này có thể dùng để tái hiện bug do race condition. Tuy nhiên, method này hiếm khi được sử dụng và **mình recommend không dùng method này trong production code**.

### 5.6. join()

Method `join()` cho phép một thread chờ đợi một thread khác hoàn thành. Điều này có thể hữu ích khi bạn cần đảm bảo hoàn thành một số nhiệm vụ nhất định trước khi tiếp tục. Khi thread A gọi method `join()` của thread B, thread A sẽ chuyển sang trạng thái chờ ( `RUNNABLE → WAITING`). Nó vẫn ở trạng thái chờ cho đến khi thread B kết thúc.

Giả sử bạn cần thực hiện một số lệnh gọi API đến các endpoints khác nhau lấy dữ liệu đồng thời. Mỗi lệnh gọi API được thực hiện trong một thread riêng biệt và bạn muốn đợi cho đến khi tất cả các thread hoàn thành yêu cầu API của chúng trước khi tổng hợp (aggregate) kết quả.

```
String[] apiEndpoints = {
    "https://api.example.com/data1",
    "https://api.example.com/data2",
    "https://api.example.com/data3"
};

List<Thread> threads = new ArrayList<>();

List<String> results = new ArrayList<>();

for (String endpoint : apiEndpoints) {
    Thread thread = new Thread(() -> {
        String response = makeApiCall(endpoint);
        synchronized (results) {
            results.add(response);
        }
    });
    threads.add(thread);
    thread.start();
}

// Wait for all threads to complete
try {
    for (Thread thread : threads) {
        thread.join();
    }
} catch (InterruptedException e) {
    e.printStackTrace();
}

// Process and aggregate results
results.forEach(response -> System.out.println("API response: " + response));

```

Nếu mọi người thấy bài viết hữu ích thì nhờ mọi người share để nội dung của Ronin được nhiều người biết hơn.

Cám ơn mọi người. 🙏

* * *

🧑‍💻 150+ Ronin Engineers: [https://roninhub.com/](https://roninhub.com/)

✏️ System Design VN: [https://fb.com/groups/systemdesign.vn](https://fb.com/groups/systemdesign.vn)

📚 Tài liệu khác: [https://roninhub.com/tai-lieu](https://roninhub.com/tai-lieu)

🎬 Youtube: [https://youtube.com/@ronin-engineer](https://youtube.com/@ronin-engineer)





