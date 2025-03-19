# Những điều rất chi là cơ bản về React

React (Công cụ) và Vite (Build tool)

Sử dụng lệnh `npm create vite@latest [introdemo - cái này là cái tên dự án của mình, để là gì cũng được] -- --template react` để tạo ứng dụng React mới.

### Component (Thành phần)

Component là khái niệm cốt lõi trong React, được định nghĩa như các hàm JavaScript.

```javascript
// Ví dụ, `App.jsx` định nghĩa component `App`, trả về JSX (JavaScript XML) để hiển thị nội dung.

const App = () => {
  return (
    <div>
      <p>Hello world</p>
    </div>
  )
}

export default App

// Ở một tệp khác
import ReactDOM from 'react-dom/client'

import App from './App'

// Được dùng để render nội dung của component App vào div có id là root, được định nghĩa trong file index.html.
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

Hãy luôn mở console của trình duyệt để theo dõi lỗi và debug.

### JSX

JSX là cú pháp giống HTML, nhưng thực chất là JavaScript được biên dịch thành các lệnh `React.createElement`. JSX cho phép nhúng nội dung động bằng cách sử dụng dấu ngoặc nhọn `{}`. 

Mọi thẻ JSX đều phải được đóng.

Ví dụ: `<Hello name='George' />`, chứ không phải `<Hello name='George'>`.

### Multiple Components (Nhiều thành phần)

Có thể định nghĩa nhiều component và sử dụng chúng trong các component khác. Ví dụ, component `Hello` được sử dụng trong component `App`.

```javascript
const App = () => {
  return (
    <div>
      <h1>Greetings</h1>
      <Hello />
      <Hello />      
      <Hello />    
    </div>
  )
}
```

### Props (Thuộc tính)

Props được sử dụng để truyền dữ liệu từ component cha xuống component con. Props được truyền dưới dạng attribute (thuộc tính) của thẻ JSX.

```javascript
const Hello = (props) => {  
    return (
        <div>
        <p>Hello {props.name}</p>    
        </div>
    )
}

// Ở một tệp khác
const App = () => {
  return (
    <div>
      <h1>Greetings</h1>
      <Hello name='George' />      
      <Hello name='Daisy' />    
    </div>
  )
}

```

Ví dụ, `<Hello name='George' />` truyền prop `name` với giá trị 'George' cho component `Hello`.
* Giá trị của props có thể là chuỗi, số, hoặc kết quả của biểu thức JavaScript (được đặt trong dấu ngoặc nhọn `{}`).

### Lỗi và Gỡ Lỗi

Luôn mở console để theo dõi lỗi và React cung cấp thông báo lỗi rõ ràng (chắc thế, dù đọc đa phần đều quá dài và khó hiểu).

Tên component React phải bắt đầu bằng chữ cái viết hoa

```javascript
// Đừng để tên Component không viết hoa thế này
const footer = () => {
  return (
    <div>
      greeting app created by <a href='https://github.com/mluukkai'>mluukkai</a>
    </div>
  )
}

// Đây
const App = () => {
  return (
    <div>
      <h1>Greetings</h1>
      <Hello name='Maya' age={26 + 10} />
      <footer />   
    </div>
  )
}
```


Nội dung của component React thường phải có một phần tử gốc (root element) duy nhất. Có thể sử dụng fragments `<> </>` để tránh các phần tử div thừa trong DOM.

```javascript
const App = () => {
  const name = 'Peter'
  const age = 10

  return (
    <>
      <h1>Greetings</h1>
      <Hello name='Maya' age={26 + 10} />
      <Hello name={name} age={age} />
      <Footer />
    </>
  )
}
```
Không render objects trực tiếp trong JSX, mà cần truy xuất từng thuộc tính của object. Thay vì `{friends[0]}` , sử dụng `{friends[0].name} {friends[0].age}` (The core of the problem is Objects are not valid as a React child, i.e. the application tries to render objects and it fails again).


React cũng cho phép render mảng chứa các giá trị primitive. Ví dụ như này

```javascript
const App = () => {
  const friends = [ 'Peter', 'Maya']

  return (
    <div>
      <p>{friends}</p>
    </div>
  )
}
```

Xóa lỗi trong console bằng cách bấm 🚫 và tải lại trang.

### Các điểm cần nhớ

* Luôn mở console.
* Chia nhỏ ứng dụng thành các component nhỏ, tái sử dụng.
* Sử dụng props để truyền dữ liệu giữa các component.
* Tuân thủ quy tắc đặt tên component.
* Sử dụng fragments để tránh các phần tử div thừa.
* Không render object trực tiếp.
* Luôn xóa lỗi trong console sau khi sửa.


Làm bài tập [tại đây](https://fullstackopen.com/en/part1/introduction_to_react#exercises-1-1-1-2)