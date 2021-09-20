# Interview

## Why React

- Easy to start
- Component-based
  - consistent
  - Easy to maintain and scale
- Virtual DOM
  - Updating DOM using vanilla JS is slow, especially when your DOM is big
  - Any changes are reflected to virtual DOM
  - React compares the previous and the current state of the virtual DOM
  - Those updates are applied to real DOM

## Redux and React Context

<https://blog.isquaredsoftware.com/2021/01/context-redux-differences/>

React Context doesn't manage any state at all. When we use Context, we manage the state via React hooks `useState`and `useReducer`. The only purpose of React Context is to avoid **prop-drilling**

Context + `useReducer` relies on passing *the current state value via Context*. React-Redux passes *the current Redux store* instance via Context.
When `useReducer` produces a new state value, *all* components that are subscribed to that context will be re-rendered => lead to perfomance issue
React Redux, components can't subscribe to specific state and only re-render when those values change.

Context + `useReducer` doesn't not have middleware to do side-effect
React + Redux have middleware support using Redux Thunk

if you get past 2-3 state-related contexts in an application, you're re-inventing a weaker version of React-Redux and should just switch to using Redux.

## Why react use immutability?

- Keep previous versions of the state, and reuse them later
- Create pure components. Immutable data can determine if changes have been made, which helps to *determine when a component require re-rerendering*.
If you mutate the data directly, the components might not re-render that could lead to bugs.

## `useState` and `useReducer`

- Use `useState` whenever
  - you manage a JS primitive or simple array
  - simple state modification
- Use `useReducer`
  - whenever you manage an object or complex array
  - complex state modification
  - whenever you use multiple `setState` call

## How React rendered UI

Problem: DOM manipulation is slow. Let's assume that you have a list of items. When you want to modify that item, you need to select that item and re-render it and re-paint the UI. Therefore, the more UI components you have, the more expensive the DOM updates could be, since they would need to be re-rendered for every DOM update.

To address this problem, React use virtual DOM. A virtual DOM is a representation of DOM object and synced with the real DOM.

React sử dụng sử dụng ReactDOM.render() và dùng jsx (React.createElement) để render virtual DOM ban đầu.

## How React update UI

Once virtual DOM has updated, React compares the entire virtual DOM with a virtual DOM snapshot that was taken right before the update.
By comparing, React knows exactly which virtual DOM **object** (component) has changed. If there are a lot of updates, React can batch them (collect the updates) for efficency.
Then real DOM is updated (this process is commit)

## React bailing out of updating states

<https://stackoverflow.com/questions/58208727/why-react-needs-another-render-to-bail-out-state-updates>
Nếu giá trị state không thay đổi khi setState thì component sẽ render thêm 1 lần nữa. Đến lần setState thứ 3 thì nó dừng hẳn không re-render tiếp.

## Why list item need a unique `key`

<https://kentcdodds.com/blog/understanding-reacts-key-prop>
Để React có thể theo dõi (track) được item nào bị xóa, thêm hay bị sửa. Key của mỗi item phải là duy nhất và không thay đổi. Nếu lấy key là index thì khi thêm, hoặc xóa item, index bị thay đổi => render sai

When the `key` props is changed, React unmount the previous instance, and mount a new one. This means that all state that had existed in the component at the time is completely removed and the component is reinitialized
Khi 1 component bất kì có props key, khi key này bị thay đổi thì component sẽ bị unmount, state của nó cũng sẽ bị xóa bỏ và React sẽ mount lại 1 component mới

## Optimize React re-render

<https://kentcdodds.com/blog/optimize-react-re-renders>
Mỗi khi render 1 component nào đó, props object của component đó sẽ được làm mới hoàn toàn, kể cả khi property của prop objects đó không đổi

## React Portal

Cho phép render 1 component vào 1 DOM node nằm ngoài root.
Trong 1 App react thông thường, toàn bộ UI (components) sẽ được render trong 1 the div với id là root `<div id="root"><div>`
Giả sử ta muốn tạo 1 cái modal và thẻ div bọc lấy modal này nằm ngoài root. Và vì thẻ div này nằm ngoài root, ta không thể tạo component và render bình thường được mà phải dùng React Portal.

## Event, Event handler, Event listener

