# Interview

## Why react use immutability?

- Avoiding direct data mutation lets us keep previous versions of the state, and reuse them later
- Create pure components. Immutable data can determine if changes have been made, which helps to determine when a component require re-rerendering.
If you mutate the data directly, the components might not re-render that could lead to bugs.

## How React rendered UI

Problem: DOM manipulation is slow. Let 's you have a list of items. If you want to modify that item you need to select that item and re-render it and re-paint the UI. Therefore, the more UI components you have, the more expensive the DOM updates could be, since they would need to be re-rendered for every DOM update.

To address this problem, React use virtual DOM. A virtual DOM is a representation of DOM object and synced with the real DOM.

React sử dụng sử dụng ReactDOM.render() và dùng jsx (React.createElement) để render virtual DOM ban đầu.

## How React update UI

Once virtual DOM has updated, React compares the entire virtual DOM with a virtual DOM snapshot that was taken right before the update.
By comparing, React knows exactly which virtual DOM **object** (component) has changed. If there are a lot of updates, React can batch them (collect the updates) for efficency.
Then real DOM is updated (this process is commit)


## Why list item need a unique `key`

<https://kentcdodds.com/blog/understanding-reacts-key-prop>
Để React có thể theo dõi (track) được item nào bị xóa, thêm hay bị sửa. Key của mỗi item phải là duy nhất và không thay đổi. Nếu lấy key là index thì khi thêm, hoặc xóa item, index bị thay đổi => render sai

When the `key` props is changed, React unmount the previous instance, and mount a new one. This means that all state that had existed in the component at the time is completely removed and the component is reinitialized
Khi 1 component bất kì có props key, khi key này bị thay đổi thì component sẽ bị unmount, state của nó cũng sẽ bị xóa bỏ và React sẽ mount lại 1 component mới

## Optimize React re-render

<https://kentcdodds.com/blog/optimize-react-re-renders>
Mỗi khi render 1 component nào đó, props object của component đó sẽ được làm mới hoàn toàn, kể cả khi property của prop objects đó không đổi

## Event, Event handler, Event listener

**Event** là 1 hành động, 1 sự kiện nào đó khi user tương tác: click chuột vào 1 phần tử, submit 1 form nào đó
**Event handler** là 1 đoạn code, 1 hàm được chạy khi event xảy ra
**Event handler** còn được gọi là ***event listener***

## Debounce và throttle

- Debounce: gọi 1 hàm nào đó sau 1 khoảng delay. VD: Có 1 input. Input này có 1 event handler cho sự kiện onChange. Nếu áp dụng debounce, event handler này sẽ chỉ chạy khi:
  - Hết thời gian delay
  - Sự kiện đó không xảy ra nữa. Nếu sự kiện đó xảy ra trong khoảng thời gian delay thì reset lại thời gian delay
- Throttle: đảm bảo 1 hàm chỉ chạy 1 lần trong 1 khoảng thời gian (vd: 10s).
Ví dụ event scroll với 1 event listener .on('scroll') và hàm f, trong điều kiện bình thường hàm f sẽ được gọi 1000 lần trong 10 giây nếu ta scroll liên tục và không có can thiệp gì. Nếu ta sử dụng throttle, cho phép event listener kích hoạt mỗi 100 mili giây, thì hàm f sẽ chỉ được gọi 100 lần là nhiều nhất trong vòng 10 giây bạn scroll

## So sánh display: flex và display: grid

- Grid: hiển thị layout theo 2 chiều
- Flex: hiển thị layout theo 1 chiều

## Kiến thức bảo mật tối thiểu ở frontend

## getter, setter trong javascript. Có ưu điểm gì so với hàm bình thường

## Animation có những thuộc tính gì

## Hiểu thế nào về responsive web design

Đó là việc 1 website có thể hiển thị và tương tác (UI và UX) tốt trên tất cả các trình duyệt và các cỡ màn hình

## Event loop

## Map trong javascript
- Gần giống như object
- Cú pháp:
```javascript
const map = new Map();
```
- Set property
```javascript
map.set( 'key', value )
```
- Get property
```javascript
map.get( 'key' );
```
- Loop
```javascript
for( let [key, value] of map )
```

## CORS
Là cơ chế của server cho phép các origin khác (origin = scheme(protocol) + domain + port) ngoài chính nó gửi request.
Cùng origin:
- Cùng scheme và cùng domain

http://example.com/app1/index.html
http://example.com/app2/index.html

- Port 80 by default

http://Example.com:80
http://example.com

