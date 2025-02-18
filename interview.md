# Interview

<https://itnext.io/frontend-interview-cheatsheet-that-helped-me-to-get-offer-on-amazon-and-linkedin-cba9584e33c7#21a5>

## Coding Interview Project

<https://jantu.hashnode.dev/how-to-implement-infinite-scrolling-with-reactjs>

## Why frontend first

- It's easier to start with, you can see your result immediately on the browser
- Every app needs UI, you can have an app without any backend at all, but you cannot have an app with only database.

## What is clean architecture

Clean architecture means we can divide the application into different layer and have seperation of concern between them

## What do you think about JS, compares to other language (cons and pros)

Cons:

- Simple: It's easier to learn and to work with. JS can be run directly within browser, meaning that there's no need to compile the code before running it.
- Flexibility: You can declare variables without type.
- Popularity: JS is strongly supported and very popular
- Rich API: JS provides lots of APIs to make interactions, which enhance UI and UX

Pros:

- Security Issues: JavaScript is client-side scripting language which means the code runs on the user's computer. This can lead to security issues if not implemented properly.
- Too Flexible: JS is weak typed language => errors in run time. It doesn't even require any variable declaration. This is allowed due to hoisting.
- JS is executed during runtime without a compiler, if your JS code has error => your app can be crashed.
- Browser Support: The browser interprets JavaScript differently in different browsers. Thus, the code must be run on various platforms before publishing.

## Good and bad refactoring

<https://www.builder.io/blog/good-vs-bad-refactoring>

- Changing the coding style, ie using a new library that makes the coding style harder to understand
- Unnecessary abstractions: make the code more complex with a lot more abstractions
- Not understanding the code and the business before refactoring that the apps can works wrongly, slower. For example: refactor an heavily-SEO app using SPA

Good refactoring:

<img src="https://i.imgur.com/GoHF5ov.png">

## You have a slow website. Which steps will you take to identify the problem

- Run Basic Speed Tests: Use tools like Google PageSpeed Insights, GTmetrix, or WebPageTest to get an overview of your website's performance
- Analyze network performance: 
  - Use Browser Developer Tools: Inspect network requests in Chrome DevTools to see if there are any slow-loading resources, large files, or numerous HTTP requests.
  - Check for Redirects: Ensure that there aren’t unnecessary redirects adding to the load time.
- Look into the FE code to evaluate Frontend Performance
  - Open the console tab to see if there is any errors
  - Optimze assets (html, fonts, images), resize and reduce js and css files
  - Check if there is any unnecessary package in the bundler 
- Evaluate Backend Performance
  - Database Optimization
  - Evaluate Server Configuration
  
## Code review

- Understand the Context
  - Read the Description: Make sure you understand the purpose of the changes.
  - Understand the Requirements: Know what the code is supposed to achieve.
- Review the Code for Functionality. Verify that the code does what it's supposed to do and doesn't break any existing functionality.
- Check for Code Quality
  - Check if the code is not so hard to understand
  - Make sure the code follow the coding convention of the project
  - Check for Code Duplication
- If there are any issues that need to discuss, discuss it with the one who make the MR or bring it up to the team
- In case the code still has minor issues but the ticket is urgent, we should merge it first then create a TODO list or technical debt to address it later  

## Why you should put CSS in the `<head>` tag

<https://web.dev/learn/performance/optimize-resource-loading>

To avoid FOUC (A flash of unstyled content). The FOUC is you will see a flicker of page's content without any style. CSS is render-blocking by design. Placing any stylesheets in the <head> will block the HTML parser until the stylesheet is downloaded and parsed to construct CSSOM => so you will see the full HTML page with style

If you put your CSS, for example, in the end of body. The HTML parser is not blocking => FOUC

## Which data type to be stored in local storage

Non-sensitive data because local storage can be access via XSS attack

## Aria attriube, when to use, when not. Tool check accessibility

Tool check accessibility: 

- https://webaim.org/resources/contrastchecker/
- Chrome dev tool: check the contrast, also related to accessibility

- Don't Use ARIA Labels for native HTML elements cause they already have accessibility built in, and adding unnecessary ARIA labels can break accessibility.
- Do not change native semantics

## Quy chuẩn cho accessibility

## Process when taking a task