**Event** là 1 hành động, 1 sự kiện nào đó khi user tương tác: click chuột vào 1 phần tử, submit 1 form nào đó
**Event handler** là 1 đoạn code, 1 hàm được chạy khi event xảy ra
**Event handler** còn được gọi là ***event listener***

## Màn bình thường và màn retina

Màn thường: 1 CSS pixel tương đương 1 điểm ảnh vật lý
Màn retina: 1 CSS pixel tương đương 4 điểm ảnh vật lý

## Debounce và throttle

- Debounce: gọi 1 hàm nào đó sau 1 khoảng delay. VD: Có 1 input. Input này có 1 event handler cho sự kiện onChange. Nếu áp dụng debounce, event handler này sẽ chỉ chạy khi:
  - Hết thời gian delay
  - Sự kiện đó không xảy ra nữa. Nếu sự kiện đó xảy ra trong khoảng thời gian delay thì reset lại thời gian delay
- Throttle: đảm bảo 1 hàm chỉ chạy 1 lần trong 1 khoảng thời gian (vd: 10s).
Ví dụ event scroll với 1 event listener .on('scroll') và hàm f, trong điều kiện bình thường hàm f sẽ được gọi 1000 lần trong 10 giây nếu ta scroll liên tục và không có can thiệp gì. Nếu ta sử dụng throttle, cho phép event listener kích hoạt mỗi 100 mili giây, thì hàm f sẽ chỉ được gọi 100 lần là nhiều nhất trong vòng 10 giây bạn scroll

## Object.assign() and Object.create()

`Object.assign(target, source)` copy enumerable properties from `source` to `target` object.
`Object.create(source)` create a new object using the `source` as prototype

## So sánh display: flex và display: grid

- Grid: hiển thị layout theo 2 chiều
- Flex: hiển thị layout theo 1 chiều

## Kiến thức bảo mật tối thiểu ở frontend

## Khi nào dùng '==' thay vì '==='

Chỉ dùng khi compare `null` và `undefined`

## getter, setter trong javascript. Có ưu điểm gì so với hàm bình thường

```javascript
const config = {
  languages: [],
  get language() {
    return this.languages
  },
  set language(lang) {
    return this.languages.push(lang);
  },
};

// Set the languages
config.language = 'VN';
// Get the languages
console.log( config.language ) // ['VN']
```

`Setter` doesn't hold any value, their purpose is to modify the properties. If your object doesn't have getter function and you call the setter function, it returns undefined

## Number.isNaN() và isNan()

`Number.isNaN(value)` check if the `value` is numeric and not a number.
`isNaN(value)` check if the `value` is not a number

## Animation có những thuộc tính gì

## Hiểu thế nào về responsive web design

Đó là việc 1 website có thể hiển thị và tương tác (UI và UX) tốt trên tất cả các trình duyệt và các cỡ màn hình

## Map trong javascript

