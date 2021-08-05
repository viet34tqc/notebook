# What I learned

## 19/07:
inset CSS

[...'Lidia']: String can be interated like array

Promise.race resolve/reject the first promise that resolve/rejects.

for...in loop iterate through Object keys

CORS và origin

flex-shrink và flex-grow: <https://www.youtube.com/watch?v=fm3dSg4cxRI>
	flex-shrink default is 1, flex-grow default is 0;

grid-auto-flow: columns

## 20/7:
- Array.from có thể thay cho map và tốt hơn array destructuring

```javascript
// bad
const baz = [...foo].map(bar);

// good
const baz = Array.from(foo, bar);
```

- Function nếu không return giá trị gì thì nó return `undefined` chứ không phải là `null`
```javascript
const func = () => { return }
func() // return undefined
```

- Ngoài property của object thì biến có thể bị `delete`, nếu `delete` thành công trả về true.
Không thể delete biến được khai báo bằng `var`, `let`, `const`
```javascript
const name = 'Lydia';
age = 21;

console.log(delete name); // false
console.log(delete age); // true
```

## 21/07
- grid-row, grid-column: được tính từ dòng kẻ bắt đầu của cột và hàng chứ không tính từ cột
```css
grid-row: 1 / -1 bắt đầu từ hàng đầu đến hàng cuối
```

- grid-area: grid-row-start / grid-column-start / grid-row-end / grid-column-end

## 25/07
- định nghĩa lexical scope và closure
- Ôn lại mục đích dùng `useCallback`
- `JSON.stringify` có 1 tham số thứ 2 đó là `replacer` quyết định xem giá trị nào được stringify. `replacer` có thể là 1 hàm hoặc 1 array. Nếu nó là 1 array thì chỉ có những giá trị trong array này mới được stringify
- `array.reduce` nếu không truyền giá trị mặc định thì `accumulator` sẽ lấy giá trị đầu tiên của hàm làm giá trị mặc định và current value sẽ là giá trị thứ 2 luôn
- class A extends class B thì constructor class A phải gọi `super()` trước khi access `this`

## 30/07
- Ôn lại về `async` và `await`. `Async function` trả về 1 `Promise`, `Promise` này sẽ được resolve sau với value được returned từ async function
Await đặt trước 1 promise hoặc 1 value. Nếu là value thì được convert sang `Promise`.
- Array.push() trả về `length` của array mới.
- for...in và for...of

## 31/07

- simple responsive with `display: grid`
```css
display: grid;
grid-auto-flow: rows
@media (min-width: 768px) {
	grid-auto-flow: columns;
}
```

## 3/8

- All the objects has prototype except for Object.prototype.
- There are Built-in function constructor like `String`, `Number`, `Array`. Their prototype inherit from Object.prototype
- **Arrow function doesn't have** `prototype` property
- `...args` must be **the last parameter**
- Javascript automatically insert Semicolon
`return
a + b;`
is the same as
`return;
a + b;`

- [] is a truthy value.
