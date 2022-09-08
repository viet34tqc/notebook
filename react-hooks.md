# React Hooks

## Notes

<https://blog.bitsrc.io/common-react-hooks-mistakes-every-developer-should-avoid-defd47d09d8c>

Hooks should not be called within loops, conditions, or nested function
Hooks should be used before any early `return` at the top level of the functional component.

## useState

Don't use `useState` when you don't use any `state` in the render phase. If you want to use a variable that preserves its state across renderings without triggering a re-render, use `useRef` instead.

Ask three questions about each piece of data: Is it passed in from a parent via props? If so, it probably isn’t state. Does it remain unchanged over time? If so, it probably isn’t state. Can you compute it based on any other state or props in your component? If so, it isn’t state.

The value of the state will only be updated in the next render. So, this block of code:

```js
const [count, setCount] = useState(0);
setCount(count + 1)
setCount(count + 1)
setCount(count + 1)
```
is the same as
```js
const [count, setCount] = useState(0);
setCount(0 + 1)
setCount(0 + 1)
setCount(0 + 1)
```

**Tips**

- You can pass a function inside `useState`. This function takes current state as the only param and the return the new value based on the current state
- Whenever a state setter function is only used synchronously in an effect, get rid of the state! <https://tkdodo.eu/blog/dont-over-use-state>

## useReducer

Syntax
```jsx
const [state, dispatch] = useReducer(reducerFunction, initialState);
```
Comparing to `setState`, you can think of `dispatch` as `setState` and the `reducerFunciton` as the callback of that `setState`

## useRef and fowardRef

### What are `ref` in React

**`ref` in React is a variable and it stores the reference to the DOM, timerID, or previous state**

The popular use of ref is to access the DOM element.
Let'say we want to focus on an input element when the user clicks a button. In plain JS, we can query the DOM to find the element first then call the `focus` function

```js
const inputElement = document.getElementById('input-element-id');
inputElement.focus();
```

Instead of querying the DOM directly, React use `useRef`, so we can store the reference to DOM in an variable like this

```jsx
const App = () => {
  const inputRef = useRef(null);

  return (
    <>
      <input
        ref={inputRef}
        value={name}
        onChange={handleChange}
      />
      <button
        onClick={() => inputRef.current.focus()}
      >
        Focus
      </button>
    </>
  );
};
```

The `ref` object will look like the following after React assigned the element.

And now, if we want to focus on an element, just need to call `inputRef.current.focus()}`

**Notes**: React component only re-renders when the state or props changes. When it re-renders, it update the DOM with the new state. React won't update any DOM ultil that component re-renders. Changing `ref` doesn't cause the component to re-render, so the DOM might not be updated. For example:
```jsx
<button disabled={count.current === 3}>Button</button>
```

**When to use `ref`**

In React, every update is split in two phases:

- During render, React calls your components to figure out what should be on the screen.
- During commit, React applies changes to the DOM.

In general, you don’t want to access refs during rendering. That goes for refs holding DOM nodes as well. During the first render, the DOM nodes have not yet been created, so ref.current will be null. And during the rendering of updates, the DOM nodes haven’t been updated yet. So it’s too early to read them.

React sets ref.current during the commit. Before updating the DOM, React sets the affected ref.current values to null. After updating the DOM, React immediately sets them to the corresponding DOM nodes.

Usually, you will access refs from event handlers. If you want to do something with a ref, but there is no particular event to do it in, you might need an effect.

### fowardRef

And now, we want this `HTML input` element to be re-used in other components. So, we create an `Input` component which contains `HTML input` element.

```jsx
const Input = (props) => {
  return <input {...props} />;
};
```

And we still want to focus on the input when the user clicks the button. We might attempt to create a reference like before and pass it down to the `Input` component, then call the `focus` function

Unfortunately, it won't work like that. **By default, `ref` only works on HTML element, not on React components. We need to tell React which HTML element it should refer in that component**, as there can be more than one element in the component.

That's where `fowardRef` becomes useful

`fowardRef` is a function and it accepts a callback function with 2 parameters
- The component props
- The ref to be fowarded

