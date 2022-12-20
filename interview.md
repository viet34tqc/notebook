# Interview

<https://itnext.io/frontend-interview-cheatsheet-that-helped-me-to-get-offer-on-amazon-and-linkedin-cba9584e33c7#21a5>

## Coding Interview Project

<https://jantu.hashnode.dev/how-to-implement-infinite-scrolling-with-reactjs>

## Why React

- Declarative

- Easy to start
- Component-based
  - consistent
  - Easy to maintain and scale
- Virtual DOM
  - Provide faster update on the DOM
  - It's copy of the real DOM
  - Updating DOM using vanilla JS is slow, especially when your DOM is big
  - Whenever there is an update, like state changes or props, React update the virtual DOM
  - React compares the previous and the current state of the virtual DOM
  - Those updates are applied to real DOM

## Redux and React Context

<https://blog.isquaredsoftware.com/2021/01/context-redux-differences/>

React Context doesn't manage any state at all. When we use Context, we manage the state via React hooks `useState` and `useReducer`. The only purpose of React Context is to avoid **prop-drilling**

Context + `useReducer` relies on passing *the current state value via Context*. React-Redux passes *the current Redux store* instance via Context.
When `useReducer` produces a new state value, *all* components that are subscribed to that context will be re-rendered => lead to perfomance issue
In React Redux, components can subscribe to specific pieces of store state and only re-render when those values change.

Context + `useReducer` doesn't not have middleware to do side-effect
React + Redux have middleware support using Redux Thunk

if you get past 2-3 state-related contexts in an application, you're re-inventing a weaker version of React-Redux and should just switch to using Redux.

## What is state in React

Information that changes in response to user interactions is called "state".
In react, it's a variable which whenever it updates, it will trigger a re-renders of the component.

## What is rendering in React

- "Rendering" means that React is calling your component, which is a function
- Each render is a snapshot, like a photo taken by a camera, that shows what the UI should look like, based on the current application state.
- Each render create their own scope, they work individually.
- The point of a re-render is to figure out how a state change should affect the user interface. And so we need to re-render all potentially-affected components, to get an accurate snapshot.

## How to build a good component

- A "good component" is a component that I can easily read and understand what it does from the first glance.
- A "good component" should have a good self-describing name. Sidebar for a component that renders sidebar is a good name. CreateIssue for a component that handles issue creation is a good name. SidebarController for a component that renders sidebar items specific for 'Issues' page is not a good name (the name indicates that the component is of some generic purpose, not specific to a particular page).
- A "good component" doesn't do things that are irrelevant to its declared purpose. Topbar component that only renders items in the top bar and controls only topbar behaviour is a good component. Sidebar component, that controls the state of various modal dialogs is not the best component.

## Why react use immutability?

- Keep previous versions of the state, and reuse them later
- Create pure components. Immutable data can determine if changes have been made, which helps to *determine when a component require re-rerendering*.
If you mutate the data directly, the components might not re-render that could lead to bugs.

## Why do we need to put side effects in the `useEffect`

With `useEffect`, we can control how the side effects run in the component. Normally, the side effects make the state change. So, if you put them outside the `useEffec` hook, it will lead to infinite re-render of the component.

## When react does not re-render

<https://www.zhenghao.io/posts/react-rerender>
<https://blog.isquaredsoftware.com/2020/05/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior/>

