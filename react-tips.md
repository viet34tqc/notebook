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