```jsx
const Input = forwardRef((props, ref) => {
  // We pass the ref to the element we want
  return (
    <input
      ref={ref}
      {...props}
    />
  );
});
```
If you are using DevTool to debug React, the name of the component using `fowardRef` can be `anonymous`. That's because the callback function we pass to is anonymous function. So we could named it like this:
```jsx
const Input = forwardRef(function Input(props, ref) => {
```

Use `fowardRef` with Typescript
```tsx
const Input = forwardRef<HTMLInputElement, IInputProps>((props, ref) => {
  return <input ref={ref} {...props} />;
});
```

`useImperativeHandle`
<https://www.youtube.com/watch?v=dSzf0nv6QmM>

Considering a case when you want to pass a ref from Parent component to child component. Default, that ref will be public and you can use that ref with any event handler in the parent component. `useImperativeHandle` will limit value, state, or function inside a child component to the parent component the ref

```jsx
const Video = forwardRef((props, ref) => {
  const videoRef = useRef();
  useImperativeHandle(ref, () => ({
    play() {
      videoRef.current.play();
    }
  }));
  return <video ref={videoRef} src="https://www.w3schools.com/tags/movie.ogg" />;
});

export default function App() {
  const videoRef = useRef(null);

  return (
    <>
      <Video ref={videoRef} />
      <button onClick={() => videoRef.current.play()}>Play</button>
      <button onClick={() => videoRef.current.pause()}>Pause</button>
    </>
  );
}
```
In the example above, the child component (Video) only exposes `play` method to the parent component, not `pause` method.

## `useEffect`

<https://tkdodo.eu/blog/simplifying-use-effect>

- One effect do one thing. Use multiple effects if you need.
- Move that `useEffect` to custom hooks if you can.

Explain this code:
```jsx
const [count, setCount] = useState(180);

useEffect(() => {
  setInterval(() => setCount(count -1), 1000)
}, []);

return <div>{count}</div>
```
In this code, we want to create a timer clock start from 180. However, it will dislay `179` then stop rather than 179, 178,...
To understand why does this happen, you need to know about the closure. So, what is the value of `count` in `setInterval`. Because the callback of `useEffect` only runs only the once after render, then `count` is referenced to the outside scope, which is 180. It equaivalents to:
```jsx
useEffect(() => {
  setInterval(() => setCount(180 -1), 1000)
}, []);
 ```

How to fix this:
```jsx
useEffect(() => {
  // Now the value in setCount is not referenced to the outside anymore
  // prevCount is take from the previous `setCount` again and again.
  setInterval((prevCount) => setCount(prevCOunt -1), 1000) 
}, []);

// Or
useEffect(() => {
  setTimeout(() => setCount(count -1), 1000) // This will create multiple setTimeout base on count value
}, [count]);
```

- Cleanup function with event

<https://www.youtube.com/watch?v=Fnb3GbY9FUY&list=PL_-VfJajZj0UXjlKfBwFX73usByw3Ph9Q&index=37>
```jsx
const [count, setCount] = useState(0);

useEffect(() => {

  return () => {
    // do something with the previous state
  }
}, [])

const handleClick = () ={
  // This will setState
}
```

## `useLayoutEffect`

<https://www.youtube.com/watch?v=loSqbCbH2xo&list=PL_-VfJajZj0UXjlKfBwFX73usByw3Ph9Q&index=39>

This hook is very similar to `useEffect`. The difference is only the task order. Most of the time, we will use `useEffect`. Only use `useLayoutEffect` when we want to update the state before rendering UI to avoid flick in the UI. See the example above for more details.

`useEffect`:

1. Update state
2. Update DOM (mutate the virtual DOM object) 
3. Re-render UI
4. Call cleanup function if deps change
5. Call `useEffect` callback

`useLayoutEffect`:

1. Update state
2. Update DOM
3. Call cleanup function (synchronously)
4. Call `useEffect` callback (synchronously). Synchronously means the step must be finish before jumping to the next step. So, the callback will be called first before rendering UI.
3. Re-render UI