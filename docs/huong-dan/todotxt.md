Nguyên tắc đầu tiên và quan trọng nhất của todo.txt:

> Mỗi dòng trong tập tin todo.txt của bạn đại diện cho **một nhiệm vụ** duy nhất.

## Vì sao lại chọn dạng văn bản thuần (plain text)?

Văn bản thuần không phụ thuộc vào phần mềm hay hệ điều hành nào. Nó dễ tìm kiếm, dễ "mang đi", nhẹ, và cực kỳ dễ xử lý. Không có cấu trúc cứng nhắc.

Nó vẫn chạy ngon lành dù máy chủ web nào đó sập hay phần mềm bạn đang dùng ngừng hoạt động. Không cần lo xuất nhập dữ liệu, không cần cơ sở dữ liệu, không cần tag, flag hay *bạn đang dùng ứng dụng của tôi nên bạn phải theo luật chơi của tôi*.

## 3 tiêu chí của một danh sách công việc hiệu quả

Nhờ một số ký tự đặc biệt được sử dụng trong todo.txt, bạn có thể tạo ra một danh sách công việc dễ dàng được phân loại theo 3 tiêu chí quan trọng dưới đây:

### 1. Mức độ ưu tiên

Danh sách công việc nên cho bạn biết việc gì là quan trọng nhất cần làm tiếp theo – có thể theo dự án, theo hoàn cảnh hoặc tổng thể. Bạn có thể gán mức độ ưu tiên cho từng nhiệm vụ để có thể biết được mình cần làm gì bây giờ, các nhiệm vụ đó sẽ hiện lên ngay đầu danh sách.

### 2. Dự án

Để hoàn thành một dự án lớn, bạn cần giải quyết từng nhiệm vụ nhỏ cụ thể trong đó. `todo.txt` sẽ liệt kê rõ các công việc gắn với từng dự án.

Ví dụ, thay vì đặt nhiệm vụ là "Dọn nhà", tốt hơn nên ghi "Xử lý toàn bộ quần áo trong nhà" trong dự án "Dọn nhà".

### 3. Ngữ cảnh (Context)