Khác origin:
- Khác schemes
http://example.com/app1
https://example.com/app2

### Ưu nhược điểm của Server Side Rendering vs Client Side Rendering vs Pre Rendering vs Dynamic Rendering
<https://viblo.asia/p/so-sanh-server-side-rendering-vs-client-side-rendering-vs-pre-rendering-vs-dynamic-rendering-LzD5dWoOljY>

### Closure
<https://dmitripavlutin.com/simple-explanation-of-javascript-closures/>
- Lexical scope của 1 hàm:
  - Là tất cả các outer scope cộng với inner scope của hàm đó.
  - Outer scope là môi trường bên ngoài nơi định nghĩa của 1 hàm.
  - Bên trong hàm đó có thể tiếp cận được các biến được định nghĩa trong hàm cũng như ở outer scope.
- Closure: là 1 hàm mà có thể nhớ được (capture, remember) các biến bên ngoài lexical scope mà không cần quan tâm là nó được thực thi ở đâu (The closure is a function that remembers the variables from the outer scope, no matter where it is excuted)
- Đặc điểm nhận dạng closure: trong hàm có 1 biến và biến này không được khai báo trong hàm đó
```javascript
function outerFunc() {
  let outerVar = 'I am outside!';

  function innerFunc() {
    console.log(outerVar); // => logs "I am outside!"
  }

  return innerFunc;
}

const myInnerFunc = outerFunc();
myInnerFunc();
```

### for-in and for-of
`for-in`:
- iterate over enumerable properties of an object. enumerable properties are the "key" of the property the Object or "index" of the Array.
If the propery is defined with the `enumerable` false, it is not interated
```javascript
Object.defineProperty(obj, 'flameWarTopic', {
    value: 'Angular2 vs ReactJS',
    configurable: true,
    writable: false,
    enumerable: false
});
```
- can be used with String, Array, or simple Object.
- cannot be used with Object like Map() or Set();

`for-of`:
- interate over **value** of iterable objects.
- can be used with built-in `String`, `Array`, array-like objects (e.g., arguments or `NodeList`), `TypedArray`, `Map`, Set
- cannot be used with simple Object

## Hiểu gì về async, await, promise, so sánh

`Promise`:
- Là 1 object bọc lấy kết quả trả về của 1 asynchonous function.
- Has 3 trạng thái:
  - pending: trạng thái mặc định khi mới tạo Promise.
  - fulfilled: trạng thái khi asynchonous function bên trong resolve,
  - rejected: trạng thái khi asynchonous function bên trong resolve,

`async function` khi được call trả về 1 `Promise`. `Promise` này sẽ được `resolve` với 1 giá trị `value`. Giá trị `value` này chính là giá trị được `return` từ `async function`

`await` chỉ có thể dùng trong `async function`
`await` đặt trước 1 `Promise` hoặc 1 `value`. Nếu là 1 `value` thì `value` này sẽ được convert thành 1 promise thông qua `Promise.resolve(value)`
`await` sẽ pause `async function` và chờ cho `Promise` `reject` hoặc `resolve`.
- Nếu `reject` thì `await` throw 1 error.
- Nếu `resolve` thì `await` trả về kết quả resolve

## onChange và onInput khi sử dụng cho input
`onChange` chỉ chạy khi mà user ra khỏi (`blur`) input đó
`onInput` không chỉ chạy như `onChange` mà còn chạy khi gõ ('keystroke') hoặc paste

## useMemo và useCallback. Có thể chuyển từ useMemo sang useCallback được không
<https://kentcdodds.com/blog/usememo-and-usecallback>
`useCallback` thường được dùng cho các event handler và event handler này là 1 prop của 1 child component và child component này dùng `React.memo`.
Mục đích dùng `React.memo` là child component không bị re-render khi parent component re-render. Tuy nhiên, nếu props của child component bị thay đổi thì nó vẫn bị render lại. Props đấy ở đây có thể là 1 event handler. Vì thế việc dùng `useCallback` ở đây là để child component hoàn toàn không bị re-render khi parent component re-render

## CSS

### counter

### fr in grid

### clamp()

### flex-shrink và flex-grow
Mặc định flex-grow: 0; flex-shrink: 1;
Dùng để tính width của flex items

`flex-shrink: 1`: Khi tổng width của các item > parent và flex-shrink = 1 => width của item sẽ bị thu lại
`flex-grow: 1`: Khi tổng width của item < parent và flex-grow: 1 => width của item sẽ tăng lên để fill hết width của parent
