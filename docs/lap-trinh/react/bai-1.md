# Khởi động chút React

Theo như trang hướng dẫn nói thì cái trang này sẽ bao gồm khoảng 80% những gì mà bạn sẽ dùng trong React hàng ngày. Gọi bài này là dịch thì *vừa đúng vừa không đúng* vì mình đã thêm bớt khá nhiều sao cho hợp với cái hiểu của mình. Nhưng cấu trúc từng phần về cơ bản vẫn vậy.

Mình nghĩ là các nguyên lý cơ bản sẽ không dễ bị thay đổi quá nhanh cho nên là chắc để vậy cũng oke.

### Tạo và nest các components

Về cơ bản thì ứng dụng React toàn là *components* (A component is a piece of the UI (user interface) that has its own logic and appearance. A component can be as small as a button, or as large as an entire page). React components về cơ bản là JavaScript functions, nhưng trả về Markup:

```js
function MyButton() {
  return (<button>I'm a button</button>);
}
// Đã khai báo `MyButton` rồi nên có thể dùng trong Component khác, kiểu như này:
export default function MyApp() {
  return (<div><h1>Welcome to my app</h1><MyButton /></div>);
}
```

Nhớ là `<MyButton />` bắt đầu bằng chữ cái in hoa để nhận diện component, còn in thường là cho HTML tag. 

### Viết markup bằng JSX

Cú pháp markup được dùng ở đây là *JSX* (Dù nó là *optional*, nhưng mấy ông dev bảo chẳng ai khổ dâm mà không dùng *JSX* cả)

JSX thì cứng nhắc hơn HTML. Ví dụ như cần phải đóng tag `<br />`. Và cũng chỉ được trả một JSX tag. Hoặc là gói trong một *shared parent*, kiểu như `<div>...</div>` hoặc `<>...</>` 

```
function AboutPage() {return (<><h1>About</h1><p>Hello there.<br />How do you do?</p></>);}
```

If you have a lot of HTML to port to JSX, you can use an [online converter](https://transform.tools/html-to-jsx)

### Thêm chút kiểu cách (styles)

Trong React thì khai báo CSS class bằng `className`. Hoạt động tương đương thuộc tính [`class`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/class) của HTML:

```html
<img className="avatar" />
```

Rồi viết quy tắc CSS sang một tệp khác

```css
/* In your CSS */
.avatar {border-radius: 50%;}
```

Thêm CSS thì tương tự như bình thường, có thể là thêm tag [`<link>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) vào tệp HTML.

### Hiện data

Sử dụng cặp ngoặc nhọn để thêm chút JavaScript vào `{}` ví dụ như `user.name`:

```
return (<h1>{user.name}</h1>);
```

Cũng có thể làm tương tự trong các thuộc tính của "tag" JSX. Ví dụ như `src={user.imageUrl}` sẽ đọc biến `user.imageUrl` trong JavaScript, và đưa vào thuộc tính `src`:

```
return (<imgclassName="avatar"src={user.imageUrl}/>);
```

<!-- In the above example, `style={{}}` is not a special syntax, but a regular `{}` object inside the `style={ }` JSX curly braces. You can use the `style` attribute when your styles depend on JavaScript variables. -->

### Conditional rendering

Về cơ bản thì dùng như viết JavaScript, như đoạn code dưới đây:

```js
let doAn;
if (duTienMua) {
  doAn = <BanhMi />;
} else {
  doAn = <LamGiCoGiMaAn />; 
}
return (
  <div>
    {doAn}
  </div>
);
```
Hoặc đơn giản hơn thì dùng Elvis operator
```
<div>{duTienMua ? (<BanhMi />) : (<LamGiCoGiMaAn />)}</div>
// Nếu không cần `else` thì gọn nữa
<div>{duTienMua && <BanhMi />}</div>
```

### Rendering lists

Dùng các chức năng của bên JS [`for` loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) hoặc [array `map()` function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) để render list components.


```js
const products = [
  { title: 'Cabbage', id: 1 },
  { title: 'Garlic', id: 2 },
  { title: 'Apple', id: 3 }
];
// Inside your component, use the `map()` function to transform an array of products into an array of `<li>` items:
const listItems = products.map(
  product =>
    <li key={product.id}>{product.title}</li>
);
return (<ul>{listItems}</ul>);

```

Để ý là `<li>` có thuộc tính `key` sử dụng để nhận diện giữa các Item với nhau.

### Responding to events

Khai báo *event handler* function trong component:

```js
function MyButton() {
  function handleClick() {
    alert('Mày bấm cái nút này xem');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

`onClick={handleClick}` không sử dụng dấu `()` nha, không phải là `onClick={handleClick()}` vì chúng ta đang coi `handleClick()` là một argument của `onClick` chứ không phải để gọi. 

### Cập nhật màn hình

Often, you'll want your component to "remember" some information and display it. For example, maybe you want to count the number of times a button is clicked. To do this, add *state* to your component.

Đầu tiên hãy import [`useState`](https://react.dev/reference/react/useState).

```
import { useState } from 'react';
```

Now you can declare a *state variable* inside your component:

```js
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
```

Bạn sẽ nhận được hai thứ từ `useState`: state (trạng thái) hiện tại (`count`), và function để update cái state đấy (`setCount`). Theo convention (Cách mà mọi người thường làm, mà nó tiện và đúng nhá) `[something, setSomething]`.

Xem thêm phần ví dụ chạy phần Code cho phần này trong [Quick Start](https://react.dev/learn#responding-to-events)

### Dùng Hooks

Functions bắt đầu bằng `use` được gọi *Hooks*, ví dụ như `useState`. Đọc danh sách các đồ đã có sẵn[API reference.](https://react.dev/reference/react), hoặc tự tạo cũng được.

Hooks are more restrictive than other functions. You can only call Hooks *at the top* of your components (or other Hooks). If you want to use `useState` in a condition or a loop, extract a new component and put it there.

Sẽ cần nói thêm về Hook, nhưng chắc để sau.

### Chia sẻ data giữa các components

Thường thì như bạn có thể đã biết, mỗi một Component đều có cho mình một State riêng. Nhưng đôi khi, bạn cần các components share data và luôn cập nhật (giống nhau).