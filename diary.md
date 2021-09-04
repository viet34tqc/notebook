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

## 6/8

- Ôn lại getter và setter
- Number.isNaN() và isNan()

## 7/8

- `import *`: import tất cả các exported values, kể cả default và named, * sẽ có dạng object
- `Object.seal()` và `Object.freeze()`
- How browser works
- Hoisting
- var, let const

## 8/8

- `Promise.all` runs all the passed promises in parallel. If one promise fail, `Promise.all` rejects **immediately** with the value of rejected promises
- `Object.fromEntries` turns 2d array into an object.
```javascript
const arr = [['name', 'Lydia'], ['age', 22]]
Object.fromEntries( arr ) // {name: Lydia, age: 22}
```
- Key in object is string. If it's not string, it is converted to string
```javascript
const animals = {};
let dog = { emoji: '🐶' }
let cat = { emoji: '🐈' }

animals[dog] = { ...dog, name: "Mara" }
animals[cat] = { ...cat, name: "Sara" }

console.log(animals[dog]) // animals[dog] and animals[cat] are all converted to animals['object Object']
```

## 13/8

`JSON.stringify` has a second parameter which is a function. It will be useful when you want to transform a value of a property
(<https://getfrontend.tips/transform-values-from-a-json-representation.html>)

## 16/8

- Create a download link <https://getfrontend.tips/create-a-download-link.html>
- Ignore items from array destructuring <https://getfrontend.tips/ignore-items-from-array-destructuring.html>
- Create objects with dynamic key: <https://getfrontend.tips/create-an-object-with-dynamic-keys.html>
- Datalist Element: <https://getfrontend.tips/create-an-autocomplete-list-with-the-datalist-element.html>
- Prefix classname with 'js': <https://getfrontend.tips/prefix-class-name-with-js-to-manipulate-with-javascript.html>
- Remove a property from an object using spread operator: <https://getfrontend.tips/remove-a-property-from-an-object.html>
- Use underscore to represent a number: <https://getfrontend.tips/use-underscores-to-represent-a-number.html>
- <https://getfrontend.tips/wrap-arrow-function-body-in-parentheses.html>
- Modify character of string: <https://getfrontend.tips/get-characters-of-a-string.html>

## 18/8

- Center child element: `display:grid; place-items: center;`

## 31/8

- :focus-within
- :where selecter hoạt động giống :is nhưng specificity bằng 0: <https://css-tricks.com/using-the-specificity-of-where-as-a-css-reset/>>
- min, max, clamp: <https://moderncss.dev/practical-uses-of-css-math-functions-calc-clamp-min-max/>
  - min(value1, value2): thiết lập giá trị max cho selector (giống max-width). Nó sẽ chọn ra giá trị nhỏ nhất giữa value 1 và value 2 tùy theo kích cỡ màn hình. VD:
  ```css
	width: min(80ch, 100% - 2rem) // Ở màn hình nhỏ hơn thì 100% - 2rem < 80ch nên width = 100% - 2rem nhưng ở màn hình lớn hơn khi mà 100% - 2rem > 80ch thì width = 80ch;
  ```
  sẽ tương đương với
  ```css
  	width: 100% - 2rem;
  	max-width: 80ch
  ```
  - max(value, value): ngược lại với min(), thiết lập giá trị nhỏ nhất cho selector (giống min-width) . VD: disable zoom trên safari khi focus vào input
	```CSS
	input {
	  font-size: max(16px, 1rem);
	}
	```