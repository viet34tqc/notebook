# useElementOnScreen

<https://betterprogramming.pub/simple-react-scroll-animations-with-zero-dependencies-b496c1e1c7bd>

```tsx
function useElementOnScreen(
  ref: RefObject<Element>,
  rootMargin = "0px",
) {
  const [isIntersecting, setIsIntersecting] = useState(true);
  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        setIsIntersecting(entry.isIntersecting);
      },
      { rootMargin }
    );
  if (ref.current) {
    observer.observe(ref.current);
  }
  return () => {
    if (ref.current) {
      observer.unobserve(ref.current);
    }
  };
}, []);
  return isIntersecting;
}
```

For a simple fade-in-and-up effect, we can simply alter the opacity and translate CSS properties depend on whether the element is on screen or not.

```tsx
const AnimateIn: FC<PropsWithChildren> = ({ children }) => {
  const ref = useRef<HTMLDivElement>(null);
  const onScreen = useElementOnScreen(ref);
  return (
    <div
      ref={ref}
      style={{
        opacity: onScreen ? 1 : 0,
        translate: onScreen ? "none" : "0 2rem",
        transition: "600ms ease-in-out",
      }}
    >
      {children}
    </div>
  );
};
```
If we want to extend the range of possible animations, we need to make our component a little more generic, allowing developers to pass in their own CSS for the from and to states.

```	tsx
const AnimateIn: FC<
  PropsWithChildren<{ from: CSSProperties; to: CSSProperties }>
> = ({ from, to, children }) => {
  const ref = useRef<HTMLDivElement>(null);
  const onScreen = useElementOnScreen(ref);
  const defaultStyles: CSSProperties = { 
    transition: "600ms ease-in-out",
  };
  return (
    <div
      ref={ref}
      style={
        onScreen
          ? {
            ...defaultStyles,
            ...to,
          }
        : {
            ...defaultStyles,
            ...from,
          }
        }
      >
        {children}
      </div> 
  );
};
```
Now, if we wanted to create a library of simple animations, we could easily build this using our AnimateIn component. For example:

```	tsx
const FadeIn: FC<PropsWithChildren> = ({ children }) => (
  <AnimateIn from={{ opacity: 0 }} to={{ opacity: 1 }}>
    {children}
  </AnimateIn>
);
const FadeUp: FC<PropsWithChildren> = ({ children }) => (
  <AnimateIn
    from={{ opacity: 0, translate: "0 2rem" }}
    to={{ opacity: 1, translate: "none" }}
  >
    {children}
  </AnimateIn>
);
const ScaleIn: FC<PropsWithChildren> = ({ children }) => (
  <AnimateIn from={{ scale: "0" }} to={{ scale: "1" }}>
    {children}
  </AnimateIn>
);
```

```tsx
export const Animate = {
  FadeIn,
  FadeUp,
  ScaleIn,
};
```

We can then call an animation like this:

```tsx
<Animate.ScaleIn>
  <h1>Hello World</h1>
</Animate.ScaleIn>
```