- Gần giống như object, khác ở chỗ key có thể là bất cứ type nào, kể cả object
- Cú pháp:
```javascript
const map = new Map([iterable]); // iterable is optional, such as new Map([[a,1], [b,2]])
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
Cùng origin: cùng scheme, domain và port
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

Để server có thể cho phép client gửi cross-origin request, server đó phải set giá trị cho 1 header có tên là `Access-Control-Allow-Origin` và giá trị đó là origin của client.

### Rendering a web page

<ol>
  <li>You type an URL into address bar in your preferred browser.</li>
  <li>The browser **parses the URL** to find the protocol, host, port and path.</li>
  <li>It forms a HTTP request</li>
  <li>To reach the host, it first needs to translate the human readable host into an IP number, and it does this by doing a DNS lookup on the host</li>
  <li>Then a socket needs to be opened from the user's computer to that IP number, on the port specified (most often port 80)</li>
  <li>When a connection is open, the HTTP request is sent to the host</li>
  <li>The host forwards the request to the web server (most often Apache) configured to listen on the specified port
  The server inspects the request (most often only the path), and launches the server plugin needed to handle the request (corresponding to the server language you use, PHP, Java, .NET, Python?)</li>
  <li>The plugin gets access to the full request, and starts to prepare a HTTP response.</li>
  <li>To construct the response, a database is (most likely) accessed.</li>
  Data from the database, together with other information the plugin decides to add, is combined into HTML.</li>
  <li>The plugin combines that data with some meta data (in the form of HTTP headers), and sends the HTTP response back to the browser.</li>
  <li>The browser receives the response, and parses the HTML in the response</li>
  <li>A DOM tree is built with HTML</li>
  <li>New requests are made to the server for each new resource that is found in the HTML source (typically images, style sheets, and JavaScript files). Go back to step 3 and repeat for each resource.</li>
  <li>Stylesheets are parsed, and the rendering information in each gets attached to the matching node in the DOM tree</li>
  <li>Javascript is parsed and executed, and DOM nodes are moved and style information is updated accordingly</li>
  <li>The browser renders the page on the screen according to the DOM tree and the style information for each node</li>
</ol>

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

### Primitive và Reference type

<https://daveceddia.com/javascript-references/>
<https://www.youtube.com/watch?v=RLBqJpK1hro>
Khi khai báo 1 biến, giá trị của biến đấy được bỏ vào trong 1 cái box, và biến trỏ vào box đó.

Primitive type: string, number, boolean, undefined, null, Symbol
Reference type: object, array, Map, Set

Primitive lưu giá trị
Reference lưu địa chỉ chứa giá trị

Primitive type là immutable. Ta chỉ có thể gán lại biến đấy sang 1 giá trị khác (1 box khác)
Reference type là mutable. Nếu ta copy biến đấy sang 1 biến khác, 2 biến đấy sẽ trỏ vào cùng 1 box chứ 2 biến không trỏ lẫn sang nhau. Và nếu 1 trong 2 biến mutate (ví dụ như là thay đổi giá trị của property) thì biến còn lại cũng thay đổi theo (vì vẫn chung 1 box)

Notes:
- Các giá trị dạng reference là riêng biệt:
Mỗi khi ta khai báo 1 hàm hoặc 1 object, `() => {}` hoặc `{}` thì 1 box mới được tạo ra
```js
const a = () => {}
const b = () => {}
console.log( a === b ) // return false.
```
- Truyền tham số dạng reference có thể xảy ra Mutation không mong muốn
```javascript
function minimum(array) {
  array.sort();
  return array[0]
}

const items = [7, 1, 9, 4];
const min = minimum(items); // Khi truyền items vào dưới dạng tham số, hàm sẽ copy tham số sang 1 biết khác: const array = items. Array thay đổi thì items cũng thay đổi vì khi này cả array và items cùng trỏ tới 1 box hay là cùng trỏ tới 1 địa chỉ chứa giá trị
console.log(min) // 1
console.log(items) // [1,4,7,9]
```

### useState rules

Only call Hooks at the top level. You shouldn't call `useState` in loop, conditions
Only call Hooks inside React function components

`useState` doens't merge objects when the state is updated. It replaces them.

When there is an update action (ex: click event).
- The update state function (setState) isn't called immediately. It's moved to a queue
- Then a re-render of the component is scheduled.
- The event handler is wrap into a batch, when the batch finishs, the re-render begins
- All scheduled re-renders run
  - `useState` is called
  - `setState` is invoked and save the final result in the hook's state
  - `useState` returns the updated value

If you update the state hook to the same value as the current state, there will be no re-render to be scheduled.

### Scope chain

Có 3 loại scope: `global scope`, `local scope` (function) và `block scope` (scope nằm giữa {}). Khi một biến không được tìm thấy trong `local scope`, JS sẽ tìm biến đó ở các outer scope cho đến scope ngoài cùng là `global scope`.

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
- can be used with built-in `String`, `Array`, array-like objects (e.g., arguments or `NodeList`), `TypedArray`, `Map`, `Set`
- cannot be used with simple Object

## Hiểu gì về async, await, promise, so sánh

`Promise`:
- Là 1 object bọc lấy kết quả trả về của 1 asynchonous function.
- Có 3 trạng thái:
  - pending: trạng thái mặc định khi mới tạo Promise.
  - fulfilled: trạng thái khi asynchonous function bên trong resolve,
  - rejected: trạng thái khi asynchonous function bên trong resolve,

**Converting a setTimeout into a promisified `delay` function**
```ts
const delay = (ms: number) => new Promise( res => setTimeout(res, ms))
const delay = (ms: number) => new Promise( res => setTimeout(res, ms, 'some value')) // If you want to send some value
```

`async function` khi được call trả về 1 `Promise`. `Promise` này sẽ được `resolve` với 1 giá trị `value`. Giá trị `value` này chính là giá trị được `return` từ `async function`

`await` chỉ có thể dùng trong `async function`
`await` đặt trước 1 `Promise` hoặc 1 `value`. Nếu là 1 `value` thì `value` này sẽ được convert thành 1 promise thông qua `Promise.resolve(value)`
`await` sẽ pause `async function` và chờ cho `Promise` `reject` hoặc `resolve`.
- Nếu `reject` thì `await` throw 1 error.
- Nếu `resolve` thì `await` trả về kết quả resolve

### onChange và onInput khi sử dụng cho input

`onChange` chỉ chạy khi mà user ra khỏi (`blur`) input đó
`onInput` không chỉ chạy như `onChange` mà còn chạy khi gõ ('keystroke') hoặc paste

### import and export

Khi import mà dùng `*`, thì chúng ta import hết tất cả các giá trị được export, bao gồm cả default và named.
`*` sẽ là 1 object chứa hết các giá trị trên

```javascript
// info.js
export const name = 'Lydia';
export const age = 21;
export default 'I love JavaScript';

