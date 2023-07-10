# Optimize React

- **Must READ**: <https://github.com/coryhouse/reactjsconsulting/issues/77>
- <https://reacthandbook.dev/react-performance-optimization>

## State

- Keep state as local as possible. Start by declaring state in the component that uses it. Lift as needed.
- Store data that doesn't need to render in refs
- Use `ref` or Pass a func to `useState` if the initial value calls an expensive func, otherwise, it will be re-calculated on each render.

## Context

- Place context providers as low as possible
- Separate contexts based on types of state

## Using composition

Pass children down (do this before you memo). Why does this work? Because components passed as children don’t re-render since they are just props. Note: don't pass a function as a child, since it'll still re-render because the func will be recreated on each render

## Use tailwind over CSS-in-JS

## Memoization

- Memoize expensive operations via `useMemo`
- Avoid needless renders via `React.memo`
- Consider wrapping functions passed to children in `useCallback` to avoid needless renders in children
- Prefer pure functions (which can be extracted from the component) over `useCallback` when possible

## Debounce callbacks on Input events

Use `debounce` from lodash library

`import {debounce} from "lodash";`

## use `React Window package` to virtualize long list

## Avoid named imports when importing third party libraries / components

Instead of importing like above (we will import the whole library), we should do like this

`import debounce from "lodash/debounce";`

## Lazy load heavy components if it's not in the viewport with `Intersection Observer`

- <https://vercel.com/blog/how-we-made-the-vercel-dashboard-3x-faster>
- <https://usehooks.com/useintersectionobserver>

```jsx
const useInViewport = (ref: any) => {
  const [isInViewport, setIsInViewport] = useState(false)
  const [isLoaded, setIsLoaded] = useState(false)

  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          setIsLoaded(true)
        }

        setIsInViewport(entry.isIntersecting)
      }
    )

    if (ref.current) {
      observer.observe(ref.current)
    }
  }, [ref])

  return  isInViewport || isLoaded
}

const HeavyChildComponent = () => {
  return (
    <div>
      <p>I am a heavy component!</p>
    </div>
  )
}

const ParentComponent = () => {
  const childRefContainer = useRef(false)
  const isRendered = useInViewport(childRefContainer)

  return (
    <>
      <div style={{height: "200vh"}}></div>
        <div ref={childRefContainer as any}>
          { isRendered && <HeavyChildComponent /> }
        </div>
    </>
  )
}
```