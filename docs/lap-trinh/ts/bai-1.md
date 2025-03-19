# Cơ bản về TypeScript

> Bài viết gốc: [Tìm Hiểu TypeScript và Kiến Thức Cơ Bản - Viblo](https://viblo.asia/p/tim-hieu-typescript-va-kien-thuc-co-ban-PmeRQpnyGoB)

### Cài đặt
-------

1.  Chạy lệnh sau để install TypeScript (nhớ install nodejs trước):

```
npm install -g typescript

```

1.  Compile:

Việc này sẽ giúp tạo ra file xxx.js

### Basic Types:

Trong TypeScript chia ra làm 7 loại cơ bản, bao gồm: boolean, number, string, array, enum, any, void. khi khai báo ta sẽ sử dụng cấu trúc như sau: var tên_biến : kiểu_trả_về = giá_trị_biến;

-   Boolean:

```
var isDone: boolean = true;

```

-   String:

```
var name: boolean = "nguyen thi A";

```

-   Number:

-   Array : có 2 kiểu khai báo tương đương với nhau trong TypeScript

```
1: var list: boolean[] = [true, false];
2: var isDone: Array<boolean> = [true, false];

```

-   Enum: khi khai báo enum một cách thông thường các phần tử sẽ được đánh số từ 0 và tăng dần

```
enum Color{Red, Green, Blue};
var c: Color = Color.Green
var colorName = Color[1] // kết quả sẽ là Green

```

Khi mốn phần tử đầu tiên là 1 chứ không phải là 0 như mặc định thì cần khai báo như sau:

```
enum Color{Red = 1, Green, Blue};
var c: Color = Color.Green
var colorName = Color[1] // kết quả sẽ là Red

```

-   Any: Any là một kiểu mà bạn có thể gán bất kỳ kiểu nào cho nó.

```
var notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // khai báo này hoàn toàn được chấp nhận.
                 // nếu notSure ban đầu khai báo và number thì
                 // tại đây chắc chắn sẽ có lỗi

var list:any[] = [1, true, "free"]; // nếu sử dụng var list:number[] thì
                                    // tất cả các phần tử trong list sẽ phải là kiểu number
list[1] = 100;

```

-   Void: Cũng giống như any nhưng void được sử dụng là đầu ra của hàm.

```
function warnUser(): void {
 alert("This is my warning message");
}

```

### Function:

Cũng giống như javaScript, typeScript có 2 cách khai báo function

```
//Named function
function add(x, y) {
    return x+y;
}

//Anonymous function
var myAdd = function(x, y) { return x+y; };

```

Nhưng khi khai báo function typeScript còn hỗ chợ việc khai báo với các kiểu trả ra của function và cũng như kiểu đầu vào của dữ liệu

```
function add(x: number, y: number): number {
    return x+y;
}

var myAdd = function(x: number, y: number): number { return x+y; };

```

Không những thế khi sử dụng typeScript ta có thể khai báo giá trị mặc định của đầu vào ngay khi khai báo function, điều mà JavaScript không có

```
function buildName(firstName: string, lastName = "Smith") {
    return firstName + " " + lastName;
}

var result1 = buildName("Bob");  //làm việc hoàn toàn OK. buildName("bob") = "bob Smith"
var result2 = buildName("Bob", "Adams", "Sr.");  //error, too many parameters
var result3 = buildName("Bob", "Adams");  //ah, just right

```

-   Không dừng lại ở đó typeScript còn hỗ chợ việc bỏ qua nhập một hoặc vài đầu vào.

```
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}

var result1 = buildName("Bob");  //làm việc hoàn toàn OK. buildName("bob") = "bob"
var result2 = buildName("Bob", "Adams", "Sr.");  //error, too many parameters
var result3 = buildName("Bob", "Adams");  //ah, just right

```

ở ví dụ trên việc khai báo biến lastName? làm cho viêc nhập lastName có thể không cần nữa và kết quả trả ra chỉ là bob mà thôi. typeScript còn có một cách làm việc với các param đầu vào có tên gọi: "Rest Parameters"

```
function buildName(firstName: string, ...restOfName: string[]) {
	return firstName + " " + restOfName.join(" ");
}

var employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");// => "Joseph Samuel Lucas MacKinzie"

```

ở trên firstName sẽ là bắt buộc phải nhập. còn các thành phần còn lại sẽ được gộp chung vào một biến array.

### Class:

```
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}
var greeter = new Greeter("world");

```

hàm constructor sẽ được chạy ngay khi khởi tạo class mới. ờ ví dụ trên khi khai báo new Greeter("world") thì việc đâu tiên sẽ là chạy hàm constructor gán message "world" vào biến greeting của class. Hơn nữa, cũng giống như các ngôn ngữ lập trình hướng đối tượng khác. chúng ta cũng có thể dễ dàng sử dụng kế thừa trong typeScript.

```
class Animal {
    name: string;
    constructor(theName: string) {
        this.name = theName;
    }

    move(meters: number = 0): string {
        return this.name + " moved " + meters + "m.";
    }
}

class Snake extends Animal {
    constructor(name: string) {
        super(name);
    }

    move(meters = 5): string {
        return super.move(meters);
    }
}

var ranLuc = new Snake("Ran Luc");
ranLuc.move();   // = Ran Luc moved 5 m.
ranLuc.move(34); // = Ran Luc moved 34 m.

```

Ở đây chúng ta sẽ có class Animal bao gồm có biến name chưa tên, và function move chỉ sự di chuyển của động vật ý. Class Snake kế thừa lại class Animal. Trong hàm khởi tạo của class Snake ta đã sử dụng super(name), việc này chính là việc gọi tới hàm constructor của class cha. Tương tự tại hàm move việc gọi super.move(meters) chính là gọi thới hàm move của class cha (Animal).