Theo phương pháp [Getting Things Done (GTD)](https://vi.wikipedia.org/wiki/Getting_Things_Done) của David Allen, nên nhóm nhiệm vụ theo ngữ cảnh – tức là nơi chốn, tình huống bạn sẽ thực hiện công việc đó. 

- Việc cần gửi email thì gắn `@email`
- Việc cần gọi điện thì gắn `@phone`
- Việc trong nhà thì gắn `@home`, v.v.

Tất cả những điều này đều có thể thực hiện một cách gọn gàng trong `todo.txt`.

## Quy tắc định dạng của `todo.txt`

![](https://raw.githubusercontent.com/todotxt/todo.txt/refs/heads/master/description.svg)

`todo.txt` chỉ đơn giản là một tệp văn bản. Nhưng nếu bạn muốn tận dụng các thông tin như mức độ ưu tiên, dự án, ngữ cảnh, ngày tạo, ngày hoàn thành,... thì sẽ có một số quy tắc đơn giản và linh hoạt.

Định dạng tệp `todo.txt` cần đạt được hai tiêu chí sau:

- Bạn có thể đọc được nội dung tệp với bất kỳ công cụ xem/chỉnh sửa văn bản (text editor) nào.
- Người dùng có thể chỉnh sửa nội dung tệp sử dụng bất kì text editor nào. Ví dụ, text editor có thể sắp xếp các dòng theo thứ tự bảng chữ cái sẽ giúp sắp xếp các nhiệm vụ của bạn một cách có trật tự

Đó cũng là lý do:

- Dòng ghi công việc bắt đầu bằng mức độ ưu tiên hoặc ngày tháng, để dễ sắp xếp.
- Nhiệm vụ đã hoàn thành thì đánh dấu bằng `x`, vừa dễ nhìn như dấu tích checkbox, vừa tự động hiện xuống cuối danh sách khi sắp xếp các dòng theo ABC.

## Nhiệm vụ chưa hoàn thành: 3 quy tắc định dạng

Todo.txt hoàn toàn mở – bạn có thể thêm bao nhiêu thông tin tùy thích vào mỗi công việc. Nhưng để dễ dùng ngay từ đầu, bạn chỉ cần nhớ cách đánh dấu:

- **Ngữ cảnh:** dùng `@` (ví dụ: `@phone`)
- **Dự án:** dùng `+` (ví dụ: `+GarageSale`)
- **Mức ưu tiên:** dùng `(A)`, `(B)`, v.v.

Ví dụ file todo.txt:

```
(A) Gửi Email cho bạn về dự án sắp tới @phone
(B) Hoàn thiện bài tập Tiếng Anh chương 6 +TiengAnh @phone
(C) Viết bản báo cáo cho tiến độ công việc hôm nay +Report
@home Dọn bàn làm việc
```

Tìm theo ngữ cảnh `@phone` sẽ ra:

```
(A) Gửi Email cho bạn về dự án sắp tới @phone
(B) Hoàn thiện bài tập Tiếng Anh chương 6 +TiengAnh @phone
```

Tìm theo dự án `+TiengAnh` sẽ ra:

```
(B) Hoàn thiện bài tập Tiếng Anh chương 6 +TiengAnh @phone
```

### Quy tắc 1: Mức độ ưu tiên LUÔN đứng đầu dòng (nếu có).

Mức ưu tiên viết hoa (A–Z), trong ngoặc đơn, sau đó là dấu cách.

Đúng:

```
(A) Gọi điện cho mẹ
```

Sai:

```
Cần gọi mẹ gấp (A) @phone
(b) Trả lời sếp
(B)->Gửi báo cáo TPS
```

### Quy tắc 2: Ngày tạo (tùy chọn) đứng ngay sau mức ưu tiên.

Nếu không có mức ưu tiên, ngày tạo đứng đầu dòng. Định dạng ngày là `YYYY-MM-DD` (Năm-Tháng-Ngày, kiểu 2011-03-02).

Đúng:

```
2011-03-02 Viết tài liệu +TodoTxt
(A) 2011-03-02 Gọi điện cho mẹ
```

Chưa đúng

```
(A) Gọi điện cho mẹ 2011-03-02
```

### Quy tắc 3: Ngữ cảnh và Dự án có thể xuất hiện bất kỳ đâu, miễn là đứng sau mức ưu tiên/ngày tạo.

- **Ngữ cảnh** bắt đầu bằng khoảng trắng và `@`
- **Dự án** bắt đầu bằng khoảng trắng và `+`
- Có thể có nhiều ngữ cảnh/dự án trong cùng một nhiệm vụ, hoặc không có cái nào.

Ví dụ:

```
(A) Gọi cho mẹ +Gia_Đình @iphone @phone
```

Không có ngữ cảnh:

```
Gửi email soandso@example.com
```

Không có dự án:

```
Tìm hiểu phép cộng 2+2
```


## Nhiệm vụ đã hoàn thành: 2 quy tắc định dạng

Có 2 dấu hiệu cho thấy một công việc đã hoàn tất:

### Quy tắc 1: Bắt đầu bằng chữ `x` thường + dấu cách.

Đúng:

```
x 2011-03-03 Gọi điện cho mẹ
```

Sai:

```
Đi học đàn
X 2012-01-01 Đặt mục tiêu năm mới
(A) x Tìm giá vé
```

Dùng `x` viết thường để các nhiệm vụ đã hoàn thành tự động được chuyển xuống cuối danh sách khi sắp xếp theo ABC

### Quy tắc 2: Ngày hoàn thành đứng ngay sau `x`.

Ví dụ:

```
x 2011-03-02 2011-03-01 Duyệt pull request của Tim +TodoTxtTouch @github
```

Nếu nhiệm vụ có ngày tạo, thì khi hoàn thành, dòng sẽ ghi ngày hoàn thành trước, ngày tạo sau. Nhờ vậy, có thể dễ dàng tính được mất bao nhiêu ngày để hoàn tất một công việc.