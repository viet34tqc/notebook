# React Tips

## Never update state during render phase

<https://alexsidorenko.com/blog/onclick-too-many-re-renders/>
It could lead to infinite loop. Update the state like this is the same as calling the callback function inside click handler
```jsx
<button onClick={setState(count + 1)}>Click</button> /*DONT do that*/
```

## Avoid using props as initial state of useState

<https://sentry.io/answers/using-props-to-initialize-state/>
<https://prateeksurana.me/blog/why-you-should-avoid-using-state-for-computed-properties/>

When we use props as initial state, that state won't react to props update. The reason is because `useState` called only once when the component mounts. 

## Working with form inputs

<https://www.joshwcomeau.com/react/data-binding/>

## Set empty string as initial value for input text and textarea

When the input is controlled, we should pass initial value as empty string rather than `undefined`. If value attribute is `undefined`, React will omit it, then the input becomes uncontrolled

## Use callback refs to interact with DOM nodes to avoid using `useEffect`

<https://tkdodo.eu/blog/avoiding-use-effect-with-callback-refs>

Passing a ref from `useRef` (a RefObject) to a React element is just syntactic sugar for:
```js
<input
  ref={(node) => {
    ref.current = node;
  }}
  defaultValue="Hello world"
/>
```

```jsx
function MeasureExample() {
  const [height, setHeight] = React.useState(0)

  // we use `useCallback` to avoid creating ref every render
  const measuredRef = React.useCallback(node => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height)
    }
  }, [])

  // Other solution with useRef as long as there is no dependancy in the function inside `useRef`
  /*const measuredRef = React.useRef(node => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height)
    }
  })*/
  // Then <h1 ref={measuredRef.current}>Hello, world</h1>

  return (
    <>
      <h1 ref={measuredRef}>Hello, world</h1>
      <h2>The above header is {Math.round(height)}px tall</h2>
    </>
  )
}
```

In this example, we use only one hook instead of `useRef` and `useCallback`

## Pass params with `history.push` in react-router-dom

<strong>Passing parameters with history push:</strong>

```jsx
import { useHistory } from "react-router-dom";

const FirstPage = props => {
    let history = useHistory();

    const someEventHandler = event => {
       history.push({
           pathname: '/secondpage',
           search: '?query=abc',
           state: { detail: 'some_value' }
       });
    };

};
```

<strong>Accessing the passed parameter using useLocation from 'react-router-dom':</strong>

```jsx
import { useEffect } from "react";
import { useLocation } from "react-router-dom";

const SecondPage = props => {
    const location = useLocation();

    useEffect(() => {
       console.log(location.pathname); // result: '/secondpage'
       console.log(location.search); // result: '?query=abc'
       console.log(location.state.detail); // result: 'some_value'
    }, [location]);

};
```

## Remove Contradicting State

<https://profy.dev/article/react-usestate-pitfalls>
This is the case when we have too much state too update and those states relates to each other, for example: loading state, error state and data state can come together

Solution: using `useReducer`, put all states in a wrapper state and update that big state.

## Escape hatch (using less dependancies)

### The latest ref

<https://tkdodo.eu/blog/refs-events-and-escape-hatches>
```jsx
export const useDebouncedState = (callback, delay) => {
  const [value, setValue] = React.useState('')
  // 👇 store callback in a ref
  const ref = React.useRef(callback)

  // 👇 update the ref when the callback changes
  React.useLayoutEffect(() => {
    ref.current = callback
  }, [callback])

  React.useEffect(() => {
    const timeoutId = setTimeout(() => {
      if (value) {
        // 👇 use the ref instead of the callback
        ref.current(value)
      }
    }, delay)

    return () => {
      clearTimeout(timeoutId)
    }
  // 👇 no need to include the callback
  }, [value, delay])

  return [value, setValue]
}
```
