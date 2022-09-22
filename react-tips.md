# React Tips

## Use callback refs to interact with DOM nodes

<https://tkdodo.eu/blog/avoiding-use-effect-with-callback-refs>

```js
const ref = useRef(null)
// is equal to
const ref = (node) => {}
```

```jsx
function MeasureExample() {
  const [height, setHeight] = React.useState(0)

  const measuredRef = React.useCallback(node => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height)
    }
  }, [])

  return (
    <>
      <h1 ref={measuredRef}>Hello, world</h1>
      <h2>The above header is {Math.round(height)}px tall</h2>
    </>
  )
}
```

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
