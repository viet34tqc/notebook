# Optimize React

<https://github.com/twclark0/react-performance>

## Debounce callbacks on DOM events

use `debounce` from lodash library

`import {debounce} from "lodash";`

## use React Window package to virtualize long list

## Tree shaking modules

Instead of importing like above, we should do like this

`import debounce from "lodash/debounce";`

## Lazy load heavy components dùng Intersection Observer

<https://vercel.com/blog/how-we-made-the-vercel-dashboard-3x-faster>
  
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