If a react component returns exact the same element reference in its render output as it did in the last time, React will skip re-rendering that particular child (Explaination: <https://www.developerway.com/posts/react-elements-children-parents>)

- The child component is pass as a prop or children prop
- The child component is memorized using `memo()`. That makes the component as pure component

``` jsx 
// The `props.children` content won't re-render if we update state
function SomeProvider({children}) {
  const [counter, setCounter] = useState(0);

  return (
    <div>
      <button onClick={() => setCounter(counter + 1)}>Count: {counter}</button>
      <OtherChildComponent />
      {children}
    </div>
  )
}
```

```jsx
// Only childA re-render, childB, childC not when the parent re-renders
function Parent({ children, lastChild }) {
  return (
    <div className="parent">
      <ChildA />
      {children}
      {lastChild}
    </div>
  );
}

<Parent lastChild={<ChildC />}>
  <ChildB />
</Parent>

function ChildA() {
  return <div className="childA"></div>;
}

function ChildB() {
  return <div className="childB"></div>;
}

function ChildC() {
  return <div className="childC"></div>;
}
```

## React re-render

<https://www.zhenghao.io/posts/react-rerender>
<https://www.developerway.com/posts/components-composition-how-to-get-it-right>
<https://www.developerway.com/posts/how-to-write-performant-react-code>
<https://www.developerway.com/posts/react-re-renders-guide>
<https://www.joshwcomeau.com/react/why-react-re-renders/>

- When props or state have changed
- When parent component re-renders, even if the props pass to child component is memorized
- When a component uses context and the value of its provider changes. Context is sorta like “invisible props”, or maybe "internal props".
- Component use custom hook and the state of that hook changes or Context's value changes.

How to prevent unnecessary re-renders

- Moving state down: keep the state of the component as close as possible if that state is used in the component only
- Composition: children as props or component as props
- `React.memo` without `useMemo`: use when rendering a heavy component that is independent from the parent component's state. Besides using `React.memo` in the child component, we can also create a function use `useMemo` inside parent component that return the heavy component.
- `React.memo` with `useMemo`: use when the child component use state from parent component as props. 
- Context value: memorize context value, split value into chunks

## `useState` and `useReducer`

- Use `useState` whenever
  - you manage a JS primitive or simple array
  - simple state modification
- Use `useReducer`
  - whenever you manage an object or complex array
  - complex state modification
  - whenever you use multiple `setState` call

## How React rendered UI

<https://beta.reactjs.org/learn/render-and-commit>
<https://dev.to/teo_garcia/understanding-rendering-in-react-i5i>

3 phases:

- Trigger a render: there are two reasons for a component to render
  - Calling `ReactDOM.render` to render the root component. This is the initial render
  - The component's states has been update via event, etc...
- Render: React calls your components to figure out what should be on the screen.
  - During the initial render, React will create the DOM nodes.
  - During the re-render, React will calculate which of their properties, if any, have changed since the previous render. It won't do anything with that infomation until the next phase, the commit phase
- Commit: React commit changes to the DOM
  - For the initial render, React will use `appendChild` DOM API to put all the DOM nodes it has created on the screen
  - For re-renders, **React only changes the DOM nodes if there is a difference between renders**. If a component re-renders, it only changes the updated DOMs (that could contains updated data), the other DOMs in that component are still the same

## How React update UI

This process is called reconciliation
Once virtual DOM has updated, React compares the entire virtual DOM with a virtual DOM snapshot that was taken right before the update.
By comparing, React knows exactly which virtual DOM **object** (component) has changed. If there are a lot of updates, React can batch them (collect the updates) for efficency.
Then real DOM is updated (this process is commit)

## React bailing out of updating states

<https://stackoverflow.com/questions/58208727/why-react-needs-another-render-to-bail-out-state-updates>

Nếu giá trị state không thay đổi khi setState thì component sẽ render thêm 1 lần nữa. Đến lần setState thứ 3 thì nó dừng hẳn không re-render tiếp.

## Preventing errors from unmounted components

<https://www.digitalocean.com/community/tutorials/how-to-handle-async-data-loading-lazy-loading-and-code-splitting-with-react>

```js
const [show, toggle] = useReducer(state => !state, true);
```
Let's say you have a component which includes 2 buttons. The first one is for fetching the data and update the state. The second one is for unmounting the component. When you click the fetching button, then immediately click the second button, there is a chance that you will encountered an error: `Can't perform a React state update on unmounted component`. That's because your asynchronous request hasn't been resolved yet when the component is unmounted.

To fix the problem you need to either cancel the request inside `useEffect`. Otherwise, you need to ignore the `setState` by using a variable to store the mounted state. The request is still resolved but, it won't make any changes to unmounted component. 

```js
useEffect(() => {
    let mounted = true;
    requestToGetData(name)
    .then(data => {
      if(mounted) {
        setState(data)
      }
    });
    return () => {
       mounted = false;
    } 
}, [name])
```

## Why we cannot write `if`, `else`... in JSX

It's because `if` is not an expression in JS

## React flushSync

<https://beta.reactjs.org/learn/manipulating-the-dom-with-refs#when-react-attaches-the-refs>

Normally, in React, state updates are queued. That means `setState` doesn't update the state immediately. `flushSync` force React to update ('flush') the DOM synchronously.

## Authentication security

<https://blog.openreplay.com/11-authentication-mistakes-and-how-to-fix-them>

## Why list item need a unique `key`

<https://kentcdodds.com/blog/understanding-reacts-key-prop>

Key giúp React có thể theo dõi (track) được item nào bị xóa, thêm hay bị sửa. Key của mỗi item phải là duy nhất và không thay đổi. Nếu lấy key là index thì khi thêm, hoặc xóa item, index bị thay đổi => render sai

Ngoài ra, khi sắp xếp, thêm, xóa item, việc render DOM sẽ tốn rất nhiều thời gian. Nếu item có key, React sẽ nhớ được item đó và chỉ việc hiển thị lại item đấy chứ không phải re-render lại từ đầu => tối ưu về mặt thời gian

When the `key` props is changed, React unmount the previous instance, and mount a new one. This means that all state that had existed in the component at the time is completely removed and the component is reinitialized
Khi 1 component bất kì có props key, khi key này bị thay đổi thì component sẽ bị unmount, state của nó cũng sẽ bị xóa bỏ và React sẽ mount lại 1 component mới

## API là gì

<https://www.youtube.com/watch?v=JUvsdFWL7WM>

API là các phương thức (functions, protocols) giúp cho ứng dụng A có thể tương tác với ứng dụng B mà không cần quan tâm ứng dụng B được thực hiện thế nào

VD: 
- ứng dụng B cung cấp 1 hàm tính tổng cho ứng dụng A.
- React cung cấp hàm `useState` để ltv sử dụng
- Backend xây dựng API để Frontend sử dụng để lấy data

Có thể liên tưởng tới UI: user tương tác với giao diện (interface) của ứng dụng mà không cần quan tâm giao diện đấy được thực hiện sao

## Optimize React re-render

<https://kentcdodds.com/blog/optimize-react-re-renders>
Mỗi khi render 1 component nào đó, props object của component đó sẽ được làm mới hoàn toàn, kể cả khi property của prop objects đó không đổi

## React Portal

Cho phép render 1 component vào 1 DOM node nằm ngoài root.
Why: để tạo ra các component mà không bị ảnh hưởng bởi style của component cha vì component cha có thể bị overflow hidden 
Trong 1 App react thông thường, toàn bộ UI (components) sẽ được render trong 1 the div với id là root `<div id="root"><div>`
Giả sử ta muốn tạo 1 cái modal và thẻ div bọc lấy modal này nằm ngoài root. Và vì thẻ div này nằm ngoài root, ta không thể tạo component và render bình thường được mà phải dùng React Portal.

## JavaScript Promises: then(f,f) vs then(f).catch(f)

The difference happens when the `success` callback returns a **rejected promise**, like `Promise.reject('Invalid')`:

`then(f,f)`: `error` callback is not invoked
`then(f).catch(f)`: error callback is invoked

## Rule for `this`

<https://blog.tusharcodes.tech/5-rules-to-master-this-in-javascript>

- If the new keyword is used when calling the function, this inside the function is a all new empty object.
- If apply, call, or bind is used, this inside the function is the object that is passed in as the argument.
- If a function is called as a method this is the object that the function is a property of.
- If a function is invoked as a free function invocation, this is the global object.
- If it's an arrow function, this value will be the context of its surrounding scope at the time it is created. Arrow function will try to resolve this inside it lexically just like any other variable and ask the Outer function - Do you have a this? And Outer Function will reply YES and gives inner function its own context to this

```js
function outer() {
  console.log(this); // { x : 5 }
  this.x = 10;
  const inner = () => {
     console.log(this); // { x : 10 }
  }
  inner();
}
outer.call({x: 5});
```

## Các cách authentication

<https://viblo.asia/p/cac-phuong-thuc-pho-bien-dung-authentication-naQZRPGP5vx>

## Why use semantic HTML

- Better for SEO
- Better accessibility

## Event, Event handler, Event listener

**Event** là 1 hành động, 1 sự kiện nào đó khi user tương tác: click chuột vào 1 phần tử, submit 1 form nào đó
**Event handler** là 1 đoạn code, 1 hàm được chạy khi event xảy ra
**Event handler** còn được gọi là ***event listener***

## Màn bình thường và màn retina

Màn thường: 1 CSS pixel tương đương 1 điểm ảnh vật lý
Màn retina: 1 CSS pixel tương đương 4 điểm ảnh vật lý

## CDN là gì

<https://viblo.asia/p/cdn-la-gi-huong-dan-tich-hop-cdn-vao-he-thong-voi-cloudflare-gGJ59rRpKX2>
CDN (Content Delivery Network) tạm dịch là mạng phân phối nội dung. Đó là một mạng lưới máy chủ (caching server), được bố trí phân tán theo khu vực địa lý, với nhiệm vụ chính là phân phối nội dung tĩnh tới người dùng nhanh nhất có thể.

## Headless website

Headless website is built with separated back-end and frontend. With headless, your frontend (head) is decoupled from the backend (body).
Your frontend is a JS application that run by browser, and it doesn't need to be hosted on a server. You just need to put your code somewhere browser can download it (Often it's just on CDN)
Whereas, your backend can be WordPress or NodeJS and hosted on a server.
If needed, frontend part will communicate to the backend end via API (using REST or GraphQL) to get the data or perform other actions such as sending email

## Cơ chế làm việc của session và JWT

<https://viblo.asia/p/cach-trien-khai-refreshtoken-va-freshtoken-p1-dung-luu-tat-ca-phien-cua-nguoi-dung-3Q75wN23lWb>
  
- Session

Khi client tạo 1 request login tới server, server sẽ tìm trong sổ user coi có hợp lệ không, nếu oke thì lấy các quyền mà user đó có, rồi tạo 1 session lưu ở server và trả về cho client 1 cookie (chứa session id (không trùng lặp)). Với mỗi request, client sẽ cầm theo cookie, server sẽ dựa vào id trong đó để tìm trong sổ session, coi cái nào khớp thì coi có quyền gì mà xử lí, không có thì bắt login lại.

- JWT

Không lưu session, cấp cho người dùng 1 tờ giấy uỷ quyền (token), trên đó có thông tin/quyền user được phép, có chữ kí và ngày hết hạn. Mỗi lần truy cập vào đâu ông bảo vệ không cần tra cứu sổ sách gì cả, cứ nhìn giấy uỷ quyền hợp lệ là cho vô.

Các vấn đế của JWT

- User A authen thành công ➜ server response token ➜ User A bị Man-in-the-middle attack, do User A bất cẩn làm token rơi vào tay hacker ➜ hacker chiếm quyền truy cập account của user A mà server không hề hay biết
- User A đang truy cập các private API, đột nhiên token expired, màn hình đột ngột bị rediect sang trang login vì server response 403
- server chưa cài đặt cơ chế hủy token, phía database thì account user A đã bị ban, nhưng token vẫn đang lưu rải rác tại các client (desktop app, mobile app, web, server khác, ...), client vẫn cứ gửi token, server encode payload token thấy vẫn còn hạn sử dụng, phần mã hash chữ ký vẫn hợp lệ ➜ user vẫn author thành công

## Debounce và throttle

<https://www.youtube.com/watch?v=LZb_Bv81vQs>
<https://www.kaidohussar.dev/posts/debounce-vs-throttle>

- Debounce: gọi 1 hàm nào đó sau 1 khoảng delay. VD: Có 1 input. Input này có 1 event handler cho sự kiện onChange. Nếu áp dụng debounce, event handler này sẽ chỉ chạy khi:
  - Hết thời gian delay
  - Sự kiện đó không xảy ra nữa. Nếu sự kiện đó xảy ra trong khoảng thời gian delay thì reset lại thời gian delay
- Throttle: đảm bảo 1 hàm chỉ chạy 1 lần trong 1 khoảng thời gian (vd: 10s).
Ví dụ event scroll với 1 event listener .on('scroll') và hàm f, trong điều kiện bình thường hàm f sẽ được gọi 1000 lần trong 10 giây nếu ta scroll liên tục và không có can thiệp gì. Nếu ta sử dụng throttle, cho phép event listener kích hoạt mỗi 100 mili giây, thì hàm f sẽ chỉ được gọi 100 lần là nhiều nhất trong vòng 10 giây bạn scroll

**Using Lodash debounce**
<https://www.carlrippon.com/using-lodash-debounce-with-react-and-ts/>
```jsx
const debouncedSearch = debounce(async (criteria) => {
  setCharacters(await search(criteria));
}, 300);

async function handleChange(e: React.ChangeEvent<HTMLInputElement>) {
  debouncedSearch(e.target.value);
}
```

## Object.assign() and Object.create()

`Object.assign(target, source)` copy enumerable properties from `source` to `target` object.
`Object.create(source)` create a new object using the `source` as prototype

## `dangerouslySetInnerHTML`

`dangerouslySetInnerHTML` là 1 property của React mà có thể sử dụng cho HTML elements. `dangerouslySetInnerHTML` được dùng để báo cho React biết là content của 1 thẻ HTML là dạng dynamic (lấy từ input) và có thể chứa các thẻ HTML khác như là thẻ `<p>`, `<span>`, `<script>`

## Code splitting

Code splitting is the splitting of the code into various of bundles or components which can be loaded on demand or in parallel

## So sánh display: flex và display: grid

- Grid: hiển thị layout theo 2 chiều
- Flex: hiển thị layout theo 1 chiều

Khi nào sử dụng `flex`

- Các element con chia dựa theo content của chính các element đó

Khi nào sử dụng `grid`

- Các element được chia đều
- Parent quản lý vị trí các element con (dùng `grid-template-area`)

## Kiến thức bảo mật tối thiểu ở frontend

## Các cách check null và undefined

```js
let x;
if ( x == void 0) {}
```

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

- Gần giống như object, khác ở chỗ key có thể là bất cứ type nào, kể cả object, function
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
- Get size
```js
map.size
```
- Loop
```javascript
for( let [key, value] of map )
```

## Else if using operation tenerry

const a = b ? c : d ? e : f

## OOP

<https://kumartul.hashnode.dev/object-oriented-programming-explained-like-never-before>

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

## Rendering a web page

<https://blogs.halodoc.io/how-does-a-browser-actually-work/>

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

## Closure

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

## Primitive và Reference type

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

## Copy object

- Normal copy: point to the same reference. If you change one of them, the other will change as well.

```js
const a = {x: 0};
const b = a;
```

- Shallow copy: using object spread or `Object.assign({}, obj). However, if you modify the nested object property, the other will change as well

```js
const a = {x: 0, y: {z: 0}};
const b = Object.assign({}, a); // or b = {...a}
```

- Deeper copy: this won't change the other object if you modify the nested object property. However, this doesn't work if the original object has property as method

```js
const a = { x: 0, y: { z: 0 } };
const b = JSON.parse(JSON.stringify(a)); 
```

- Use `lodash.cloneDeep`

## Rules of hooks

- Only call Hooks at the top level. You shouldn't call `useState` in loop, conditions
- Only call Hooks inside React function components
- Whenever we have connected useState and useEffect hooks, it’s a good opportunity to extract them into a custom hook:
- `useState` doens't merge objects when the state is updated. It replaces them.
- Don't use `async` with callback function in `useEffect`. It can work fine if you have only one `useEffect`. However, if you have multiple `useEffect`, you could run into race conditon.

When there is an update action (ex: click event).

- When you call the `setState`, it might trigger an re-render. However, `setState` doesn't update the state immediately. React waits for all the `setState` in the event handlers to be finished before re-rendering. In another way, it groups multiple state updates into a single update. This is called batching (<https://github.com/reactwg/react-18/discussions/21>)
- You can pass a value or an updated function to the `setState`. All of them are added to the queue.
- When all the scheduled re-renders run
  - `useState` is called
  - React goes through all the queue, calculates and returns the updated value to `useState`

If you update the state hook to the same value as the current state, there will be no re-render to be scheduled.

## Scope chain

Có 3 loại scope: `global scope`, `local scope` (function) và `block scope` (scope nằm giữa {}). Khi một biến không được tìm thấy trong `local scope`, JS sẽ tìm biến đó ở các outer scope cho đến scope ngoài cùng là `global scope`.

## for-in and for-of

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
- Là 1 object bọc lấy kết quả trả về của 1 asynchronous function.
- Có 3 trạng thái:
  - pending: trạng thái mặc định khi mới tạo Promise.
  - fulfilled: trạng thái khi asynchronous function bên trong resolve,
  - rejected: trạng thái khi asynchronous function bên trong resolve,

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

## onChange và onInput khi sử dụng cho input

`onChange` chỉ chạy khi mà user ra khỏi (`blur`) input đó
`onInput` không chỉ chạy như `onChange` mà còn chạy khi gõ ('keystroke') hoặc paste

## `import` and `export`

### `export`

- export 1 biến hoặc hàm: `export a`
- export list: `export {a,b,c}`
- export default: 
  - `export default ChildComponent`: chỉ 1 có 1 default export duy nhất. Khi import ta chỉ cần viết `import ChildComponent from './abc.js'`
  - `export * from` hay còn gọi là re-export: `export {default} from './abc.js'`. Kĩ thuật này được sử dụng để gom các export từ các child component và export lại 1 lần nữa.  `*` ở đây là 1 object, chứa tất cả các export từ `./abc.js`, kể cả export thường lẫn export default. `{default}` là object destructure. `export * from` tương đương với `import * from` kết hợp với `export *`. Khi có nhiều child component cần re-export thì mình có thể viết như sau
  ```js
  export {default as a} from './a.js'
  export {default as c} from './c.js'
  ```

### `import`

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

## What happens when you type a URL into browser

<https://www.youtube.com/watch?v=AlkDbnbv7dk>

## Core Web Vital

<https://prateeksurana.me/blog/future-of-rendering-in-react/>
<https://i.imgur.com/jqIBleM.png>
<https://i.imgur.com/DCsCct2.png> 
<https://indepth.dev/posts/1498/101-javascript-critical-rendering-path>
<https://web.dev/learn-web-vitals/>

- Critical Rendering Path: <https://indepth.dev/posts/1498/101-javascript-critical-rendering-path> is the sequences of steps of: convert the HTML, CSS, and JavaScript (images are not considered as critical resource) into pixels on the screen
- DOMContentLoaded: the time for loading HTML before starting to load the content. The event does not wait for images, subframes or even stylesheets to be completely loaded. The only target is for the Document to be loaded
- Load: when all the resources are loaded ( resources are parsed and get acknowledged off before DOMContentLoaded)
- Speed Index: shows how quickly the contents of a page are visibly populated.
- Time to first byte (TTFB): the time it takes the browsers to start receiving the first byte of a request.
- First Contentful Paint(FCP): the moment when the first element from the DOM appears in the users' browser.
- Largest Contentful Paint (LCP): the time when the largest content element in the viewport becomes visible. It can be used to determine when the main content of the page has finished rendering on the screen.
- Time To (fully) Interactive (TTI): the amount of time it takes for the page to be fully interactive. Users might expect this is the same time as LCP, but this can be different. If TTI and LCP differ, users might assume the website is broken.
- First Input Delay (FID): the time from when a user first interacts with a page to the time when the browser is actually able to respond to that interaction.
- Total Blocking Time (TTB): total amount of time between First Contentful Paint (FCP) and Time to Interactive (TTI) where the main thread was blocked for long enough to prevent input responsiveness.

## Obtimize speed:

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

## Critical Rending Path

The critical rendering path includes the Document Object Model (DOM), CSS Object Model (CSSOM), Render tree, Layout, and Paint.
The document object model is created as the HTML is parsed. The HTML may request JavaScript, which may, in turn, alter the DOM. The HTML includes or makes requests for styles, which in turn builds the CSS object model. The browser engine combines the two to create the Render Tree. Layout determines the size and location of everything on the page. Once the layout is determined, pixels are painted to the screen.
Optimizing the critical render path improves render performance. Performance tips include:
- Minimizing the number of critical resources by deferring their download ( defer attribute), marking them as async (async attribute), or eliminating them altogether.
- Optimizing the number of requests required along with the file size of each request.
- Optimizing the order in which critical resources are loaded by prioritizing the downloading critical assets, shorten the critical path length.

## var and let, const

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

## Hoisting

It's the process that stores functions and variables in the memory before executing any code (creation phase);
- Functions are stored with the entire function. That's why you can invoke them before the line we declare them
- Variables is different.
  - Variables with the `var` keyword are stored with the value of `undefined`
  - Variables with the `let` and `const` keyword are stored `uninitialized`. If we try to access them before declare them, it will return `ReferenceError`

## Object.freeze() và Object.seal()

`Object.freeze()` không cho phép thêm, sửa, xóa properties. Tuy nhiên, `Object.freeze()` chỉ shallowly freeze, nếu properties là 1 object khác thì vẫn có thể sửa được
`Object.seal()` không cho phép thêm, xóa, nhưng vẫn cho sửa existing properties

## useMemo và useCallback. Có thể chuyển từ useMemo sang useCallback được không

<https://kentcdodds.com/blog/usememo-and-usecallback>
`useCallback` thường được dùng cho các event handler và event handler này là 1 prop của 1 child component và child component này dùng `React.memo`.
Mục đích dùng `React.memo` là child component không bị re-render khi parent component re-render. Tuy nhiên, nếu props của child component bị thay đổi thì nó vẫn bị render lại. Props đấy ở đây có thể là 1 event handler. Vì thế việc dùng `useCallback` ở đây là để child component hoàn toàn không bị re-render khi parent component re-render

Chuyển đổi giữa `useMemo` và `useCallback`
`useCallback(fn, deps)` tương đương với `useMemo(() => fn, deps)`.

## Check client or server side

```js
if ( typeof window !== undefined ) {
  // do something on client side
}
```

## CSS

## prefers-color-scheme

Đây là 1 tính năng của CSS `@media` để check xem thiết bị của bạn (máy tính hoặc điện thoại) đang sử dụng dark theme hay light theme

```CSS
/* Nếu hệ thống đang sử dụng dark theme thì đoạn code CSS trong này sẽ được chạy và ngược lại với light theme */
@media (prefers-color-scheme: dark) {

}
```

```JS
const prefersDark = window.matchMedia && window.matchMedia("(prefers-color-scheme: dark)").matches; // true if system is using dark theme
```

## counter

## `srcset` and `sizes`

<https://imagekit.io/responsive-images/>

```html
<img src="https://ik.imgkit.net/ikmedia/women-dress-1.jpg" 
     srcset="https://ik.imgkit.net/ikmedia/women-dress-1.jpg?tr=w-225 225w,
             https://ik.imgkit.net/ikmedia/women-dress-1.jpg?tr=w-300 300w,
             https://ik.imgkit.net/ikmedia/women-dress-1.jpg?tr=w-350 350w,
             https://ik.imgkit.net/ikmedia/women-dress-1.jpg?tr=w-640 640w"
     sizes="(max-width: 400px) 100vw, (max-width: 700px) 50vw, (max-width: 900px) 33vw, 225px">
```

First, we need to calculate the image size. For example, if the viewport < 400px, let's say 350px, the image size will be 100vw, which is 350px. Then, the browser will use this image https://ik.imgkit.net/ikmedia/women-dress-1.jpg?tr=w-350

If the viewport is 650px, the image size will be 325px, and the browser use https://ik.imgkit.net/ikmedia/women-dress-1.jpg?tr=w-350, which is the closest one

## fr in grid

## min, max, clamp()

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
- `clamp(min, value, max)`: thứ tự các giá trị cũng không quan trọng. Thường dùng để làm responsive cho font-size.
```CSS
p {
  font-size: clamp(2rem, 5vw, 5rem)
}
```
`font-size` sẽ được tính theo vw. 5vw là 5% width của view. Nếu `value` nằm giữa `2rem` và `5rem` thì `font-size` là 5vw. Nếu `value` < `2rem` thì trả về `2rem`, nếu > `5rem` thì trả về `5rem`.
1 lưu ý là nếu chỉ dùng 5vw thì font-size sẽ bị fix theo vw và trên mobile có thể bị bé. Khi dùng `clamp`, giá trị min của font-size sẽ luôn là 2rem
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

## Build URL with params

<https://www.valentinog.com/blog/url/>

## Create text on image with an overlay

Set z-index for the container

Using `background-blend-mode`: <https://www.smashingmagazine.com/2021/09/reducing-need-pseudo-elements/>

## `display: flex`

<https://www.joshwcomeau.com/css/interactive-guide-to-flexbox/>
<https://www.youtube.com/watch?v=1zKX71GYisE>

Mặc định `flex-grow: 0; flex-shrink: 1; flex-basis: auto;`
`flex-basis`: giá trị ban đầu của flex item để từ đó tăng hoặc giảm. Nếu flex-item có width thì `flex-basis` sẽ override width đấy. Note: `max-width` và `max-height` sẽ override `flex-basis`. `min-width` và `max-width` cũng ngăn chặn item bị shrink hoặc grow
`flex-shrink` và `flex-grow`: tính toán sự tăng giảm width của flex items

`flex-shrink: 1` sẽ không chạy đúng khi width của container giảm trong trường hợp

- item có min-width, vd input có default min-width: 170px - 200px tùy browser => set `min-width: 0` cho input
- item có content dài

`flex-shrink: 1`: Khi tổng width của các item > parent và `flex-shrink` = 1 => width của item sẽ bị thu lại
`flex-grow: 1`: Khi tổng width của item < parent và `flex-grow`: 1 => width của item sẽ tăng lên để fill hết width của parent

Khi container vẫn còn space hoặc container bị thu hẹp và các item đều được set `flex-grow` hoặc `flex-shrink` thì phần thừa hoặc thiếu đó sẽ được tính theo tỉ lệ `flex-grow` hoặc `flex-shrink` giữa các item

Ví dụ: extra space là 150px, item 1 có `flex-grow` là 1, item 2 có `flex-grow` là 2 => item 1 sẽ tăng 50px so với flex basis, item 2 sẽ tăng 100px so với flex basis. Tương tự với `flex-shrink`, khi container bị thu hẹp lại thì width của các item sẽ bị giảm theo tỉ lệ

`flex-wrap: wrap` sẽ tách row khi container width thu lại, từ 1 row sẽ thành 2, 3 row. `flex-grow` và `flex-shrink` sẽ tính toán theo từng row => width các item sẽ có thể bị thay đổi nếu row được tách ra

## So sánh `flex-basis` và width

`flex-basis` sẽ thay đổi theo chiều. Tức là khi `flex-direction` là `column` thì initial height của flex item sẽ là `flex-basis`

## `vmin`, `vmax`

`vmin` and vmax can change whilst the browser window is resized or the orientation of the mobile phone is changed.
`vmin` is the minimum between the viewport's height or width in percentage depending on which is smaller.
`vmax` is the maximum between the viewport's height or width in percentage depending on which is bigger.

## <!DOCTYPE html>

This is the doctype tag to let the browser know that this is HTML5 page and should be rendered as text/html

## <meta charset="utf-8">

This meta tag tells the browser which character encoding browser to use. UTF-8 is greate because you can use all sorts of symbols and emoji

## <meta name="viewport" content="width=device-width"

`width=device-width` tells the browser to use 100% of the device's width as the viewport => there's no horizontal scrollbar. You can commbine this attribute with `initial-scale` to 1 to let the user zoom arond.

## og meta tags

These og meta tags are used to display information of the post like the title, featured image... when you share them on social networks.

## `text-size-adjust`

If you don't have a responsive mobile website, and you open it on a small screen, so the browser might resize the text to make it bigger so it’s easier to read. The CSS `text-size-adjust` property can either disable this feature with the value none or specify a percentage up to which the browser is allowed to make the text bigger.

## Resource hint: preconnect, dns-prefetch, preload

<https://viblo.asia/p/thuat-ngu-trong-frontend-optimization-2oKLn2YgLQO>
<https://webpack.js.org/guides/code-splitting/>
<https://www.youtube.com/watch?v=6q75MVFLlok>
<https://blog.dareboost.com/en/2020/05/preload-prefetch-preconnect-resource-hints/>

Trước khi trình duyệt lấy được dữ liệu từ server, nó sẽ phải trải qua 3 bước cơ bản như sau:

- Kết nối tới DNS Server để phân giải tên miền của máy chủ
- Thiết lập kết nối TCP
- Mã hóa kết nối bằng giao thức TLS

Trong từng bước trên trình duyệt gửi một phần dữ liệu tới máy chủ, và máy chủ gửi lại một phản hồi. Hành trình này, từ máy chủ gốc đến địa chỉ cần đến và quay ngược trở lại, được gọi là khứ hồi (round trip). Mỗi bước sẽ có 1 hoặc nhiều round trip. Mỗi round trip khi được thực hiện lại có 1 độ trễ nhất định (latency). Qua đây chúng ta thấy rằng việc chờ đợi để thiết lập kết nối trước khi download data mình cần về khá tiêu tốn khá nhiều thời gian và chính điều này sẽ làm giảm tốc độ tải trang web

Mặc định browser sau sẽ parse HTML và CSS để biết mình cần tải data gì và tải chúng theo thứ tự. `preconnect` sẽ nói với browser rằng tôi đang thực sự cần tài nguyên này, hãy thực hiện các bước trên sớm nhất có thể. Và trình duyệt sẽ thực hiện 3 bước trên song song với các kết nối khác trước. Nếu không có `preconnect` thì tài nguyên này có thể phải đợi các kết nối khác xong trước và thực hiện lại từ đầu 3 bước trên.
`preconnect` thường được sử dụng để tăng tốc độ tải

`dns-prefetch` tương tự như `preconnect` nhưng chỉ thực hiện trước bước kết nối tới DNS Server. `dns-prefetch` có thể dùng chung với `preconnect` như là 1 dạng fallback khi 1 số trình duyệt không hỗ trợ `preconnect`.

`prefetch` báo cho browser biết để download trước 1 tài nguyên nào đó với low priority mà tài nguyên này có thể chưa cần sử dụng ngay. Lấy ví dụ ở 1 trang bán hàng, khả năng cao là khách sẽ đặt hàng và thanh toán trang thanh toán. Vậy nên ở trang chủ, mình có thể prefetch 1 vài script cho trang thanh toán, ví dụ checkout.js và cache script này vào trình duyệt. Khi sang trang thanh toán, script này sẽ được sử dụng ngay mà không cần thiết lập kết nối và down về

<https://indepth.dev/posts/1498/101-javascript-critical-rendering-path>
  
`preload` thì gần như là 1 nâng cấp của `preconnect`. Nó yêu cầu browser thiết lập kết nối và download tài nguyên đó càng sớm càng tốt, song song với các tải về HTML và CSS (`preconnect` phải đợi parse HTML và CSS trước). Tuy nhiên, độ ưu tiên của preload vẫn nằm sau các tài nguyên synchronous trong thẻ `head`. Ta thường dùng `preload` để load các tài nguyên không có trong HTML mà chỉ xuất hiện khi parse JS và CSS (VD: font). Việc dùng `preload` phải cẩn thận vì nó làm đảo lộn độ ưu tiên của browser. Chỉ dùng preload các file *above the fold*

The HTML attribute crossorigin defines how to handle crossorigin requests. Setting the crossorigin attribute (equivalent to crossorigin="anonymous") will switch the request to a CORS request using the same-origin policy. **It is required on the rel="preload" as font requests require same-origin policy.**

<https://web.dev/codelab-preload-web-fonts/#preloading-web-fonts>

Không nên lạm dụng resource hint vì chúng vẫn làm tiêu tốn tài nguyên CPU của cả client và server

## Race conditions

<https://wanago.io/2022/04/11/abort-controller-race-conditions-react>
A race condition is when your application have a sequence of events and their order is uncontrollable. This might occur with asynchronous

