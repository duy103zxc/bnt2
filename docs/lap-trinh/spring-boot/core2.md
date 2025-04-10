# Giải thích Dependency Injection (DI) và IoC

Nhưng không phải dùng quả minh họa của Loda =)).

### Dependency Injection (DI)

Trong tài liệu có nói thế này: *Dependency Injection is a design pattern,...*. Thế thì bạn có thể hiểu nôm na nó là một phương pháp lập trình. Dưới đây là ví dụ (Khum thích ví dụ của Loda nên viết lại chút):

```
public class BoDoi {
    private AK47 gun; // Mỗi chú bộ đội sẽ cầm AK47 khi đi ra ngoài chẳng hạn
    public BoDoi(){
      gun = new AK47(); // Khi anh bộ đội ra ngoài, anh sẽ vác theo khẩu AK47
    }
}
```

Trước hết, qua đoạn code này, bạn sẽ thấy là khi bạn tạo ra một anh `BoDoi`, bạn sẽ tạo ra thêm 1 khẩu `AK47` đi kèm với anh ấy. Lúc này,`AK47` tồn tại mang ý nghĩa là dependency (phụ thuộc) của `BoDoi`.

Khi khởi tạo thuộc tính như này, bạn vô tình tạo ra một điểm thắt nút trong chương trình của mình, giả sử, `BoDoi` muốn cầm M4A1 hay AWP hoặc cần gì cầm súng khi đi mua bia cơ chứ? Bạn sẽ phải thay class `AK47` thành `M4A1` hay `TayKhông` ư?

Hay nguy hiểm hơn, khẩu `AK47` bị hỏng? (code lớp `AK47` không hoạt động?) nó sẽ ảnh hưởng trực tiếp tới chú bộ đội `BoDoi`.

Vấn đề là ở đó, nguyên tắc là:

> Các Class không nên phụ thuộc vào các kế thừa cấp thấp, mà nên phụ thuộc vào Abstraction (lớp trừu tượng).

Nghe hơi khó hiểu. Bây giờ mình thay đoạn code như này:

```
// Một interface cho việc dùng súng
public interface EquipGun {
  public void equip();
}

// Một object cấp thấp, implement của EquipGun
public class AK47 implements EquipGun {
  public void equip() {
    System.out.println("Đã đeo AK47");
  }
}

// Bây giờ BoDoi chỉ phụ thuộc vào EquipGun. nếu muốn thay đổi súng của anh ấy, chúng ta chỉ cần cho EquipGun một thể hiện mới.

public class BoDoi {
    private EquipGun equipGun;
    public BoDoi() {
      equipGun = new AK47();
    }
}
```

Tới đây, chúng ta mới chỉ `Abtract` hóa thuộc tính của `BoDoi` mà thôi, còn thực tế, `BoDoi` vẫn đang bị gắn với một khẩu `AK47` duy nhất. Vậy muốn thay súng cho anh ấy, bạn phải làm như nào.

Phải sửa `code` thêm chút nữa:

```
public class BoDoi{
    private EquipGun gun;
    public BoDoi(EquipGun anything){
      this.gun = anything // Tạo ra một anh BoDoi, với súng tùy biến
      // Không bị phụ thuộc quá nhiều vào thời điểm khởi tạo, hay code.
    }
}

public class Main {
  public static void main(String[] args) {
    EquipGun AK47 = new AK47(); // Tạo ra đối tượng AK47 ở ngoài đối tượng
    BoDoi truongAnhNgoc = new BoDoi(AK47); // Cho anh đeo súng khi được khởi tạo
  }
}
```

Với đoạn code ở trên, chúng ta đã_gần như tách được `AK47` ra hoàn toàn khỏi `BoDoi`. điều này làm giảm sự phụ thuộc giữa`BoDoi`và`AK47`. Mà tăng tính tùy biến, linh hoạt cho`code`.

Bây giờ `BoDoi` sẽ hoạt động với `EquipGun` mà thôi. Và `EquipGun` ở đâu ra? Chúng ta tạo ra và đưa nó vào `(Inject)` anh `BoDoi`.

Khái niệm `Dependency Injection` từ đây mà ra~

> Dependency Injection là việc các Object nên phụ thuộc vào các Abstract Class và thể hiện chi tiết của nó sẽ được Inject vào đối tượng lúc runtime.

Bây giờ muốn `BoDoi` dùng súng gì khác, bạn chỉ cần tạo một Class kế thừa `EquipGun` và _Inject_ (dịch là _Tiêm vào_ cũng được) nó vào `BoDoi` là xong!

Các cách để _Inject dependency_ vào một đối tượng có thể kể đến như sau:

-  Constructor Injection: Cái này chính là ví dụ của mình, tiêm dependency ngay vào`Contructor` cho tiện.
-  Setter Injection:  Xài `BoDoi.setEquipGun(new P2000())` (Round đầu chỉ cần P2000 thôi)
-  Interface Injection : Mỗi `Class` muốn inject cái gì, thì phải *implement* một`Interface` có chứa một hàm `inject(xx)` (Gần như thay thế cho setter ý bạn). Rồi bạn muốn inject gì đó thì gọi cái hàm `inject(xx)` ra. Cách này hơi dài và khó cho người mới.

### Inversion of Control

`Dependency Injection` giúp chúng ta dễ dàng mở rộng `code` và giảm sự phụ thuộc giữa các dependency với nhau. Tuy nhiên, lúc này, khi code bạn sẽ phải kiêm thêm nhiệm vụ `Inject dependency (tiêm sự phụ thuộc)`. Thử tưởng tượng một `Class` có hàng chục dependency thì bạn sẽ phải tự tay inject từng ý cái. Việc này lại dẫn tới khó khăn trong việc code, quản lý code và dependency

```
public static void main(String[] args) {
    EquipGun AK47 = new AK47();
    AoChongDan gucci = new AoChongDan();
    MuBaoHiem muCoi = new MuBaoHiem();
    BoDoi truongAnhNgoc = new BoDoi(AK47, gucci, muCoi);
}
```

Bây giờ giả sử, chúng ta định nghĩa trước toàn bộ các `dependency` có trong Project, mô tả nó và tống nó vào 1 cái `kho` và giao cho một thằng tên là `framework` quản lý. Bất kỳ các `Class` nào khi khởi tạo, nó cần dependency gì, thì cái `framework` này sẽ tự tìm trong `kho` rồi _inject_ vào đối tượng thay chúng ta. sẽ tiện hơn phải không?

Và, đó cũng chính là nguyên lý chính của`Inversion of Control (IOC)`-`Đảo chiều sự điều khiển`

Nguyên văn Wiki:

> Inversion of Control is a programming principle. flow of control within the application is not controlled by the application itself, but rather by the underlying framework.

Khi đó, code chúng ta sẽ chỉ cần như này, để lấy ra 1 đối tượng:

```
@Override
public void run(String... args) throws Exception {
    BoDoi BoDoi = context.getBean(BoDoi.class);
}
```

Đối với `Java` thì có một số Framework hỗ trợ chúng ta`Inversion of Control (IOC)`. Thường thì dùng luôn Spring framework. 