// index.js
import * as info from './info';
console.log(info);

{
  default: "I love JavaScript",
  name: "Lydia",
  age: 21
}
```

### Các chỉ số cần quan tâm khi optimize speed:

- Time to first byte (TTFB): the time it takes the browsers to start receiving a data (images, fonts, js, css...) after requesting data from server: HTTP request time + Process request time + HTTP response time.
- Critical Rendering Path: is the sequences of steps of: convert the HTML, CSS, and JavaScript into pixels on the screen.
- DOMContentLoaded: the time for loading HTML before starting to load the content
- Load: when all the resources are loaded ( resources are parsed and get acknowledged off before DOMContentLoaded)
- First Contentful Paint(FCP): the moment when the first element from the DOM appears in the users' browser.
- Speed Index: shows how quickly the contents of a page are visibly populated.
- Time To (fully) Interactive(TTI): the amount of time it takes for the page to be fully interactive.
- First Input Delay (FID): the time from when a user first interacts with a page to the time when the browser is actually able to respond to that interaction.
- Total Blocking Time: total amount of time between First Contentful Paint (FCP) and Time to Interactive (TTI) where the main thread was blocked for long enough to prevent input responsiveness.
- Largest Contentful Paint (2.5s): the time when the largest content element in the viewport becomes visible. It can be used to determine when the main content of the page has finished rendering on the screen.

### Obtimize speed:

<https://towardsdev.com/website-performance-optimization-267b28b877df>
- TTFB: <https://searchfacts.com/reduce-ttfb/>
  - Fast web hosting
  - Keep the server close to users
  - Caching (the server wont need to build the page by fetching from database)
  - Fast DNS provider: reduce the DNS lookup times
  - CDN for static files (images, CSS, JS files)
- Font: reduce webfont
- Image: optimize and lazy load
- Caching:
  - Browser caching
  - Page caching
- Gzip compression: compresses Web pages, CSS, and JavaScript at the server level before sending them over to the browser. On the user side, a browser unzips the files and presents the contents.
- Reduce redirect: because they can create additional HTTP request

### Critical Rending Path

The critical rendering path includes the Document Object Model (DOM), CSS Object Model (CSSOM), Render tree, Layout, and Paint.
The document object model is created as the HTML is parsed. The HTML may request JavaScript, which may, in turn, alter the DOM. The HTML includes or makes requests for styles, which in turn builds the CSS object model. The browser engine combines the two to create the Render Tree. Layout determines the size and location of everything on the page. Once the layout is determined, pixels are painted to the screen.
Optimizing the critical render path improves render performance. Performance tips include:
- Minimizing the number of critical resources by deferring their download ( defer attribute), marking them as async (async attribute), or eliminating them altogether.
- Optimizing the number of requests required along with the file size of each request.
- Optimizing the order in which critical resources are loaded by prioritizing the downloading critical assets, shorten the critical path length.

### var and let, const

- Blocked scope
`var` is blocked scope only when it is declared inside a function
`let` and `const` blocked scope every where
- Creation phase
This is the very first phase when JS setting up memory for the data. No code is executed at this point.
Variable declared with `let` and `const` are stored `uninitialized`
Variable declared with `var` are stored with the default value of `undefined`

Then if we try to access the variables before its declaration:
When variable declared with `var`, it returns `undefined`
When variable declared with `let` and `const`, it returns `ReferenceError`

### Hoisting

It's the process that stores functions and variables in the memory before executing any code (creation phase);
- Functions are stored with the entire function. That's why you can invoke them before the line we declare them
- Variables is different.
  - Variables with the `var` keyword are stored with the value of `undefined`
  - Variables with the `let` and `const` keyword are stored `uninitialized`. If we try to access them before declare them, it will return `ReferenceError`

### Object.freeze() và Object.seal()

`Object.freeze()` không cho phép thêm, sửa, xóa properties. Tuy nhiên, `Object.freeze()` chỉ shallowly freeze, nếu properties là 1 object khác thì vẫn có thể sửa được
`Object.seal()` không cho phép thêm, xóa, nhưng vẫn cho sửa existing properties

## useMemo và useCallback. Có thể chuyển từ useMemo sang useCallback được không

<https://kentcdodds.com/blog/usememo-and-usecallback>
`useCallback` thường được dùng cho các event handler và event handler này là 1 prop của 1 child component và child component này dùng `React.memo`.
Mục đích dùng `React.memo` là child component không bị re-render khi parent component re-render. Tuy nhiên, nếu props của child component bị thay đổi thì nó vẫn bị render lại. Props đấy ở đây có thể là 1 event handler. Vì thế việc dùng `useCallback` ở đây là để child component hoàn toàn không bị re-render khi parent component re-render

Chuyển đổi giữa `useMemo` và `useCallback`
`useCallback(fn, deps)` tương đương với `useMemo(() => fn, deps)`.

## CSS

### counter

### fr in grid

### min, max, clamp()

<https://moderncss.dev/practical-uses-of-css-math-functions-calc-clamp-min-max/>
<https://ishadeed.com/article/css-min-max-clamp/>
- `min(value1, value2)`: thiết lập giá trị max cho selector (giống max-width). Nó sẽ chọn ra giá trị nhỏ nhất giữa value 1 và value 2 tùy theo viewport width. Thứ tự value không quan trọng. VD:
```css
width: min(80ch, 100% - 2rem) // Ở màn hình nhỏ hơn thì 100% - 2rem < 80ch nên width = 100% - 2rem nhưng ở màn hình lớn hơn khi mà 100% - 2rem > 80ch thì width = 80ch;
```
  CSS trên sẽ tương đương với
```css
  width: 100% - 2rem;
  max-width: 80ch
