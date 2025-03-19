# Component state, event handlers

### Component Helper Functions (Hàm hỗ trợ component)

* Component có thể chứa các hàm hỗ trợ để xử lý logic, ví dụ như tính toán năm sinh dựa trên tuổi.
* Các hàm này có thể truy cập trực tiếp các `props` được truyền vào component.
* Trong JavaScript, việc định nghĩa hàm bên trong hàm khác là phổ biến.

### Destructuring (Phân rã cấu trúc)

* Destructuring cho phép trích xuất giá trị từ objects và arrays vào các biến riêng biệt.
* Ví dụ, `const { name, age } = props` trích xuất `name` và `age` từ object `props`.
* Có thể destructure `props` trực tiếp trong tham số của component: `const Hello = ({ name, age }) => {...}`.
* Destructuring giúp code ngắn gọn và dễ đọc hơn.

### Page Re-rendering (Kết xuất lại trang)

* Để cập nhật giao diện khi dữ liệu thay đổi, cần kết xuất lại (re-render) component.
* Gọi `ReactDOM.createRoot(document.getElementById('root')).render(...)` để kết xuất lại component.
* Sử dụng `setInterval` để tự động re-render component sau một khoảng thời gian nhất định.
* Việc gọi liên tục hàm render không phải là cách được khuyến khích.

### Stateful Component (Component có trạng thái)

* `useState` hook được sử dụng để thêm trạng thái (state) vào component.
* `useState` trả về một mảng gồm hai phần tử: giá trị trạng thái hiện tại và hàm để cập nhật trạng thái.
* Khi hàm cập nhật trạng thái (ví dụ, `setCounter`) được gọi, React sẽ re-render component.
* Sử dụng `console.log` để debug và theo dõi quá trình re-render.

### Event Handling (Xử lý sự kiện)

* Event handlers được gọi khi một sự kiện cụ thể xảy ra (ví dụ, click button).
* Sử dụng thuộc tính `onClick` để đăng ký event handler cho button.
* Event handler có thể được định nghĩa trực tiếp trong JSX hoặc dưới dạng hàm riêng biệt.
* Event handler phải là một hàm hoặc tham chiếu đến một hàm.
* Việc gọi trực tiếp hàm trong `onClick` sẽ gây ra lỗi.
* Tách event handler vào các hàm riêng biệt giúp code dễ đọc và bảo trì hơn.

### Passing State - to Child Components (Truyền trạng thái - cho component con)

* Nên chia ứng dụng thành các component nhỏ, tái sử dụng.
* "Lift state up" (nâng trạng thái lên) bằng cách đặt trạng thái ở component cha và truyền xuống component con qua `props`.
* Component con sẽ re-render khi component cha re-render.
* Đặt tên `props` cho event handler theo quy ước `onSomething` và tên hàm event handler là `handleSomething`.

### Changes in State Cause Re-rendering (Thay đổi trạng thái gây ra re-rendering)

Khi state (trạng thái) thay đổi, component sẽ re-render và các component con cũng sẽ re-render theo.

> Mẹo vặt cuộc sống: Sử dụng `console.log` để theo dõi giá trị trạng thái và quá trình re-render. Không nên đoán code chạy như thế nào, hãy dùng console.log để quan sát.

### Refactoring the Components (Tái cấu trúc các component)

* Sử dụng destructuring để đơn giản hóa component.
* Sử dụng cú pháp arrow function ngắn gọn khi hàm chỉ có một câu lệnh `return`.
* Tái cấu trúc component giúp code ngắn gọn và dễ đọc hơn.