- Read the requirement carefully. If not, ask the BA or PM or client for clarification
- If the task is too big, break it into smaller chunks
- Make a plan or steps for each task: with input X, which steps we need for output Y (<https://2coffee.dev/bai-viet/archimedes-cung-loi-suy-nghi-dau-vao-dau-ra-trong-lap-trinh>)
- If the task requires a technology you've never done before, research or discuss with your team member to see if you can do it or you can meet the deadline
- Write test case
- Code
- Try to pass the unit test

## Compare SPA and NextJS. Why use NextJS over SPA

- Faster initial page load: it can pre-render the content on server side without waiting for bundling process like in SPA
- SEO friendly: NextJS pre-render the page before returning to client, so Google Bot can read and crawl the page, that is impossible in SPA
- Routing without any router packages
- Images optimization with `Image` component
- Code splitting and bundling

## Why React (over vanilla JS)

- Like other JS frameworks, you can break your app into components that can be reuseable
- It's declarative, we can describe how UI should look like by JS state. In vanilla JS, you need to handle both the state logic and UI interaction
- It's faster by using Virtual DOM. Virtual DOM is just objects, React update the real DOM via virtual DOM.

## Why use virtual DOM in React

<https://www.youtube.com/watch?v=za2FZ8QCE18&ab_channel=FrontStart>

The Virtual DOM in React is a lightweight copy of the actual DOM kept in memory. In fact, it's a tree of React Elements, or objects, which is the results of calling `React.createElement`. 

React uses Virtual DOM to enable the declarative styling of React. We tells React what UI we want through state, without having to directly manipulate the DOM. When there is change, React identifies the exact modified elements by comparing the current Virtual DOM snapshot with the previous one, so it updates only the necessary objects rather than creating a new DOM every time.

Using Virtual DOM, we can avoid two problems if we use the real DOM directly:

- When the DOM Tree structure becomes larger, it becomes difficult to manage. For example, there is now a large form component, and when changing a value, it may be necessary to traverse all the form component nodes and operate the DOM to change the value, which can easily result in unnoticed bugs during this process.
- Low efficiency. Although DOM operations may be fast, during rendering and reflow stages, frequent direct DOM operations are often the main cause of frame stuttering, so the more you directly manipulate the DOM, the more likely you are to hit performance bottlenecks.

## Redux and React Context

- <https://blog.isquaredsoftware.com/2021/01/context-redux-differences/>
- <https://formidable.com/blog/2021/stores-no-context-api/>

Redux is like using multiple React Context

React Context doesn't manage any state at all. When we use Context, we manage the state via React hooks `useState` and `useReducer`. The only purpose of React Context is to avoid **prop-drilling**

Context + `useReducer` relies on passing *the current state value via Context*. React-Redux passes *the current Redux store* instance via Context.
When `useReducer` produces a new state value, *all* components that are subscribed to that context will be re-rendered => lead to perfomance issue
In React Redux, components can subscribe to specific pieces of store state and only re-render when those values change.

Context + `useReducer` doesn't not have middleware to do side-effect
React + Redux have middleware support using Redux Thunk

if you get past 2-3 state-related contexts in an application, you're re-inventing a weaker version of React-Redux and should just switch to using Redux.

## What is state in React

It's the data that represents UI
In react, it's a variable which whenever it updates, it will trigger a re-renders of the component.

## Handle state

TLDR: Check if that state can be calculated from other state, if not, create new state.

When to create a new state

- State to save data from API
- Status state (loading, error status while loading data from API)
- State relates to event (boolean state when click on a button...);

## React hooks in simplest defination

- `useState`: create a value that is preserved across renders and triggers a re-render when it changes
- `useEffect`: synchronize a component with some external system
- `useRef`: create a value that is preserved across renders, but won't trigger a re-render when it changes
- `useContext`: get access to what was passed to a Context's Provider
- `useReducer`: create a value that is preserved across renders and triggers a re-render when it changes, using the reducer pattern
- `useMemo`: cache the result of a calculation between renders
- `useCallback`: cache a function between renders
- `useLayoutEffect`: synchronize a component with some external system, *before* the browser paints the screen

## What is state in React

It's the data that represents UI
In react, it's a variable which whenever it updates, it will trigger a re-renders of the component.

## What is rendering in React

- "Rendering" means that React is calling your component, which is a function
- Each render is a snapshot, like a photo taken by a camera, that shows what the UI should look like, based on the current application state.
- Each render create their own scope, they work individually.
- The point of a re-render is to figure out how a state change should affect the user interface. And so we need to re-render all potentially-affected components, to get an accurate snapshot.

## React element and React component

- React element is a JS object that describes a DOM node and its attributes or properties. It's created by calling `React.createElement`
- React component is a function (or class) that returns React element.
- JSX (`<App />`) is syntax sugar of `React.createElement`. When we write JSX, we are just using `React.createElement`

## Deep copy and shallow copy in JS

- <https://stackoverflow.com/questions/61421873/object-copy-using-spread-syntax-actually-shallow-or-deep>
- <https://anonystick.com/blog-developer/phong-van-su-khac-nhau-giua-shallow-copying-va-deep-copying-trong-object-javascript-2019112823755023>
- <https://developer.mozilla.org/en-US/docs/Glossary/Deep_copy>

To copy an oject in JS, we have some types of copy

- Shallow Copy: the original object and the new one has the same memory address (same reference)
  - Just assign old one to new one: `const newObj = oldObj`.
  - `Object.assign()` and `Object.create()`
- Deep copy: every value of the copied object gets a new memory address rather than the original object.
  - `JSON.parse(JSON.stringify(object))`: **NOT RECOMMENDED**. The new object will have different memory address and any changes on old object doesn't affect new one. This medthod cannot clone un-serializable data (like `function`) and it will turn your `Date` object into string, `NaN` into null. Turn your `Set`, `Map` into regular Object
  - `structuredClone`
- Hybrid
  - Spread operator: It deep copies if the object is not nested (the value of property must be primitive value). For nested data, it deep copies the topmost data and shallow copies the nested data. **The same goes for array**

  ```js
  const obj = {a:1,b:2,c:{d:3}};
  const shallowClone = {...obj};
  obj.c.d = 34;
  console.log(obj);
  console.log(shallowClone);
  ```

## Why we shouldn't directly call React Components

- <https://dev.to/igor_bykov/react-calling-functional-components-as-functions-1d3l>
- <https://kentcdodds.com/blog/dont-call-a-react-function-component>

When a functional component is used with JSX (or `React.createElement`), it will have a lifecycle and can have a state.

When a function is called directly as `Component()` it will just run and (probably) return something. The component doesn't exist in React tree elements

It could be dangerous if the component have state and is called directly. The reason relates to the number of hook in the parent component. You need to make sure that the hooks are always **called the same number of times for a given component**.

Let's say we have an `useState` in both parent and child component and calling `useState` in parent component will add more child component. In our case we are calling child function component, so there will be two `useState` in the parent component. Because number of child component can be changed => number of `useState` hook changes => error

## Why we shouldn't declare a component inside another component

That's because when the outer component re-renders, it clears the previous output and mounts the new output of inner component => the inner component will be reset (unmount and mount again) and lose all of its state.

## Micro Frontend

- <https://newsletter.systemdesign.one/p/micro-frontends>

## How to build a good component

- A "good component" is a component that I can easily read and understand what it does from the first glance.
- A "good component" should have a good self-describing name. Sidebar for a component that renders sidebar is a good name. CreateIssue for a component that handles issue creation is a good name. SidebarController for a component that renders sidebar items specific for 'Issues' page is not a good name (the name indicates that the component is of some generic purpose, not specific to a particular page).
- A "good component" doesn't do things that are irrelevant to its declared purpose. Topbar component that only renders items in the top bar and controls only topbar behaviour is a good component. Sidebar component, that controls the state of various modal dialogs is not the best component.

## Why react use immutability?

- Keep previous versions of the state, and reuse them later
- Create pure components. Immutable data can detect if changes have been made, which helps to *determine when a component requires re-rerendering*. If we mutate the state as object or array directly, React still sees that object or array as the same one because JS compares objects and arrays by reference, not value.

To detect a state change, React shallowly compares the old and new value state (similar to '==='). If you mutate the data (object and array) directly (using `Array.push` for example), React won't detect the change in state => won't re-render

## How do you scale large React component

## Benefit of `react-query` over `useEffect`

- <https://www.youtube.com/watch?v=Kjkx2BASAZA>
- <https://tkdodo.eu/blog/why-you-want-react-query>

Here are the reasons:

- We don't have to handle loading, error, and data state manually
- Caching data: the data returned from the request is cached until the `staleTime` has passed

## When does clean up function of `useEffect` run

- When the component unmounts
- When the `useEffect` runs again (because the dependancy changed or there is no dependency) => then it runs the clean up function of the previous effect.

## When to use `useCallback` and `useMemo`

Caching a function or value with `useCallback` and `useMemo` is only valuable in a few cases:

- You pass them as a prop to a component wrapped in memo. You want to skip re-rendering if the value hasn’t changed. Memoization lets your component re-render only if dependencies changed.
- The function or value you're passing is later used as a dependency of some Hook. For example, another function wrapped in useCallback depends on it, or you depend on this function from useEffect.
- The calculation of the value is slow

The downside of this unnecessary `useMemo` and `useCallback` is making the code less readable

## What is the use of `generic` in TS

- To enable types to act as parameters.
- When you start needing a generic is when you truly don't know what type is going to be passed into the function or you have things inside the function that rely on knowing that type

## Difference between `generic` and `unknown` in TS

We use `unknown` when we doesn't know the type of the argument yet. We cannot use `unknown` value directly as `any`, so we need to use type-checking to narrow down the type of that argument

With generic, the type is infer from value passed in the function

```ts
const Test = (a: unknown) => a
const a = Test(123) // a is unknown, you need to do type checking to use it

const Test2 = <T>(a: T) => a
const a = Test(123) // a is number
```

## Difference between `.ts` file and `.d.ts` file

- `.ts`: 
  - Code implementation + type definitions
  - Compiled into js file, even if there are only type definitions in the file (in this case the js file is empty and can be tree-shakeable) 
- `.d.ts`:
  - Only type definitions
  - Not compiled into js file

## Handle API in React

## Controlled vs uncontrolled component

- <https://phuoc.ng/collection/this-vs-that/controlled-vs-uncontrolled-components/>
- <https://www.jameskerr.blog/posts/partially-controlled-react-components/>

- Controlled component: the state of the component is managed by React state. When a user interacts with the component, its state changes and React will re-render the component with the new state
- Uncontrolled component: the state of the component is managed by the DOM. When a user interacts with the component, the DOM updates the state of component directly, bypassing React

## `useEffect` and `useLayoutEffect`

<https://phuoc.ng/collection/this-vs-that/react-use-effect-vs-react-use-layout-effect/>

The key difference between `useEffect` and `useLayoutEffect` is their timing. `useEffect` runs after the browser paint, while `useLayoutEffect` runs before the browser paints (after the rendering is done and React has updated the DOM)

`useEffect` runs after paint because most types of effect shouldn't block the browser from updating the screen

It's worth noting that `useLayoutEffect` runs synchronously, which means it can potentially slow down the browser painting if it takes too long to execute. In general, only use `useLayoutEffect` when you need to measure or modify something in the DOM before the browser painting

## What is Temporary Dead Zone

It is the area where a variable is inaccessible until it is declared.
The variables declared with `let` and `const` are also hoisted like `var`, they are both saved in memory before the code executions however they are not initialized
When we try to access those variables before they are declared, JavaScript throws a `ReferenceError`

## Why do we need to put side effects in the `useEffect`

With `useEffect`, we can control how the side effects run in the component. Normally, the side effects make the state change. So, if you put them outside the `useEffec` hook, it will lead to infinite re-render of the component.

## Why the callback in `useEffect` mustn't be async function

Because async function return promise. `useEffect` hook isn't expecting us to return a promise. It expects us to return either nothing or a cleanup function.

## React folder structure

<https://dev.to/josemukorivo/how-i-approach-and-structure-enterprise-frontend-applications-after-4-years-of-using-nextjs-2f5>

<img src="https://i.imgur.com/oEhdERi.png">

## When react does not re-render

- <https://www.zhenghao.io/posts/react-rerender>
- <https://blog.isquaredsoftware.com/2020/05/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior/>

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

- <https://www.zhenghao.io/posts/react-rerender>
- <https://www.developerway.com/posts/components-composition-how-to-get-it-right>
- <https://www.developerway.com/posts/how-to-write-performant-react-code>
- <https://www.developerway.com/posts/react-re-renders-guide>
- <https://www.joshwcomeau.com/react/why-react-re-renders/>

- When props or state have changed
- When parent component re-renders, even if the props pass to child component is memorized
- When a component uses context and the value of its provider changes. Context is sorta like "invisible props", or maybe "internal props".
- Component use custom hook and the state of that hook changes

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

- <https://beta.reactjs.org/learn/render-and-commit>
- <https://dev.to/teo_garcia/understanding-rendering-in-react-i5i>

<img src="https://i.imgur.com/7W6SqA9.png" />

3 phases:

- Trigger a render: there are two reasons for a component to render
  - Initial render: calling `ReactDOM.render` to render the root component. This is the initial render
  - Re-render: the component's states has been updated via event, etc...
- Render: React calls your components to figure out what should be on the screen.
  - During the initial render, React generates the virtual DOM for all the elements
  - During the re-render, React will compare the previous Virtual DOM with the current Virtual DOM side by side, line by line [https://www.youtube.com/watch?v=za2FZ8QCE18&ab_channel=FrontStart](https://www.youtube.com/watch?v=za2FZ8QCE18&ab_channel=FrontStart). It won't do anything with that information until the next phase, the commit phase.
- Commit: React commit changes to the DOM
  - For the initial render, React inserts the real DOM nodes from virtual DOM nodes into the document.
  - For re-renders, **React only changes the DOM nodes if there is a difference between renders**. If a component re-renders, it only changes the updated DOMs (that could contains updated data), the other DOMs in that component are still the same
- After rendering is done and React updated the DOM, the browser will repaint the screen. This process is also known as 'browser rendering'

## How React update UI

<https://www.youtube.com/watch?v=7YhdqIR2Yzo>

By a process called reconciliation

All React does is create a tree of elements (React elements are object that is the result of calling `React.createElement`). This tree of elements is called `Virtual DOM` and is kept in the memory.

Once virtual DOM has updated, React compares the entire virtual DOM with a virtual DOM snapshot that was taken right before the update.
It only finds the elements that has changed. If there are a lot of updates, React can batch them (collect the updates) for efficency.
Then React syncs the virtual DOM with the real DOM (this process is commit)

## React bailing out of updating states

<https://stackoverflow.com/questions/58208727/why-react-needs-another-render-to-bail-out-state-updates>

Nếu giá trị state không thay đổi khi setState thì component sẽ render thêm 1 lần nữa. Đến lần setState thứ 3 thì nó dừng hẳn không re-render tiếp.

## Preventing setState errors on unmounted components or race condition

**Update**: the warning has been removed from React version 18.

<https://www.developerway.com/posts/fetching-in-react-lost-promises>
<https://www.digitalocean.com/community/tutorials/how-to-handle-async-data-loading-lazy-loading-and-code-splitting-with-react>

Let's say you have two components, parent and child component.
Parent component has a button to unmount the child component. Inside child component, we call API and update a state inside. When the child component is requesting API, click the toggle button to unmount child component, then there is a chance that you will encountered an error: `Can't perform a React state update on unmounted component`. That's because your asynchronous request hasn't been resolved yet when the component is unmounted. So, when the request is resolved, it update the state of unmounted component.

Another case is when we type into an input and call API. If we type too fast, there would be multiple request to be sent, the result might come in different order than you expected => race condition.

To fix the problem, we have two methods:

- Ignore the `setState` by using a variable to store the mounted state. The request is still resolved but, it won't make any `setState` to unmounted component.

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
       mounted = false; // mounted here has the value of previous render. So if we type too fast, we won't have multiple setState, because the setState of previous render will never run
    }
}, [name])
```

- Cancel the request inside `useEffect` using `AbortController`

```js
useEffect(() => {
  // create controller here
  const controller = new AbortController();

  // pass controller as signal to fetch
  fetch(url, { signal: controller.signal })
    .then((r) => r.json())
    .then((r) => {
      setData(r);
    })
    .catch((error) => {
    // it's a real error, not because of AbortController
    if ( error && error.name !== 'AbortError') {
      // do something,
    }

  return () => {
    // abort the request here
    controller.abort(); // If the dependancy changes, this callback is call, controller will abort the previous request send.
  };
}, [url]);
```

## Why we cannot write `if`, `else`... in JSX

It's because `if` is not an expression in JS

## React flushSync

<https://beta.reactjs.org/learn/manipulating-the-dom-with-refs#when-react-attaches-the-refs>

Normally, in React, state updates are queued. That means `setState` doesn't update the state immediately. `flushSync` force React to update ('flush') the DOM synchronously.

## Authentication security

<https://blog.openreplay.com/11-authentication-mistakes-and-how-to-fix-them>

## How can we implement security for a website

Implementing security for a website involves multiple layers, from client-side protections to securing server-side operations and database management. Below is a general outline of these security layers along with suggestions for each. I'll also describe some diagrams to illustrate these concepts.

1. Client-Side Security

- XSS Protection: Use Content Security Policy (CSP) headers and libraries like DOMPurify to sanitize user inputs to prevent Cross-Site Scripting (XSS).
- Authentication & Authorization: Ensure that authentication tokens (such as JWTs) are securely stored in HTTP-only cookies and not in local storage to avoid exposure to XSS attacks.
- HTTPS & Secure Cookies: Use HTTPS to protect data in transit and enable the Secure flag on cookies. Ensure sensitive cookies are HttpOnly to prevent JavaScript access.
- CSRF Protection: Implement CSRF tokens to protect against Cross-Site Request Forgery attacks.

+-------------------+      XSS Prevention     +------------------------+
| User Interaction  |-----------------------> | Sanitized HTML Content |
| (User Input)      |                         +------------------------+
+-------------------+
        |
        v
+-------------------+      HTTPS/TLS      +---------------------+
|   Client Browser  |-------------------->|   Server            |
|                   |                     +---------------------+
+-------------------+

2. Server-Side Security

- Input Validation: Use a strict validation schema (e.g., with Zod) to ensure only expected data is processed by the server.
- Rate Limiting: Prevent brute-force attacks by setting rate limits on login endpoints.
- Authentication: Implement robust authentication using OAuth or JWTs with short expiration times. Also, use refresh tokens securely.
- Authorization: Define roles and permissions within the application to control access to specific resources.
- Data Encryption: Encrypt sensitive data at rest using AES-256 encryption, and consider using hashing algorithms (such as bcrypt) for passwords.
- Error Handling: Avoid exposing stack traces and detailed error messages that could help attackers identify vulnerabilities.

+---------------+      Data Validation      +-------------+
|  Client       |-------------------------> |    Server   |
+---------------+                           +-------------+
       |                                         |
       v            Rate Limiting                v
+---------------+  (e.g., Login)        +-----------------+
| Authentication|---------------------->| Authorization   |
+---------------+                       +-----------------+
       |                                         |
       v                                         v
+----------------+       Encrypted Data       +-----------------+
| Database       |<-------------------------->|   API Responses |
+----------------+                             +-----------------+

3. Database Security

- Access Control: Limit database access to the application server and restrict permissions.
- SQL Injection Prevention: Use parameterized queries or ORM libraries to prevent SQL Injection.
- Data Encryption: Encrypt sensitive data fields at the database level.
- Backup & Auditing: Regularly back up data and maintain an audit log of database access.

+-------------------+       Limited Access       +---------------------+
|   Application     |--------------------------->|     Database       |
|   Server          |                            |                    |
+-------------------+                            +---------------------+
      |                       Encrypted Fields           |
      |<----------------------------------------------->|
      |                       Regular Backups            |
      +------------------------------------------------>+

4. Network and Infrastructure Security

- Firewall & Network Segmentation: Use firewalls to restrict access to certain IPs and segment the network to isolate sensitive components.
- DDoS Protection: Set up Distributed Denial-of-Service (DDoS) protection, e.g., with services like Cloudflare, to mitigate large-scale attacks.
- Intrusion Detection: Implement Intrusion Detection Systems (IDS) to monitor for malicious activity.

+-------------------+    Firewall &    +------------------+
|     User          |----------------->| Web Application  |
|                   |                  |   Server         |
+-------------------+                  +------------------+
         |                                     |
         v                                     v
+------------------+        IDS        +---------------------+
|   Network Router |-----------------> | Database Server     |
+------------------+                   +---------------------+

5. DevOps & Deployment Security

- Code Security: Use static code analysis tools to scan for vulnerabilities.
- Dependency Management: Regularly update libraries and dependencies to address known vulnerabilities.
- Environment Management: Avoid hard-coding sensitive data and use environment variables for secrets.
- Logging & Monitoring: Use centralized logging to monitor application behavior and identify suspicious activity.

## Why list item need a unique `key`

- <https://kentcdodds.com/blog/understanding-reacts-key-prop>
- <https://www.youtube.com/watch?v=za2FZ8QCE18&ab_channel=FrontStart>
- <https://tigerabrodi.blog/key-optimization-in-react>

React uses `key` to keep track of which items have changed, are added, or are removed. If the key of item retains between re-renders, React will just re-use the component instead of re-creating it from scratch => better perfomance.

Let's say you have a list of items. Under the hood, React converts the items into React elements with the same type. Without `key`, there is no way to distinguish them. When we have an unique key for each item, React can differentiate and also track them when they get deleted, added or re-ordered.

When the `key` props is changed, React unmount the previous instance, and mount a new one. **The state of the component belongs to the key**, so all states that have existed in the component at the time is completely removed and the component is reinitialized. So, when an list item has a state and we are using `index` as the `key`, then we re-order the list the state of the item will not re-ordered as well

In this example <https://www.developerway.com/posts/react-key-attribute#part3>, after re-order, the component with key='au' still takes 'Australia' for the country props as before. That's why when we use `memo`, this component isn't re-rendered because the props doesn't change. If we are not using `memo`, the list item still re-render no matter which type of `key` we use because the component re-renders

Key must be unique. You shouldn't take `index` as key when you add or remove items (at the top of the list), the index (or key) of all items will be changed => bugs

Using `index` as `key` is a good idea: 
  
- When your list is static, we won't do anything on it
- When the data of the list is changed on every render and you are not modifying the list, eg: pagination

## How to approach a design

## The problem with `Array.prototyp.includes()`

`includes` will look for every single element in the array => the larger array, the longer it takes to find an element.

Solution: use `new Set()` instead of array, then we use `has()`, which is the built-in method of `Set`

```js
const arr = new Set([1,2,3])
arr.has(1) // return true
```

## Why the result of `setState` is not sync

Because React use batch updating. That means React groups all the `setStates`, put them into queues, execute them together within only one re-render. If `setState` is sync, we cannot batch them in only one re-render

## Do client component render only on client

<https://www.pronextjs.dev/do-client-components-render-during-ssr>

Client components render both on client and server. Components would be run first on the server to generate the HTML that is sent back during server side rendering, then again on the client during client-side "hydration". And hooks like `useEffect` or `useLayoutEffect` will only run on the client.

## Why not just fetch all the data from the client?

- It's faster: NextJS servers are usually deployed in the same Virtual Private Cloud (VPC) as the microservices they call. This means that the network latency between the NextJS server and the microservices is very low
- API Keys are hidden

## Return a promise inside `fetch` = `await`

Return a promise here is equal to `await wait(1000)`
```js
wait(1000)
  .then(() => {
    console.log('2');
    return wait(1000);
  }).then(() => {
    console.log('1');
    return wait(1000);
  })
```

## Optimize React re-render

<https://kentcdodds.com/blog/optimize-react-re-renders>
Mỗi khi render 1 component nào đó, props object của component đó sẽ được làm mới hoàn toàn, kể cả khi property của prop objects đó không đổi

## Why use Vite over CRA, why it's fast

- <https://semaphoreci.com/blog/vite>
- <https://www.youtube.com/watch?v=M_edImKoEt8>

CRA use webpack for bundling. Webpack bundles the entire application code before it can be served. With a large codebase, it takes more time to spin up the development server and reflecting the changes made takes a long time.

Unlike CRA, Vite does not build your entire application before serving, instead, it builds the application on demand using native ESM.

Benefits of using native ESM

- There is no need for bundling. They are understood directly by browser
- Native EMS is on-demand by nature. Based on the browser request, Vite transforms and serves source code on demand. If the module is not needed for some screen, it is not processed.

Vite divides the application modules into two categories: dependencies and source code.

- Dependencies: Plain JavaScript that will not change much during development (e.g. component libraries such as MUI).
  - Vite pre-bundles these dependencies using `esbuild`, which is 10-100x faster than JavaScript-based bundlers.
  - Pre-bundling ensures that each dependency maps to only one HTTP request, avoiding HTTP overhead and network congestion.
  - As dependencies do not change, they can also be cached and we can skip pre-bundling.
- Source code: JSX, CSS, Vue/React and other components that require conversion. Vite serves source code using `native ESM`.

Dependency pre-bundling only applies in development mode, and uses esbuild to convert dependencies to ESM. In production builds, @rollup/plugin-commonjs is used instead.

## What is side effect

Side effect is anything that affects something outside the function scope, ex: network request, updating the screen, starting an animation, changing data. They are things happens on the side, not during rendering

## Is react re-render bad? When is it bad and when is isn't?

Only optimize when the re-render makes your app slow.
When re-render is bad?: when a component has complex calculation or includes charts.
Why we shouldn't pre-optimize (with `useMemo` and `useCallback`)

- Your code is more complex
- React is very fast. If your component is pure, rendering your component one time or multiple times shouldn't cause any differences on the actual UI.

## What is pure function

Pure function is a function:

- That doesn't change any objects or variables outside function scope
- Given the same input, returns the same output

## Declare React hook rule

- Don't declare Hooks inside loops, conditions, or nested functions. Because React requires the order of Hook calls are the same between re-renders. If we put a hook calls inside an `if` statement, this order will be changed
- Don't declare Hooks from regular JavaScript functions. So don't declare hooks inside callback function of `useEffect`

## Error Boundary

Is React error boundary component that catch errors inside React component

## Why we shouldn't use `@import` in CSS

This method causes the browser to load each CSS file sequentially, rather than in parallel. Since CSS is render-blocking, by default, this is likely to affect page performance.

## Can you explain the role of HTML tag, HTML elements, window, and HTML document?

HTML document is HTML file. When an HTML document is loaded into a web browser, a `document` object appears. `document` object represents the whole html document. `document` is a property of `window` object. `window.document` === `document`. For HTML document, `document.documentElement` returns root element (<html>)

HTML elements are part of DOM which includes HTML opening tags, closing tags, attributes or text content
HTML tags are name of HTML elements

## Explain the difference between Async/Await and Promises

- `Async await` is syntactic sugar for `promises`. Making code looks like executed synchronously and easier to understand
- Debugging is much easier with `async/await` using `try catch` block. In promise, we have to use `then catch` chaining. In case we have errors, if we don't handle, the script dies

## Explain how you can use JavaScript functions such as forEach, Map

- `map` is used to transform each element of an array. It returns an array without changing original array
- `forEach` is used to run a function for each element. For example, we have a variable and we want to sum each element with that var. `forEach` returns undefined

## Explain the difference between Flexbox and CSS Grid. When would you use any of these? What are their strengths and weaknesses?

The basic difference between CSS Grid Layout and CSS Flexbox Layout is that flexbox was designed for layout in one dimension - either a row or a column. Grid was designed for two-dimensional layout - rows, and columns at the same time

If we want a FLEXIBLE layout (the size of the content decides how much individual space each item takes up), we use flex.
If we want a more fixed layout (you create a layout and then you place items into it) and you want to control the alignment by row and column, we use grid.

## What if we put anchor tag into `head` and put `title` or `link` tag into body

- When we put the tags used in `body` tag into `head`, they will be moved back to the very top of `body` tag
- When we put `title` or `link` tag in the body, it still works but not recommended.

## Can you describe the key principles of accessibility in web development, and how you would apply these in a front-end project

## one-way binding and two-way binding and unidirectional data flow

- One-way binding (One-way data binding) means the data (or state) flows in one direction, state updates lead to updates in view or vice versa. Ex: you have a count state and you display it somewhere. When count state updates, the view for count state updates as well.
- Two-way binding means data flows in both direction. Ex: you have an input in view, and a state. When you type into input, the state changes accordingly (view updates => state updates). Then you assign this state to `value` attribute of input and the value of input changes when you type as well (state updates => view updates, or you can display the state somewhere else, that could be also state updates => view updates)
- Unidirectional data flow: In React, it also means the data is passed from parent to child component and not vice versa

## Which property is not Browser Object Model(BOM) ?

Document

## How would you work through a new codebase at work?

- Understand the product before looking into the code: learn how to use it first
- Reading through codebase folder structure
- Start reading through recent pull requests since it's a good way of understanding what the team has been working on lately.
- After that, start with small ticket I can begin working on.

## Which resources are loaded first when accessing a website

HTML loaded first. When it's parsed, resources in `<head>` tag are loaded, then resources in `body` tag

The lifecycle of an HTML page has three important events:

`DOMContentLoaded` (`document.addEventListener("DOMContentLoaded")`)– the browser fully loaded HTML, and the DOM tree is built, but external resources like pictures <img> and stylesheets may not yet have loaded.
`load` (`window.onload`) – not only HTML is loaded, but also all the external resources: images, styles etc.
`beforeunload`/`unload` (`window.beforeunload`/`window.unload`) – the user is leaving the page.

## What is load balancing and how it works?

Load balancing is the process of distributing traffic among multiple servers to improve a service or application's performance and reliability. Load balancing is handled by a tool or application called a load balancer. A load balancer can be either hardware-based or software-based

## `useEffect` instead of `getServerSideProps`, what's the problem?

`useEffect` will run after your component mounted, therefore you will notice your page loads then the data appears.
`getServerSideProps` will make your page load with the data already on it, so you won't notice a flash of data appearing after the page already loaded.

## Should we set props value as initial state

<https://vhudyma-blog.eu/react-antipatterns-props-in-initial-state>
This is anti pattern because `useState` hook initializes the state only once - when the child component is rendered for the first time. So, the state in child component will not be updated when prop value from parent updates

## React Portal

Cho phép render 1 component vào 1 DOM node nằm ngoài root.
Why: để tạo ra các component mà không bị ảnh hưởng bởi style của component cha vì component cha có thể bị overflow hidden
Trong 1 App react thông thường, toàn bộ UI (components) sẽ được render trong 1 the div với id là root `<div id="root"><div>`
Giả sử ta muốn tạo 1 cái modal và thẻ div bọc lấy modal này nằm ngoài root. Và vì thẻ div này nằm ngoài root, ta không thể tạo component và render bình thường được mà phải dùng React Portal.

## Why you shouldn't put refs in a dependency array

<https://www.epicreact.dev/why-you-shouldnt-put-refs-in-a-dependency-array>

The purpose of a dependency array is to trigger the `useEffect` callback when there are changes in the values provided each time the component renders. However, React can't know that the value has changed if that change doesn't trigger a render. `ref` value doesn't trigger re-render, so it doesn't trigger the `useEffect` callback as well

Here is the rule for `useEffect` dependency array: Anything you use in your effect callback that won't trigger a re-render when updated should not go into the dependency array.

## JavaScript Promises: then(f,f) vs then(f).catch(f)

The difference happens when the `success` callback returns a **rejected promise**, like `Promise.reject('Invalid')`:

`then(f,f)`: `error` callback is not invoked
`then(f).catch(f)`: error callback is invoked

## Loop with promises

Using `async/await`

```js
const promise1 = new Promise(res => {
  setTimeout(res, 1000, 'first');
});
const promise2 = new Promise(res => {
  setTimeout(res, 2000, 'second');
});

(async () => {
  for (const item of [promise1, promise2]) {
    console.log(await item);
  }
})();
```

## Can `catch` in `try...catch` catch errors using `throw Error()` or `reject()` in Promise

**UPDATE**: I've tried in the latest version of chrome and firefox and it works 

<https://catchjs.com/Docs/AsyncAwait>
<https://javascript.info/try-catch>

No.
If we `throw` an error in Promise or using `setTimeout`, `try...catch` cannot catch the error. Because the callback in promise and `setTimeout` is executed later in different call stack, when `try...catch` is already done.

```js
async function fails() {
  throw Error();
}

function fails2() {
    return new Promise((resolve, reject) => {
        reject(new Error());
        // Or using setTimeout
        setTimeout(() => reject(new Error()), 1000)
    });
}

try {
  fails2();
} catch (error) {
  // Error here cannot catch
  console.log('error', error);
}
```

To catch that, we need to use `await`

```js
(async function () {
    try {
        await fails2();
    } catch (e) {
        console.log("that failed", e);
    }
})();
```

## Difference between `throw Error()` and `reject(new Error())`

<https://catchjs.com/Docs/AsyncAwait>

`reject(new Error())` just a function call, so it doesn't break the execution flow like `throw` does. This means you can write code that both rejects and resolves, like this:

```js
// Reject is call first because it lies above `resolve`
// If `resolve` is called fisrt, and we throw, `throw` wont be called
function test() {
    return new Promise((resolve, reject) => {
        reject(new Error());
        resolve("great success");
    });
}

test()
  .then(val => console.log(val))
  .catch(err => console.log(err));
```

## Rule for `this`

- <https://javascript.info/arrow-functions>
- <https://blog.tusharcodes.tech/5-rules-to-master-this-in-javascript>
- <https://i.imgur.com/tUjhc3r.png>
- <https://unicorn-utterances.com/posts/javascript-bind-usage>

**TLDR**
By default, for method in class using normal function, JavaScript looks at the class that uses the `this` keyword, not the class that creates the `this` keyword. On the other hand, Arrow function method will bound `this` to original scope (original class) and cannot be modified (even if you use `bind`)

For normal function:

- If the `new` keyword is used when calling the function, `this` inside the function is a all new empty object.
- If `apply`, `call`, or `bind` is used, `this` inside the function is the object that is passed in as the argument.
- If a function is called as a method `this` is the object that the function is a property of.
- If a method is assigned to a variable, then that method is detached from object and `this` is `undefined` in strict mode or `window` as global object. Example: `this` will be `undefined` in controller class because when you execute a method of that class in route, the method is detached from controller class <https://stackoverflow.com/questions/45643005/why-is-this-undefined-in-this-class-method>
- If a function is invoked as a free function invocation, `this` is the global object.

For arrow function:

`this` value in arrow function will ignore all the rules above and be the context of its surrounding scope at the time it is created (closest parent function). Arrow function will try to resolve `this` inside it lexically just like any other variable and ask the Outer function - Do you have a `this`? And Outer Function will reply YES and gives inner function its own context to this

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

## Normal function and arrow function

- `this` keyword: in arrow function, `this` value equal to the value of `this` in the context where the arrow function is created. Meanwhile, in normal function, `this` value depends on the object that call the function

```js
class Person {
  constructor() {
    this.name = 'viet'
  }
  getName() {
    setTimeout(() => {
      console.log(this.name); // return 'viet' because `this` in arrow function refers to the definition of `this` in the closest parent function
    }, 1000)

    setTimeout(function() {
      console.log(this.name); // return 'undefined' because the callback is executed by window
    }, 1000)
  }
}

const p = new Person();
p.getName();

const person = {
  wrapper() {
    return () => {
      console.log(this);
      return () => {
        console.log(this);
      };
    };
  },
};

person.wrapper()()(); // return person
```

- Cannot call `new` with arrow function
- There is no `argument` variable in arrow function
- We can use arrow function to preserve the `this` value of that function, because `this` value inside the function can be unpredictable because it is determined by the execution context. Using `bind()` does the same 

## Stack and heap

- In JS, stack is where to store primitive value (string, number, boolean, null, undefined, bigint, Symbol)
- On the other hand, heap is the place to store dynamic value (object, array)

## Các cách authentication

<https://viblo.asia/p/cac-phuong-thuc-pho-bien-dung-authentication-naQZRPGP5vx>

## Why use semantic HTML

- Better for SEO
- Better accessibility

## How browser works

<https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work>

## Event, Event handler, Event listener

**Event** là 1 hành động, 1 sự kiện nào đó khi user tương tác: click chuột vào 1 phần tử, submit 1 form nào đó
**Event handler** là 1 đoạn code, 1 hàm được chạy khi event xảy ra
**Event handler** còn được gọi là ***event listener***

## Concurrent mode in React

- <https://3perf.com/talks/react-concurrency/>
- <https://vercel.com/blog/how-react-18-improves-application-performance>

Help to build components that render concurrently without blocking the user interface.

Using concurrent features, React can pause and resume the rendering of components based on external events such as user interaction. When the user started interacting with ComponentTwo, React paused the current rendering, prioritized and render ComponentTwo, after which it resumed rendering ComponentOne (using Suspense)

We mark the UI update as 'non-urgent'. What does 'a non-urgent update' mean?

With React 17 and below, every update that happens in the app is considered urgent. If you click a button, React has to handle the update immediately. If you type into the text field, React has to rerender the list of notes immediately.

With React 18, your updates now can have a priority. Every update you make in the app is still, by default, urgent.
But what React now also supports is non-urgent updates. And non-urgent updates don't block the page – no matter how long they take. So what types of action are considered as 'non-urgent'.

Let's say we have filter input and a list. When we type into the input it's urgent update, the re-render of the list is not. We don't want the input to be laggy but the list can because we can wait for the list to be re-rendered while we typing with a loading indicator

**Drawbacks**

- non-urgent updates take longer. This is because React has to yield the control back to the browser all the time, which introduces some delays.
- Extra CPU cost
- Doesn’t help with expensive components

### `useTransition`

- <https://andreigatej.dev/blog/the-underlying-mechanisms-of-reacts-concurrent-mode/>
- <https://blog.bitsrc.io/understanding-transition-in-react-18-using-usetransition-hook-8639d6ced0f5>

If you want to create a transition between two different components' rendering, this hook provides a way to handle that, allowing you to defer the next component's rendering until the previous one is complete. It's like debouncing the UI update when the state update or update the state without blocking the UI

It tells React to keep the current UI interactive while preparing the new state in the background without committing the updates immediately (of course, you can display a loading when `isPending` is true)

## React design patterns

<https://blog.openreplay.com/applying-design-principles-in-react/>

## Màn bình thường và màn retina

Màn thường: 1 CSS pixel tương đương 1 điểm ảnh vật lý
Màn retina: 1 CSS pixel tương đương 4 điểm ảnh vật lý

## What is CDN

The primary purpose of any CDN (Content Delivery Network) is to reduce latency and deliver content to the end user as quickly as possible.

A CDN provider will have several servers in different geographical locations. Those servers can store a copy of your static resources and send them to the user when the browser requests them.

## Headless website

Headless website is built with separated back-end and frontend. With headless, your frontend (head) is decoupled from the backend (body).
Your frontend is a JS application that run by browser, and it doesn't need to be hosted on a server. You just need to put your code somewhere browser can download it (Often it's just on CDN)

Whereas, your backend can be WordPress or NodeJS and hosted on a server.

If needed, frontend part will communicate to the backend end via API (using REST or GraphQL) to get the data or perform other actions such as sending email

## Handle `this` in `setTimeout`

<https://developer.mozilla.org/en-US/docs/Web/API/setTimeout#the_this_problem>

- USE A WRAPPER FUNCTION or `bind` or `call`

```js
const myArray = ["zero", "one", "two"];
myArray.myMethod = function (sProperty) {
  console.log(arguments.length > 0 ? this[sProperty] : this);
}

// USE A WRAPPER FUNCTION
setTimeout(function () {
  myArray.myMethod();
}, 2.0 * 1000);

// Or using bind
setTimeout(
  myArray.myMethod.bind(myArray)
, 2.0 * 1000);
```

## Cơ chế làm việc của session và JWT

<https://viblo.asia/p/cach-trien-khai-refreshtoken-va-freshtoken-p1-dung-luu-tat-ca-phien-cua-nguoi-dung-3Q75wN23lWb>

- Session

Khi client tạo 1 request login tới server, server sẽ tìm trong database xem user đó có hợp lệ không, nếu oke thì lấy các quyền mà user đó có, rồi tạo 1 session lưu ở server (có thể lưu ở ram hoặc database) và trả về cho client 1 cookie (chứa session id (không trùng lặp)). Với mỗi request, client sẽ cầm theo cookie, server sẽ dựa vào id trong đó để tìm trong sổ session, coi cái nào khớp thì coi có quyền gì mà xử lí, không có thì bắt login lại.

Khi user không tương tác với server trong 1 khoảng thời gian thì session của user đó sẽ timeout (session timeout) hoặc khi cookie lưu phía client expire thì phiên đăng nhập của user hết hạn và người dùng sẽ bị đăng xuất

- JWT

Không lưu session, cấp cho người dùng 1 tờ giấy uỷ quyền (token), trên đó có thông tin/quyền user được phép, có chữ kí và ngày hết hạn. Mỗi lần truy cập vào đâu ông bảo vệ không cần tra cứu sổ sách gì cả, cứ nhìn giấy uỷ quyền hợp lệ là cho vô.

Các vấn đế của JWT

- User A authen thành công ➜ server response token ➜ User A bị Man-in-the-middle attack, do User A bất cẩn làm token rơi vào tay hacker ➜ hacker chiếm quyền truy cập account của user A mà server không hề hay biết
- User A đang truy cập các private API, đột nhiên token expired, màn hình đột ngột bị rediect sang trang login vì server response 403
- server chưa cài đặt cơ chế hủy token, phía database thì account user A đã bị ban, nhưng token vẫn đang lưu rải rác tại các client (desktop app, mobile app, web, server khác, ...), client vẫn cứ gửi token, server decode payload token thấy vẫn còn hạn sử dụng, phần mã hash chữ ký vẫn hợp lệ ➜ user vẫn author thành công

Cách logout jwt

- Khi gọi API logout thì lưu token đã logout vào blacklist.
- Trong các API được authen, khi có request đến, check token có nằm trong blacklist hay không, nếu có thì coi như unauthorize.
- Bài toán đặt ra là lưu blacklist ở đâu để việc đọc token đảm bảo được về mặt hiệu suất khi mà tần số gọi API dày đặc như vậy? Câu trả lời là lưu trong redis. Mặc khác vì jwt token nó có thời hạn sống ngắn, nên hoàn toàn có thể clean up dữ liệu, xoá các token đã thực sự hết hạn trong redis.

## Why use zustand over redux

- Zustand has minimal boilerplate
  - Define a store with just a single function.
  - No need for actions, reducers, or middleware setup.
- has a much smaller bundle size
- has built-in support for middleware like logging or persisting state.

## Debounce và throttle

- <https://www.youtube.com/watch?v=LZb_Bv81vQs>
- <https://www.kaidohussar.dev/posts/debounce-vs-throttle>

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

## Why is pnpm fast

- <https://dev.to/stackblitz/what-is-pnpm-and-is-it-really-so-fast-and-space-efficient-29la>
- <https://www.showwcase.com/show/36421/what-is-pnpm-and-why-you-should-use-it>

- Utilizes a shared dependency mechanism that allows different projects to use the same copy of a package. Packages are stored centrally in a single content-addressable store on your machine and are hard-linked from this storage => you only ever download a package ONCE and require less disk space
- Installation times are faster

## `npm install` and `npm ci`

- `npm install`
  - When you run `npm install`, it compares the list of dependencies in `package.json` and `package-lock.json`. If they are not the same, `npm install` will update the `package-lock.json` to match the dependency specifications in `package.json`. Otherwise, it installs the dependencies as specified in `package-lock.json` 
- `npm ci`
  - Installs exact dependencies and versions listed in `package-lock.json`, without updating lock file. This means `npm ci` always installs a project with a clean slate
  - It removes the `node_modules` folder before installing dependencies, ensuring a clean slate.
  - It's faster, and often used in CI/CD environments to ensure consistent. The reason is when developer changes the `package.json` without running `npm install` and push it to repository. `package.json` is modified but not `package-lock.json`. When the CI/CD runs, the CI/CD server pulls the code and run `npm install` and updates lock file. Now the version of `package-lock.json` on git and on server is different. So, the next time CI/CD runs, it cannot pull the code anymore because there is conflict in `package-lock.json`. When we use `npm ci` for CI/CD pipeline, it install exactly what is listed in `package-lock.json`

## Semantic version (SemVer)

Syntax: `MAJOR.MINOR.PATCH`

Ví dụ:
Version 0.3.10 ra đời trước 0.10.3
Version 0.1.1 1.0.0
Version 1.100.100 10.10.10

Thay đổi:

- MAJOR: often refers to 'breaking change', which means the big update and can be incompatible with older version.
- MINOR: add more functions and compatible with the APIs of current version.
- PATCH: fix bugs

Range version:

- `"library-x" : "*"` => latest
- `"library-x" : "~1.2.3"` => only update patches release: 1.2.3 <= x < 1.3.0
- `"library-x" : "^1.2.3"` => only update patches and minors release: 1.2.3 <= x <2.0.0

## `target` and `currentTarget`

- target = element that triggered event;
- currentTarget = element that listens to event.

## Object.assign() and Object.create()

`Object.assign(target, source)` copy enumerable properties from `source` to `target` object.
`Object.create(source)` create a new object using the `source` as prototype

## `dangerouslySetInnerHTML`

`dangerouslySetInnerHTML` là 1 property của React mà có thể sử dụng cho HTML elements. `dangerouslySetInnerHTML` được dùng để báo cho React biết là content của 1 thẻ HTML là dạng dynamic (lấy từ input) và có thể chứa các thẻ HTML khác như là thẻ `<p>`, `<span>`, `<script>`

## Code splitting

Code splitting is the splitting of the code into various of bundles or components which can be loaded on demand or in parallel

## Display date as string in JS

First we need to create a Date instance using `new Date()`, then we pass in our date string, or Unix timestamp (in mi)

```js
// Create a date object for Halloween
let halloween = new Date('October 31, 2022');

// Create a date object for March 21 at 2pm
let springLuncheon = new Date('March 21, 2023 14:00');

// Using timestamp
let earthDay = new Date(1682136000000);
```

For better accuracy across browsers and operating systems, it's recommended that you use the **ISO 8601 format**: `YYYY-MM-DDTHH:MM:SS`.

Examples: '2011-10-05T14:48:00.000Z'. T means nothing but a space between date and time, Z means zero UTC offset (UTC+0). If Z is ignore, the local system timezone will be used

You can also create a Date object by passing in a series of arguments: year, monthIndex, day, hours, minutes, seconds, and milliseconds. Only year and monthIndex are required.

```js
// Notice the month index is 9 even though October is the 10th month
let halloween = new Date(2021, 9, 31);

// Christmas morning at 10:30 am, local time
let christmas = new Date(2021, 11, 25, 10, 30);
```

Use `toISOString` convert Date object back to ISO Format:

```js
new Date("05 October 2011 14:48 UTC").toISOString()
```

- `Date.UTC`

`Date.UTC()` returns the number of milliseconds since January 1, 1970, 00:00:00 UTC (timestamp)

```js
const date = new Date(Date.UTC(2012, 11, 20, 3, 0, 0));
```

### Get data from Date instance

After creating date instance, we use `Intl.DateTimeFormat` to display Date with the format that is passed in

```js
const date = new Date(Date.UTC(2012, 11, 20, 3, 0, 0));
new Intl.DateTimeFormat('en-US').format(date)
```

If you want to get the day, month, year, you can use `getDay`, `getMonth`, `getYear` from date object

## Các cách check null và undefined

```js
let x;
if ( x == void 0) {}

let firstName = null;
let lastName;

console.log(Object.is(firstName, null)); // true
console.log(Object.is(lastName, null)); // false
```

## What is functional programming

Divive problem into small function. Each function must be pure function.

## Object `getter`, `setter` in JS

JavaScript object getters and setters are used to control access to an object's private/internal properties.

Setters can also be used to validate the values being assigned to a property, ensuring data integrity and consistency.

```javascript
const config = {
  _languages: [],
  get language() {
    return this._languages
  },
  set language(lang) {
    return this._languages.push(lang);
  },
};

// Set the languages
config._language = 'VN';
// Get the languages
console.log( config.language ) // ['VN']
```

`Setter` doesn't hold any value, their purpose is to modify the properties. If your object doesn't have getter function and you call the setter function, it returns undefined

## Number.isNaN() và isNan()

`Number.isNaN(value)` check if the `value` is numeric and not a number.
`isNaN(value)` check if the `value` is not a number

## Animation properties

name, delay, duration, iteration count, play state, easing

- Best practice when write CSS animation
  - The name of the animation should be at the end
  - Animation duration first, then delay, for example: `0.8s infinite ease alternate spinner`
- At minimum, you only need to specify animation name and duration
- Using `alternate` as direction to alternate between foward and reverse direction, create a back-end-forth effect

## Animation vs transtion

- Animation has multiple steps, while transtion only has 3 step which is start and end
- Looping: Animation can loop
- Direction: Animation can go fowared, backward and altenate between two while transtion only go in one direction
- Play state: animation can be paused

- Not all properties can be transitioned, for example: 'background'. In this case, you can apply background to pseudo-element and transition its opacity

## Hiểu thế nào về responsive web design

Đó là việc 1 website có thể hiển thị và tương tác (UI và UX) tốt trên tất cả các trình duyệt và các cỡ màn hình

## `Map` trong javascript

<https://phuoc.ng/collection/this-vs-that/object-vs-map/>

Why: <https://www.builder.io/blog/maps>

- `Map` sửa xóa property nhanh hơn so với object => Sử dụng `Map` nếu cần sửa xóa nhiều property, còn lại nếu chỉ có đọc data thì có thể dùng object
- Object có built-in properties và các properties này có thể bị override
- Gần giống như object, khác ở chỗ key có thể là bất cứ type nào, kể cả object, function
- `Map` giữ nguyên order của key. Object chỉ giữ nguyên order khi key là string hay symbol. Nếu key dạng số thì order sẽ có thể bị thay đổi
- Việc lặp qua các phần tử trong `Map` cũng đơn giản hơn, k cần sử dụng đến `Object.keys`, `Object.values`... để transform trước khi lặp

  ```js
  map1.forEach((value, key) => ...
  // Or
  for (const [key, value] of map1) {
  // Nếu muốn chỉ lặp qua key hoặc values thì có thể dùng `map1.keys()` hoặc `map1.values()`
  ```

Cú pháp:

```javascript
const map = new Map([iterable]); // iterable is optional, such as new Map([[a,1], [b,2]])
// Or
const myMap = new Map(Object.entries({
  key: 'value',
  keyTwo: 'valueTwo',
}))
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
for( const [key, value] of map )
```

- Map destructured

```js
// Map reserves order or keys, not like Object
const [[firstKey, firstValue]] = myMap
```

- Convert to array: [...newMap]

- Copy

```js
const copied = new Map(myMap);
```

### `Map()` and `Weakmap()`

<https://phuoc.ng/collection/this-vs-that/map-vs-weak-map/>

- `Weakmap()` only allows key as `object`
- The key of `Weakmap()` can be garbage collected when it comes to the key as object

`Weakmap` tương tự như `Map` nhưng key của Weakmap có thể được gabage collected nếu không có tham chiếu nào đến nó => tránh hiện tượng memory leak.

## Else if using operation tenerry

`const a = b ? c : d ? e : f`

## OOP

<https://kumartul.hashnode.dev/object-oriented-programming-explained-like-never-before>

## CORS

CORS (Cross-Origin Resource Sharing) is a security feature in web browsers that controls how resources (like APIs or files) can be requested from a different domain than the one where your web page is running.

**Why is CORS needed?**

By default, browsers block requests to a different origin (domain, protocol, or port) for security reasons. This is called the Same-Origin Policy. CORS allows servers to explicitly permit requests from trusted origins by adding specific HTTP headers.

If client app at `https://myapp.com` tries to fetch data from `https://api.external.com`, server at `https://api.external.com` must include CORS headers (`Access-Control-Allow-Origin: https://myapp.com`)

Cùng origin: cùng scheme, domain và port

- Cùng scheme và cùng domain

<http://example.com/app1/index.html>
<http://example.com/app2/index.html>

- Port 80 by default

<http://Example.com:80>
<http://example.com>

Khác origin:

- Khác schemes
<http://example.com/app1>
<https://example.com/app2>

Để server có thể cho phép client gửi cross-origin request, server đó phải set giá trị cho 1 header có tên là `Access-Control-Allow-Origin` và giá trị đó là origin của client.

## Rendering a web page

- <https://abhisaha.com/blog/exploring-browser-rendering-process>
- <https://blogs.halodoc.io/how-does-a-browser-actually-work/>

<ol>
  <li>You type an URL into address bar in your preferred browser.</li>
  <li>The browser **parses the URL** to find the protocol, host, port and path.</li>
  <li>It forms a HTTP request</li>
  <li>To reach the host, it first needs to translate the human readable host into an IP number, and it does this by doing a DNS lookup on the host</li>
  <li>Then a socket needs to be opened from the user's computer to that IP number, on the port specified (most often port 80)</li>
  <li>When a connection is open, the HTTP request is sent to the server</li>
  <li>server host forwards the request to the web server configured to listen on the specified port
  <li>The server inspects the request (most often only the path), and launches the server plugin needed to handle the request (corresponding to the server language you use, PHP, Java, .NET, Python?)</li>
  <li>The plugin gets access to the full request, and starts to prepare a HTTP response.</li>
  <li>To construct the response, a database is (most likely) accessed.</li>
  <li>Data from the database, together with other information the plugin decides to add, is combined into HTML.</li>
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

- <https://daveceddia.com/javascript-references/>
- <https://www.youtube.com/watch?v=RLBqJpK1hro>

Khi khai báo 1 biến, giá trị của biến đấy được bỏ vào trong 1 cái box, và biến trỏ vào box đó.

Primitive type: string, number, boolean, undefined, null, Symbol
Reference type: object, array, Map, Set

Primitive lưu giá trị
Reference lưu địa chỉ chứa giá trị.

=> Khi so sánh, primitive value so sánh bằng giá trị, còn object so sánh thông qua reference. JS sẽ check xem 2 object có reference tới cùng 1 box không

Primitive type là immutable. Ta chỉ có thể gán lại biến đấy sang 1 giá trị khác (1 box khác)

```js
const a = 'abc';
a[2] = 'd'
console.log(a) // Still 'abc'
```

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
const min = minimum(items); // Khi truyền items vào dưới dạng tham số, hàm sẽ copy tham số sang 1 biến khác: const array = items. Array thay đổi thì items cũng thay đổi vì khi này cả array và items cùng trỏ tới 1 box hay là cùng trỏ tới 1 địa chỉ chứa giá trị
console.log(min) // 1
console.log(items) // [1,4,7,9]
```

## API là gì

<https://www.youtube.com/watch?v=JUvsdFWL7WM>

API is the set of mechanism (functions, protocol, commands...) that enables two applications to talk to each other

For example:

- Application B provides a function to sum 2 numbers for Application A
- React provides 'useState' functions
- Backend provides API to let Frontend communicate with database.

## What is RESTApi

<https://www.youtube.com/watch?v=-mN3VyJuCjM&ab_channel=ByteByteGo>

RESTApi is a common API standard that client uses to talk to server. Client interacts with server by making a request to an endpoint over HTTP protocol, which uses HTTP methods. HTTP methods are POST, GET, DELETE, PUT. Then server returns a response with a status code: 2xx is successful, 3xx means redirection, 4xx means client error (something wrong with our request), 5xx means server error.

RestAPI should be stateless. It means client and server doesn't need to store information of each other. Every requests and responses is independent from each other.

## Pass by value and pass by reference

- <https://www.aleksandrhovhannisyan.com/blog/javascript-pass-by-reference/#what-is-pass-by-value>
- <https://www.30secondsofcode.org/js/s/pass-by-reference-or-pass-by-value>

- Passing by value is about copying. When an argument is passed by value, the formal parameter receives a copy of the argument's value (argument is the variable that passed to function when the function is called)
- In pass by reference, param would be an alias for argument, which means param values change will lead to change in argument value. But JS always **pass by value** (both primitive and non-primitive values) and doesn't pass by reference.

Here we are passing a primitive value:

```js
function swap(a, b) {
    const temp = a;
    a = b;
    b = temp;

    console.log(a); // 2
    console.log(b); // 4
}

let x = 4;
let y = 2;
swap(x, y);

// x and y do not swap, whereas a and b do locally.
// If it did, then a would've been an alias for x and b would've been an alias for y. Any changes to a and b would have been reflected back to x and y
console.log(x); // 4
console.log(y); // 2
```

And now, we pass a non-primitive value (an object)

```js
function changeName(person) {
    // person.name = 'Jane';
    // If we modify the param, the original argument changes as well but still points to the same memory address in the heap
    // But if we assign person to the new value which is the new object
    // The original argument doesn't change at all
    person = {name: 'Jane'};
}

let bob = { name: 'Bob' };
changeName(bob);
console.log(bob) // bob is still {name: 'Bob'}
```

## What is asynchronous in JS

Asynchronous means the program can handle multiple task simultaneously rather than excecuting them one by one.

## Regular function and async function. When to use async function

Async function always return a promise. It's often used when we want to perform some asynchronous action inside the function.

## How do you debug error in JS

## Why we use arrow function over normal function

- Less code to write
- We can utilize `this` where the arrow function is defined because `this` in arrow function relates to the outer scope environment.

## Why you should put your modal outside of main area

- <https://www.developerway.com/posts/positioning-and-portals-in-react>

Normally, when you build a modal, you will set its position `fixed` and give it a high value of `z-index`. However, if you render your modal in a nested div, there will be a high chance that your modal will lie below other elements.

The reason is because of `stacking context`.

Actually, fixed elements are relative to its containing block which is viewport by default. We can also create this containing block by adding `transfrom` property. So if the parent element of nested modal is a containing block, the `z-index` value of modal will be counted only inside its parent. And if modal's parent lie below other elements in DOM, the modal will lie below other elements as well.

## `nth-child` and `nth-last-child`

You can simply understand that `nth-last-child` is reverse to `nth-child`. By using `nth-last-child`, we start with the last item instead of first item in `nth-child`.

`nth-last-child` can be used with `:has()` to check if the container has minium amount of children (<https://ishadeed.com/article/conditional-css-has-nth-last-child/>)

## Common web securities

- Preventing XSS
  - Don't save sensitive data in local storage
  - Using cookie with `httpOnly` flag set to true
  - Escape dynamic content when you read it from database
  - Sanitize data before saving into db
- Preventing CSRF
  - Using cookie with `SameSite` flag set to `Lax` or `Strict` to prevent
- SQL injection

## What is responsive design, how to implement it

Is a web design approach to make web pages render well on all screen sizes and resolutions while ensuring good usability

How:

Without CSS, using only HTML can make web layout responsive by default.
With CSS, for simple layout, we can use `flexbox` and `grid` to make responsive. For rather complex layout, we can use media query

## CSS priority

`!important` => inline style => id => class => tag name

## Rules of hooks

- Only call Hooks at the top level. You shouldn't call `useState` in loop, conditions
- Only call Hooks inside React function components
- Whenever we have connected `useState` and `useEffect` hooks, it's a good opportunity to extract them into a custom hook:
- `useState` doens't merge objects when the state is updated. It replaces them.
- Don't use `async` with callback function in `useEffect`. It can work fine if you have only one `useEffect`. However, if you have multiple `useEffect`, you could run into race conditon.

When there is an update action (ex: click event).

- When you call the `setState`, it might trigger an re-render. However, `setState` doesn't update the state immediately. React waits for all the `setState` in the event handlers to be finished before re-rendering. In another way, it groups multiple state updates into a single update. This is called `batching` (<https://github.com/reactwg/react-18/discussions/21>)
- You can pass a value or an updated function to the `setState`. All of them are added to the queue.
- When all the scheduled re-renders run
  - `useState` is called
  - React goes through all the queue, calculates and returns the updated value to `useState`

If you update the state hook to the same value as the current state, there will be no re-render to be scheduled.

## Scope chain

Có 3 loại scope: `global scope`, `local scope` (function) và `block scope` (scope nằm giữa {}). Khi một biến không được tìm thấy trong `local scope`, JS sẽ tìm biến đó ở các outer scope cho đến scope ngoài cùng là `global scope`.

## for-in and for-of

`for-in`:

- iterate over enumerable properties of an object. enumerable properties are the "key" of the Object or "index" of the Array.
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

## Difference between `:focus` and `:focus-visible`

- `:focus`: triggered when element receives focus, regardless of how it was focused (mouse, keyboard, or programmatically).
- `:focus-visible`: 
  - triggered when element receives focus, and the browser determines that focus should be visibly indicated (typically via keyboard or assistive technology).
  - When you click on an button, browser doesn't think this element needs focus indication => won't display focus style
  - When you click on an input, `:focus-visible` is still triggered because it helps users see where they are typing or interacting

## Difference between `async, await` and `promise`
They are both used to handle asynchronous tasks. The difference is `promise` involves chaining `.then` and `.catch` methods, whereas `Async` `Await` uses a `try-catch` block that looks more like synchronous code. So, it's easier to read code with `async` and `await`

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

## `onChange` và `onInput` khi sử dụng cho input

- `onChange` 
  - Triggered: When the value of an input element changes and then blurs (for <input type="text">) or immediately (for <input type="checkbox"> or <select>). (**NOTE**: `onChange` in React behaves like `onInput`)
  - Use Case: When you care about the final value, not intermediate keystrokes for input text
- `onInput`
  - Triggered: Immediately as the user types or changes the input's value or on paste
  - Use Case: When you need real-time feedback (e.g., live search, character counters)

## `import` and `export`

### `export`

chỉ có 1 default export duy nhất. Khi import ta chỉ cần viết `import ChildComponent from './abc.js'`

`export * from` hay còn gọi là re-export: `export {default} from './abc.js'`. Kĩ thuật này được sử dụng để gom các export từ các child component và export lại 1 lần nữa.  `*` ở đây là 1 object, chứa tất cả các export từ `./abc.js`, kể cả export thường lẫn export default. `{default}` là object destructure. `export * from` tương đương với `import * from` kết hợp với `export *`. Khi có nhiều child component cần re-export thì mình có thể viết như sau

```js
export {default as a} from './a.js' // tương đương với import {default as a} from './a.js' và export a
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

// {
//   default: "I love JavaScript",
//   name: "Lydia",
//   age: 21
// }
```

## What happens when you type a URL into browser

<https://www.youtube.com/watch?v=AlkDbnbv7dk>

## Core Web Vital

- <https://www.debugbear.com/docs/metrics/core-web-vitals> **MUST READ**
- <https://prateeksurana.me/blog/future-of-rendering-in-react/>
- <https://i.imgur.com/jqIBleM.png>
- <https://i.imgur.com/DCsCct2.png>
- <https://www.youtube.com/watch?v=ZKH3DLT4BKw&ab_channel=AddyOsmani>
- <https://indepth.dev/posts/1498/101-javascript-critical-rendering-path>
- <https://web.dev/learn-web-vitals/>

- DOMContentLoaded: the time for loading HTML before starting to load the content. The event does not wait for images, subframes or even stylesheets to be completely loaded. The only target is for the Document to be loaded
- Load: when all the resources are loaded ( resources are parsed and get acknowledged off before DOMContentLoaded)
- Speed Index: shows how quickly the contents of a page are visibly populated.
- Time to first byte (TTFB): the time it takes the browsers to start receiving the first byte of a request. It includes four components: DNS lookup, TCP connection, SSL connection, HTTP request time. How to improve: 
  - Caching
  - Avoids redirect
  - Use CDN
  - Reduce queries
- First paint (FP): Time for the first pixel to render on the screen after user navigates to web page
- First Contentful Paint (FCP): the moment when the first element from the DOM appears in the browser. Element here includes text, images, non-white canvas, or scalable vector graphics (SVG). How to improve: 
  - Eliminate render-blocking resources
  - Minify CSS and JS
  - Remove unused CSS, unused JavaScript
- Largest Contentful Paint (LCP): the time when the largest content element (text or images, video) in the viewport becomes visible. It can be used to determine when the main content of the page has finished rendering on the screen. How to improve:
  - Implement effective image compression (consider using WebP or AVIF formats)
  - Remove non-critical third-party scripts (like unused analytics or ad scripts)
  - Upgrade server resources or implement a CDN
- Time To (fully) Interactive (TTI): the amount of time it takes for the page to be fully interactive. Users might expect this is the same time as LCP, but this can be different. If TTI and LCP differ, users might assume the website is slow.
- First Input Delay (FID): the time from when a user first interacts with a page to the time when the browser is actually able to respond to that interaction.
  - Minimize JavaScript execution time (use async/defer attributes)
  - Implement code splitting to reduce the initial bundle size
  - Use a web worker for heavy computations
- CLS (Cumulative Layout Shift) – it's a metric that measures the visual stability of a page layout. It's calculated as the sum of shifts that are visible during page loading. This includes unexpected shifts, i.e., those unrelated to user interaction.
  - Set explicit width and height for media elements
  - Avoid dynamically injecting content above existing elements
  - Use CSS transform for animations instead of properties that trigger reflow
- Total Blocking Time (TTB): total amount of time between First Contentful Paint (FCP) and Time to Interactive (TTI) where the main thread was blocked for long enough to prevent input responsiveness.

## Obtimize speed

- <https://www.builder.io/m/explainers/seo-core-web-vitals>
- <https://focusreactive.com/deep-dive-into-web-performance-mastering-lcp-optimization-for-seo-success/>
- <https://towardsdev.com/website-performance-optimization-267b28b877df>

- Font: reduce webfont
- Image: 
  - Compress and resize the images
  - Use responsive images using `srcset` and `sizes`
  - Use modern image format: webp, avif
  - lazy load for images below the fold
- Code optimization:
  - Minify and compress CSS/JS
  - Implement tree shaking to eliminate dead code
  - Use code splitting for large applications (e.g., React.lazy() and Suspense)
- Caching:
  - Browser caching
  - server-side caching (e.g., Redis or Memcached)
- Server:
  - Fast web hosting
  - CDN for static files (images, CSS, JS files)
  - Optimize database queries (use indexing, avoid N+1 queries)
  - Gzip compression: compresses Web pages, CSS, and JavaScript at the server level before sending them over to the browser. On the user side, a browser unzips the files and presents the contents.
- Critical rendering path optimization
  - Defer non-critical JavaScript using async or defer attributes
  - Preload key resources with `<link rel="preload">` (ex: `preload` for hero image,...)

## Critical Rending Path

- <https://web.dev/learn/performance/understanding-the-critical-path>
- <https://www.toptal.com/web/website-performance-critical-rendering-path>

The critical rendering path refers to the steps from when the browser sends the request until the web page starts rendering in the browser.

The critical rendering path involves the following steps:

- Once the browser gets the HTML, it starts parsing it. When it encounters a resource (CSS or JS), it tries to download it.
  - If it's a stylesheet file, the browser will have to parse it completely before rendering the page (and that's why CSS is said to be render blocking). Then the CSS object model (CSSOM) is built.
  - If it's a script (with no `async` and `defer`), the browser has to: stop parsing, download the script, and run it. Only after that can it continue parsing, because JavaScript programs can alter the contents of a web page (HTML, in particular). And that's why JS is called parser blocking.
- Once all the parsing is done, the browser has the Document Object Model (DOM) and Cascading Style Sheets Object Model (CSSOM). It combines the two to create the Render Tree. 
- Next, layout (or reflow) happens to calculate the size and position of every nodes on the page. 
- Once the layout is determined, pixels are painted to the screen. (Repaint)

This process's length depends on the the critical resoucres:

- Part of the HTML.
- Render-blocking CSS in the <head> element.
- Render-blocking JavaScript in the <head> element.

Optimizing the critical render path involves reducing the time to receive the HTML (represented by the Time to First Byte (TTFB) metric), and reducing the impact of render-blocking resources. Here are some tips:

- Minimize the number of critical resources by deferring their download (defer attribute), marking them as async (async attribute), or just remove them.
- Optimize the file size of each request.
- Caching the resources
- Optimize the order in which critical resources are loaded by prioritizing the downloading critical assets, shorten the critical path length.

## `var` and `let`, `const`

- Blocked scope

`var` is global scoped and only blocked scope only when it is declared inside a function
`let` and `const` blocked scope every where

- Creation phase

This is the very first phase when JS setting up memory for the data. No code is executed at this point. Both variable declarations are hoisted, and their memory space have all been set up. However:

- Variable declared with `let` and `const` have no default value (`uninitialized`)
- Variable declared with `var` are stored with the default value of `undefined`

Then if we try to access the variables before its declaration:

- When variable declared with `var`, it returns `undefined`
- When variable declared with `let` and `const`, it returns `ReferenceError`

## Hoisting

It's the process of moving the declaration of functions, variables, or classes to the top their scope, prior to execution of the code
It stores functions and variables in the memory before executing any code (creation phase);

- Functions are stored with the entire function. That's why you can invoke them before the line we declare them. However, if you declare function by assigning it to a variable, the hoisting rule for variable is applied.
- Variables are different.
  - Variables with the `var` keyword are stored with the value of `undefined`
  - Variables with the `let` and `const` keyword are stored `uninitialized`. If we try to access them before declare them, it will return `ReferenceError`

## Object.freeze() và Object.seal()

`Object.freeze()` không cho phép thêm, sửa, xóa properties. Tuy nhiên, `Object.freeze()` chỉ shallowly freeze, nếu properties là 1 object khác thì vẫn có thể sửa được
`Object.seal()` không cho phép thêm, xóa, nhưng vẫn cho sửa existing properties

## `useMemo` và `useCallback`. Có thể chuyển từ `useMemo` sang `useCallback` được không

- <https://kentcdodds.com/blog/usememo-and-usecallback>

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

## Expressions and Statement

- Expression is the code snippet that resolves to a value, like: operator (+, -, ...)
- Statement is the code snippet that performs an action, like declare and assign a value to variable (`let a = 1`) or check condition (`if (a===1)`)

The best way to distinguish them is using `console.log`, anything that can be passed to `console.log()` is an expression.

## `void`

`void` is an **operator** that evaluates an expression and returns `undefined`. `void` is an operator, not a function, so that we don't need to wrap expression in parentheses

```js
void 0 === undefined
void function() {} === undefined
```

## Design system

A design system is a collection of reusable components with clear standards (code styleguides, fonts, colors, spacing...). These components combine together to build any applications.

A UI framework library like Bootstrap, MUI, Ant-design... are design system. The downside of these libraries is that every sites using the same library can look the same. Of course, we can customize the library but it can take times.

Atomic design

- Atom: like button, input, label
- Molecules: collections of atoms, like Search Bar
- Organisms: collections of Molecules, like Header, Footer
- Templates: collections of Organism, like layouts. A site can have multiple layouts
- Pages: a specific pages that can use one of templates

## `prefers-color-scheme`

Đây là 1 tính năng của CSS `@media` để check xem thiết bị của bạn (máy tính hoặc điện thoại) đang sử dụng dark theme hay light theme

```CSS
/* Nếu hệ thống đang sử dụng dark theme thì đoạn code CSS trong này sẽ được chạy và ngược lại với light theme */
@media (prefers-color-scheme: dark) {

}
```

```JS
const prefersDark = window.matchMedia && window.matchMedia("(prefers-color-scheme: dark)").matches; // true if system is using dark theme
```

## `color-scheme`

<https://zellwk.com/blog/understanding-color-scheme/>

`color-scheme` tells the browser to render **user-agent stylesheets** (the built-in stylesheets of each browser) according to the user’s preferred color scheme (which is set in their operating system).

## `.lock` file

- <https://duthanhduoc.com/blog/tai-sao-package-lock-json-ton-tai-va-cach-no-hoat-dong>
- <https://11sigma.com/blog/2021/09/03/yarn-lock-how-it-works-and-what-you-risk-without-maintaining-yarn-dependencies-deep-dive/>

When you initialize a project and push to git, another team member clone and install packages. If you specified the `~` syntax and a patch release of a package has been released, that one is going to be installed.

So your original project and the newly initialized project are actually different. Even if a patch or minor release should not introduce breaking changes, we all know bugs can (and so, they will) slide in.

The `package-lock.json` sets your currently installed version of each package (all also the depandancies of that package) in stone. It ensures that every member in the team have the same version for each package.

When present in a project, `.lock` file is the source of information about the current version of dependencies in the project. `yarn` or `npm` will compare dependencies version in the `.lock` file with the current version in the `package.json` and updates packages if needed.

## `srcset` and `sizes`

- <https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images>
- <https://blog.webdevsimplified.com/2023-05/responsive-images/>
- <https://12daysofweb.dev/2021/image-display-elements/>
- <https://imagekit.io/responsive-images/>

```html
<img src="https://ik.imgkit.net/ikmedia/women-dress-1.jpg"
     srcset="https://ik.imgkit.net/ikmedia/women-dress-1.jpg?tr=w-225 225w,
             https://ik.imgkit.net/ikmedia/women-dress-1.jpg?tr=w-300 300w,
             https://ik.imgkit.net/ikmedia/women-dress-1.jpg?tr=w-350 350w,
             https://ik.imgkit.net/ikmedia/women-dress-1.jpg?tr=w-640 640w"
     sizes="(max-width: 400px) 100vw, (max-width: 700px) 50vw, (max-width: 900px) 33vw, 225px">
```

First, we need to calculate the image size from 'sizes' attribute. For example, if the viewport < 400px, let's say 350px, the image size will be 100vw, which is 350px. Then, the browser will use this image <https://ik.imgkit.net/ikmedia/women-dress-1.jpg?tr=w-350>

If the viewport is 650px, the image size will be 325px. It tells browser to use the picture with minimum of 325px => the browser use <https://ik.imgkit.net/ikmedia/women-dress-1.jpg?tr=w-350>, which is the closest one

## Difference between `{}`, `Object`, `Record<string, unknown>` in Typescript

- `{}` includes most of the type, including primative types and non-primitive types, but not `null` and `undefined`
- `object` represents all non-primative values (array, function, object) and does not include primitive types (number, string, boolean), `null`, `undefined`,
- `Record<string, unknown>`: only accepts plain object with pairs of key, value (not accept array, function)

## Difference between using `srcset` and `sizes` and using `<picture>` and `<source>` tag

In HTML, both the srcset attribute and the <picture> element are used for responsive images, but they serve slightly different purposes and are used in different ways. 

- `srcset` and `size`: you have only one image with different sizes and you give hints to the browser to choose the most appropriate size based on the current viewport size and device pixel density.
- `<picture>` and `<source>`: you have multiple image sources for different conditions (like different layout, different screen sizes...). `source` element is more like a command than a suggestion.

```
<picture>
  <source srcset="large.png" media="(min-width: 75em)">
  <source srcset="medium.png" media="(min-width: 40em)">
  <img src="small.png" alt="A description of the image." 
    width="300" height="200" loading="lazy" decoding="async">
</picture>
```

## min, max, clamp()

- <https://moderncss.dev/practical-uses-of-css-math-functions-calc-clamp-min-max/>
- <https://ishadeed.com/article/css-min-max-clamp/>

- `min(value1, value2)`: thiết lập giá trị max cho selector (giống `max-width`). Nó sẽ chọn ra giá trị nhỏ nhất giữa value 1 và value 2 tùy theo viewport width. Thứ tự value không quan trọng. VD:

```css
.container {
  /*Ở màn hình nhỏ hơn thì 100% - 2rem < 80ch nên width = 100% - 2rem nhưng ở màn hình lớn hơn khi mà 100% - 2rem > 80ch thì width = 80ch;*/
  width: min(80ch, 100% - 2rem)
}
```

CSS trên sẽ tương đương với

```css
.container {
  width: 100% - 2rem;
  max-width: 80ch
}
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

- <https://www.valentinog.com/blog/url/>
- <https://www.builder.io/blog/new-url>

```js
const url = new URL('https://builder.io/api/v2/content')

url.searchParams.set('model', model)
url.searchParams.set('locale', locale)
url.searchParams.set('text', text)

console.log(url.toString())
console.log(url.searchParams.get('foo'))
```

URL properties to know

<https://i.imgur.com/KfQOAZq.png>

```js
const url = new URL('https://builder.io/blog?page=1');

url.protocol // https:
url.host     // builder.io
url.pathname // /blog
url.search   // ?page=1
url.href     // https://builder.io/blog?page=1
url.origin   // https://builder.io
url.searchParams.get('page') // 1
```

## `new URL()` and `new URLSearchParams()`

If you only need to work with the params part, you could use `URLSearchParams`

```js
const params = new URLSearchParams({
  page: 1,
  text: 'foobar',
})
params.set('page', 2)
params.toString()
```

## Create text on image with an overlay

- Set `z-index: 1; position: relative` to the container and `z-index: -1, position: absolute` to the overlay. When we do that, the container creates a new stacking context. Overlay element will lie only below other child elements in its stacking context.
- Using `background-blend-mode`: <https://www.smashingmagazine.com/2021/09/reducing-need-pseudo-elements/>

## `vmin`, `vmax`

`vmin` and vmax can change whilst the browser window is resized or the orientation of the mobile phone is changed.
`vmin` is the minimum between the viewport's height or width in percentage depending on which is smaller: `min(vh, vw)`
`vmax` is the maximum between the viewport's height or width in percentage depending on which is bigger: `max(vh, vw)`

## <!DOCTYPE html>

This is the doctype tag to let the browser know that this is HTML5 page and should be rendered as text/html

## `<meta charset="utf-8">`

This meta tag tells the browser which character encoding browser to use. UTF-8 is great because you can use all sorts of symbols and emoji

## `<meta name="viewport" content="width=device-width"`

`width=device-width` tells the browser to use 100% of the device's width as the viewport => there's no horizontal scrollbar. You can commbine this attribute with `initial-scale` to 1 to let the user zoom arond.

## og meta tags

These og meta tags are used to display information of the post like the title, featured image... when you share them on social networks.

## `text-size-adjust`

If you don't have a responsive mobile website, and you open it on a small screen, so the browser might resize the text to make it bigger so it's easier to read. The CSS `text-size-adjust` property can either disable this feature with the value none or specify a percentage up to which the browser is allowed to make the text bigger.

## Resource hint: preconnect, dns-prefetch, preload

- <https://imkev.dev/fetchpriority-opportunity>
- <https://3perf.com/blog/link-rels/> (**Must read**)
- <https://viblo.asia/p/thuat-ngu-trong-frontend-optimization-2oKLn2YgLQO>
- <https://webpack.js.org/guides/code-splitting/>
- <https://www.youtube.com/watch?v=6q75MVFLlok>
- <https://blog.dareboost.com/en/2020/05/preload-prefetch-preconnect-resource-hints/>

**Why use resource hint**

To start loading, the browser needs to scan (preload scanner) the page to understand what resources it needs to render in order to start loading them. And if you don't give the browser any hint, it will only find those resources when it encounters an img element or text node with a required web font.

TLDR:

- `dns-prefetch` & `preconnect` are for high priority resources from a domain but you don't know what exactly what the resources are, you just know that you are going to download them from that domain. (like google font)
- `preload` is for resources with high priority but loaded inside stylesheets or scripts like above-the-fold CSS background images or custom fonts. For image in the markup, preload has little effect
- `prefetch` is for low priority files very likely needed soon, like HTML, CSS or images used on subsequent pages.
- `dns-prefetch` & `preconnect` reference just the domain name, like '<https://example.com>', whereas `preload` and `prefetch` reference a specific file, like header-logo.svg. `prerender` references an entire page, like blog.html.
- `dns-prefetch` & `preconnect` should also be used sparingly as some web browsers may limit the number of preemptive connections.
- Both `prefetch` and `prerender` should be used with care to avoid downloading files that aren't used, which can be costly on mobile networks. Avoid using `prefetch` and `prerender` unless files are certain to be used later or extra data download isn't an issue.
- Resource hints for font files (even when self-hosted) and CORS enabled resources will also need the crossorigin attribute: `<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>`

Mặc định browser parse HTML tải các resoureces theo thứ tự được khai báo trong HTML. Tuy nhiên, với 1 số resource quan trọng cần độ ưu tiên cao hơn, ta có thể sử dụng resource hint cho điều chỉnh lại thứ tự này và yêu cầu browser load chúng nhanh nhất có thể

Trước khi trình duyệt lấy được dữ liệu từ server, nó sẽ phải trải qua 3 bước cơ bản như sau:

- Kết nối tới DNS Server để phân giải tên miền của máy chủ
- Thiết lập kết nối TCP
- Mã hóa kết nối bằng giao thức TLS

Trong từng bước trên trình duyệt gửi một phần dữ liệu tới máy chủ, và máy chủ gửi lại một phản hồi. Hành trình này, từ máy chủ gốc đến địa chỉ cần đến và quay ngược trở lại, được gọi là khứ hồi (round trip). Mỗi bước sẽ có 1 hoặc nhiều round trip. Mỗi round trip khi được thực hiện lại có 1 độ trễ nhất định (latency). Qua đây chúng ta thấy rằng việc chờ đợi để thiết lập kết nối trước khi download data mình cần về khá tiêu tốn khá nhiều thời gian và chính điều này sẽ làm giảm tốc độ tải trang web

`preconnect` sẽ nói với browser rằng tôi đang thực sự cần tài nguyên này, hãy thực hiện việc kết nối sớm nhất có thể. Và trình duyệt sẽ thực hiện 3 bước trên song song với các kết nối khác trước. Nếu không có `preconnect` thì tài nguyên này có thể phải đợi các kết nối khác xong trước và thực hiện lại từ đầu 3 bước trên. Sử dụng `preconnect` để kết nối tới các domain của các resource mà các resource này chưa biết cụ thể (giống google font: domain của google font là 'fonts.gstatic.com')

`dns-prefetch` tương tự như `preconnect` nhưng chỉ thực hiện trước bước kết nối tới DNS Server. `dns-prefetch` có thể dùng chung với `preconnect` như là 1 dạng fallback khi 1 số trình duyệt không hỗ trợ `preconnect`.

`prefetch` báo cho browser biết để download trước 1 tài nguyên nào đó với low priority mà tài nguyên này có thể chưa cần sử dụng ngay. Lấy ví dụ ở 1 trang bán hàng, khả năng cao là khách sẽ đặt hàng và thanh toán trang thanh toán. Vậy nên ở trang chủ, mình có thể prefetch 1 vài script cho trang thanh toán, ví dụ checkout.js và cache script này vào trình duyệt. Khi sang trang thanh toán, script này sẽ được sử dụng ngay mà không cần thiết lập kết nối và down về

<https://indepth.dev/posts/1498/101-javascript-critical-rendering-path>

`preload` yêu cầu browser download tài nguyên và cache càng sớm càng tốt. Dùng cho các resource cần load ngay sau khi render page (e.g font vì browser cần display text ngay khi sau khi render tree hoặc hero image).

The HTML attribute crossorigin defines how to handle crossorigin requests. Setting the crossorigin attribute (equivalent to crossorigin="anonymous") will switch the request to a CORS request using the same-origin policy. **It is required on the rel="preload" as font requests require same-origin policy.**

<https://web.dev/codelab-preload-web-fonts/#preloading-web-fonts>

Không nên lạm dụng resource hint vì chúng vẫn làm tiêu tốn tài nguyên CPU của cả client và server

## Race conditions

- <https://wanago.io/2022/04/11/abort-controller-race-conditions-react>

A race condition is when your application have a sequence of events and their order is uncontrollable. This might occur with asynchronous

## Node and element

- <https://thisthat.dev/element-vs-node/>
- <https://stackoverflow.com/questions/9979172/difference-between-node-object-and-element-object>
- <https://dmitripavlutin.com/dom-node-element/>

- Node represents a single node in the DOM. DOM document consists of a hierarchy of nodes. Each node can have a parent and/or children.
- There are many types of nodes (ELEMENT_NODE, TEXT_NODE, etc...). Node.ELEMENT_NODE represents an element node so DOM Element is one specific type of node (ELEMENT_NODE).
- `Nodelist` is array-like list of nodes

```html
<p>
  <b>Thank you</b> for visiting my web page!
</p>
```

```js
const paragraph = document.querySelector('p');
paragraph.childNodes; // NodeList:       [HTMLElement, Text]
paragraph.children;   // HTMLCollection: [HTMLElement]
```

`paragraph.children` returns elements only. Text node wasn't included here because its type is text (Node.TEXT_NODE), and not an element (Node.ELEMENT_NODE).

## `clientHeight`, `scrollY`, `scrollHeight` and more

- <https://htmldom.dev/determine-the-height-and-width-of-an-element/>

Element:

- `clientHeight`: height of visible content + padding. `document.documentElement.clientHeight` returns the viewport height
- `offsetHeight`: height + padding + border of element
- `getBoundingClientRect()`: return the position of the element that is relative to the **current** viewport. The viewport is the area that is being viewed. So the returned value of `getBoundingClientRect()` may vary depending on the place we are viewing, normally when you scroll down the `top` and `bottom` will be decreased

Scroll:

- `scrollY`: `window.scrollY` returns how long the scrollbar has scrolled
- `scrollTop`: returns how long the element has scrolled
- `scrollHeight`: returns height + padding + content of element (content is visible or not) 
  - `document.documentElement.scrollHeight` returns the height of the whole document.
  - `scrollHeight` = `clientHeight` if no scroll
  - Determine if an element has been totally scrolled: `Math.abs(element.scrollHeight - element.clientHeight - element.scrollTop) <= 1` => `element.scrollHeight ~ element.clientHeight + element.scrollTop`

Mouse event:

These properties belongs to mouse events, like: `mousedown`, `mouseup`

- `clientX`, `clientY`: returns the coordinate of mouse pointer relative to the viewport

## Get CSS styles (`padding`, `margin`) of an element

```js
const styles = window.getComputedStyle(ele);
console.log(styles.paddingTop)
console.log(styles.marginTop)
console.log(styles.backgroundColor)
```

## Get and set element attribute

```js
image.setAttribute('width', '100px');
image.getAttribute('width')
```

## Move element

Use `insertBefore`, `insertAdjacentElement`, `insertAdjacentHTML`

## Handle scroll

There are two API to handle scrolling in JS: `scrollTo` and `scrollIntoView`

`scrollTo` calls on window, `scrollIntoView` calls on element itself

```jsx
function App() {
  const paragraphRef = useRef(null);
  return (
    <div>
      <button
        onClick={() =>
          window.scrollTo({
            top: paragraphRef.current.getBoundingClientRect().top
            behavior: "smooth",
          })
        }
      ><button
        onClick={() =>
          paragraphRef.current.scrollIntoView({
            behavior: "smooth",
            block: "start"
          })
        }
      >
        Scroll to element
      </button>
      <p ref={paragraphRef}>
        Lorem ipsum…
      </p>
    </div>
  );
}
```
