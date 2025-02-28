# Ôn tập lại cơ bản các khái niệm
Phần này là phần ghi chú cho bài [Cùng ôn lại các khái niệm về Cấu trúc dữ liệu, Giải thuật, Độ phức tạp thuật toán](https://viblo.asia/p/cung-on-lai-cac-khai-niem-ve-cau-truc-du-lieu-giai-thuat-do-phuc-tap-thuat-toan-Eb85oVy6l2G) trên Viblo.

## Thuật toán là gì?

Định nghĩa của [Robert Sedgewick](https://en.wikipedia.org/wiki/Robert_Sedgewick_(computer_scientist)) trong cuốn sách kinh điển [Algorithms](https://www.amazon.com/Algorithms-4th-Robert-Sedgewick/dp/032157351X) của ông:

> When we write a computer program, we are generally implementing a method that has been devised previously to solve some problem. This method is often independent of the particular programming language being used—it is likely to be equally appropriate for many computers and many programming languages. It is the method, rather than the computer program itself, that specifies the steps that we can take to solve the problem. The term algorithm is used in computer science to describe a finite, deterministic, and effective problem-solving method suitable for implementation as a computer program. Algorithms are the stuff of computer science: they are central objects of study in the field.

## Cấu trúc dữ liệu là gì?

> Algorithms + Data Structures = Programs

Tức **Giải thuật + Cấu trúc dữ liệu = Chương trình máy tính**.

![](https://images.viblo.asia/810d24dd-56fe-40f0-9ae7-649497da42bb.jpg)

Về cơ bản thì **giải thuật** phải ánh phương pháp xử lý vấn đề, hay cụ thể là các phép tính toán, còn đối tượng để tính toán bởi máy tính chính là **dữ liệu**. Dữ liệu biểu diễn các thông tin của bài toán, từ thông tin đầu vào, cho đến kết quả đầu ra. Dữ liệu có thể ở các dạng đơn giản như số nguyên, số thực, ký tự, boolean … Hoặc dữ liệu cũng có thể có cấu trúc phức tạp, gồm nhiều thành phần dữ liệu được liên kết với nhau theo một cách nào đó.

Các kiểu dữ liệu được tạo thành từ nhiều kiểu dữ liệu khác được gọi là kiểu dữ liệu có cấu trúc. Và trong khoa học máy tính, việc nghiên cứu về cấu trúc dữ liệu là nghiên cứu về cách lưu trữ và tổ chức dữ liệu sao cho chúng có thể được sử dụng một cách hiệu quả.

Một số cấu trúc dữ liệu (data structure) cơ bản, thường được sử dụng nhiều trong các chương trình máy tính (và thường được hỏi nhiều trong các buổi Coding Interview nữa) có thể kể ra là:

- Mảng (array)
- Danh sách liên kết (linked list)
- Ngăn xếp (stack)
- Hàng đợi (queue)
- Bảng băm (hash table)
- Cây (tree)
- Đống (heap)
- Đồ thị (graph)

Trong những bài tiếp theo, khi đề cập đến một số giải thuật, với ví dụ cụ thể, chúng ta sẽ dần ôn lại về các kiến thức về một số cấu trúc dữ liệu ở trên.

## Đánh giá thuật toán?

### Độ phức tạp của thuật toán (Complexity)

**Độ hiệu quả của thuật toán** có thể xét trên 2 phương diện:

- Hiệu quả về mặt thời gian (chạy nhanh)
- Hiệu quả về mặt bộ nhớ (dung lượng bộ nhớ cần dùng để lưu dữ liệu vào, dữ liệu ra, và các kết quả trung gian khi thực hiện thuật toán).

Dựa trên hai yếu tố

- Liên quan đến dung lượng bộ nhớ mà thuật toán đòi hỏi, người ta sử dụng một thước đo để đánh giá, gọi là **độ phức tạp không gian (Space Complexity)** của thuật toán. Còn để 
- Đánh giá thời gian chạy của thuật toán, người ta dùng khái niệm **độ phức tạp thời gian (Time Complexity)**.

Ngày nay thì ta thường chú trọng đến Time Complexity hơn là Space Complexity.

Ngoài ra, khi đánh giá về độ phức tạp của thuật toán, người ta cũng thường xét nó trong các trường hợp nhất định là: **best case** (trường hợp tốt nhất), **worst case** (trường hợp tồi nhất) và **average case** (trường hợp trung bình). Các trường hợp này xảy ra có thể tùy thuộc vào tính chất, hay cách tổ chức của dữ liệu đầu vào (ví dụ như với bài toán sắp xếp một dãy từ nhỏ đến lớn thì ngay từ đầu dữ liệu đầu vào đã được sắp xếp luôn từ nhỏ đến lớn, hoặc được sắp xếp luôn từ lớn đến nhỏ chẳng hạn). Thường thì trong thực tế chúng ta khó có thể kỳ vọng vào trường hợp tốt nhất (hay xấu nhất) sẽ xảy ra thường xuyên được, thế nên về cơ bản thì average-case complexity là yếu tố tốt hơn để đánh giá về hiệu quả của thuật toán. Và khi không nhắc đến trường hợp nào đặc biệt, thì chúng ta ngầm hiểu chúng ta đang nhắc đến Space Complexity hay Time Complexity ở average case.

### Khái niệm O lớn

Đương nhiên là khi so sánh hiệu quả về mặt thời gian giữa các thuật toán thì chúng ta không thể so một thuật toán được cài đặt bằng ngôn ngữ lập trình C, chạy trên máy tính với CPU là core i9 đời mới, xung nhịp một nhân là 4GHz, 64GB RAM, với một thuật toán được cài đặt bằng Ruby, chạy trên máy tính với chip i3 đời cũ, 4GB RAM được. Trong khoa học máy tính, chúng ta có một cách tiếp cận để có thể giúp kết luận về thời gian chạy của một thuật toán mà nó không phụ thuộc vào cách thức thuật toán được cài đặt, ngôn ngữ lập trình được sử dụng, hay môi trường máy tính mà trên đó thuật toán được chạy. Cách thức đó được tạo ra trên nền tảng toán học. Chúng ta thường dùng đến một khái niệm gọi là **O Lớn** (Big O)

Ta nói f(n)=O(g(n))f(n) = O(g(n)), với f(n)f(n) và g(n)g(n) là các hàm thực không âm của đối số nguyên dương nn, nếu tồn tại hằng số dương CC và n0n\_0 sao cho f(n)<=Cg(n)f(n) <= Cg(n) với mọi n>=n0n >= n\_0. Điều đó đồng nghĩa với việc hàm f(n)f(n) bị chặn trên bởi hàm g(n)g(n) với một hằng số CC nào đó (đem nhân với g(n)g(n)), khi nn đủ lớn.

Trong lĩnh vực khoa học máy tính, thì O lớn có thể được định nghĩa là:

> Ký hiệu O lớn dùng để mô tả hành vi thuật toán, về mặt thời gian tính toán hoặc lượng bộ nhớ cần dùng, khi kích thước dữ liệu thay đổi. Ký hiệu O lớn mô tả các hàm theo tốc độ tăng của chúng: các hàm khác nhau có cùng tốc độ tăng có thể được mô tả bởi cùng một ký hiệu O lớn.

Tức về cơ bản thì O lớn giúp chúng ta có một cái nhìn toàn cảnh về việc khi lượng dữ liệu đầu vào tăng lên, thì thời gian chạy của thuật toán sẽ thay đổi ra sao. Mô tả hàm bằng ký hiệu O lớn thường chỉ cung cấp cho chúng ta **một chặn trên cho tốc độ tăng của hàm** (chứ nó không cho ra một con số cụ thể).

### Một số hàm O lớn thường gặp

Khi đánh giá thời gian chạy của thuật toán bằng khái niệm O lớn, chúng ta thường biểu diễn nó dưới dạng O(f(n))O(f(n)), với f(n)f(n) là một cận trên của thời gian chạy, và không thể tìm được một hàm g(n)g(n) nào khác cũng là cận trên của thời gian chạy, mà lại tăng chậm hơn hàm f(n)f(n).

Dưới đây là một số hàm O lớn chúng ta thường gặp khi phân tích về thuật toán, được sắp xếp theo thứ tự tốt dần:

- O(1)O(1): Tức thời gian chạy của thuật toán bị chặn trên bởi một hằng số nào đó, hay nói cách khác thì thuật toán có thời gian chạy là hằng, và không phụ thuộc vào kích thước dữ liệu. Ví dụ như phép insert trong Stack hoặc Queue, hay phép search với cấu trúc Hash Table có độ phức tạp là O(1)O(1)
- O(logn)O(logn): Ví dụ như phép search trong cây nhị phân tìm kiếm (Binary Search Tree)
- O(n)O(n): Tức thời gian chạy của thuật toán bị chặn trên bởi một hàm tuyến tính, hay ta nói thời gian chạy của thuật toán là tuyến tính. Ví dụ như phép tìm kiếm trong một mảng.
- O(nlogn)O(nlogn): Ví dụ như các thuật toán Quicksort, Mergesort
- O(n2)O(n^2): Ví dụ như các thuật toán Buble sort, Selection sort, Insertion sort. Hay Quicksort ở worst case.
- O(n3)O(n^3)
- O(2n)O(2^n)
- O(n!)O(n!)

![big o complexity](https://images.viblo.asia/1dd9e734-5096-4571-9c56-a0842434e895.png)