```
- `max(value, value)`: ngược lại với min(), thiết lập giá trị nhỏ nhất cho selector (giống min-width) . VD: disable zoom trên safari khi focus vào input
```CSS
input {
  font-size: max(16px, 1rem);
}
```
- clamp(min, value, max): thứ tự các giá trị cũng không quan trọng. Thường dùng để làm responsive cho font-size. VD:
```CSS
p {
  font-size: clamp(2rem, 5vw, 5rem)
}
```
Nếu chỉ dùng 5vw thì font-size sẽ bị fix theo vw và trên mobile có thể bị bé. Khi dùng `clamp`, giá trị min của font-size sẽ luôn là 2rem
Cần chú ý là khi zoom thì font size sẽ không bị thay đổi vì nó phụ thuộc vào vw. Có thể sửa lại dùng rem vì rem thay đổi theo zoom
=> có thể thay đổi thành:
```CSS
p {
  font-size: clamp(2rem, 1rem + 2vw, 5rem);
}
```
Chỉnh padding cho section hero:
```CSS
.hero {
  padding: clamp(2rem, 15vmax, 15rem) 1rem;
}
```

### Create text on image with an overlay

Set z-index for the container

Using `background-blend-mode`: <https://www.smashingmagazine.com/2021/09/reducing-need-pseudo-elements/>

### flex-shrink và flex-grow

Mặc định flex-grow: 0; flex-shrink: 1;
Dùng để tính width của flex items

`flex-shrink: 1`: Khi tổng width của các item > parent và flex-shrink = 1 => width của item sẽ bị thu lại
`flex-grow: 1`: Khi tổng width của item < parent và flex-grow: 1 => width của item sẽ tăng lên để fill hết width của parent
