# Optimize React

- **Must READ**: <https://github.com/coryhouse/reactjsconsulting/issues/77>
- <https://reacthandbook.dev/react-performance-optimization>
- <https://hackernoon.com/front-end-optimization-my-journey-to-accelerate-load-times-in-heavy-front-end>

Optimize React app is all about optimizing bundle size and reducing unnecessary re-renders

## Packages

In an react app, we often use many packages that could lead to heavy bundle size. So we should use them carefully by:

- Reduce unnecessary packages from bundle
- Use tree shaking packages
- Dynamic import the scripts which are not necessary at first load, eg: scripts only needed when we make an interaction => increase TTI
- Use tailwind over CSS-in-JS 

## State

We need to use state carefully because state change makes component re-render

- Keep state as local as possible. Start by declaring state in the component that uses it. Lift as needed.
- Store data that doesn't need to render in refs
- Use `ref` or Pass a func to `useState` if the initial value calls an expensive func, otherwise, it will be re-calculated on each render.

## Context

- Place context providers as low as possible. Only wrap the components that need the context to avoid unnecessary renders.
- Separate contexts based on types of state

## Using composition

Pass children down (do this before you memo). Why does this work? Because components passed as children don’t re-render since they are just props. Note: don't pass a function as a child, since it'll still re-render because the func will be recreated on each render

## Web workers

<https://tropicolx.hashnode.dev/optimizing-performance-in-react-applications?ref=dailydev#heading-web-workers>

## Memoization

- Memoize expensive operations via `useMemo`
- Avoid needless renders via `React.memo`
- Consider wrapping functions passed to children in `useCallback` to avoid needless renders in children
- Prefer pure functions (which can be extracted from the component) over `useCallback` when possible

## Debounce callbacks on Input events

Use `debounce` from lodash library

`import {debounce} from "lodash/debouce";`

## Use `React Window package` to virtualize long list

## Avoid named imports when importing third party libraries / components

For some packages that doesn't use ES6 module for module system, instead of importing like above (we will import the whole library), we need to use a technique called cherry-picking

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

## Improve INP

<https://vercel.com/blog/improving-interaction-to-next-paint-with-react-18-and-suspense>

- Break long task: you can take hydration off the main thread and make it non-blocking, simply by creating a Suspense boundary. By this way, the UI stays responseive.
- Using CSS instead of JavaScript for animations, since a separate thread handles CSS, called the browser's compositor thread. Some CSS properties, such as transforms, can also use the GPU for rendering.
- Throttling or debouncing events—especially ones driven by scrolling—that may be called repeatedly by user input.
- Reducing your DOM size, to avoid having the browser recalculate too many elements on each render.
- Related to the above, inline SVGs can be especially troublesome if you have too many or if they end up in your client-side JS bundle (for example, by inlining them in JSX). You may need reference them in an <img> tag or look into alternate ways of rendering them, such as keeping them in React Server Components.
- In the case of a complex application, using web workers to arbitrarily execute JavaScript on separate threads, keeping the main thread open for user input.
- Lazy loading images, fonts, or scripts, which Next.js can automatically do for you.