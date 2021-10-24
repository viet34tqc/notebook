# React Hooks

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
