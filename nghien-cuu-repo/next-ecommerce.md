# Next Ecommerce

## Icon được chia thành từng file. Mỗi file trả về 1 svg

```jsx

const Bag = ({ ...props }) => {
  return (
    <svg
      width="20"
      height="22"
      viewBox="0 0 20 22"
      fill="none"
      stroke="currentColor"
      {...props}
    >
      ....
    </svg>
  )
}

export default Bag
```

## Kết hợp Module CSS với tailwind

Sử dụng class kèm theo @apply. Class cha đặt tên là `root`

```css
.root {
  @apply relative flex items-center;
}
```

## Component sử dụng prop cho HTML tag

```jsx
interface ContainerProps {
  children?: any
  el?: HTMLElement
}

const Container: FC<ContainerProps> = ({
  type = 'div',
  children
})

let Component: React.ComponentType<React.HTMLAttributes<HTMLDivElement>> =
    el as any
    
  return <Component className={rootClassName}>{children}</Component> 
```

## UI Context

- Project có nhiều loại modal và sidebar
- Dùng 1 context tên là UIContext truyền state và hàm để đóng mở sidebar tương ứng
- Các sidebar/modal nằm trong 1 wrapper. Sidebar/Modal được gọi ra tương ứng với với state

## Khai báo proptype

```jsx
interface MarqueeProps {
  className?: string
  children?: ReactNode[] | Component[] | any[]
  variant?: 'primary' | 'secondary'
}
const Marquee: FC<MarqueeProps> = ({
  className = '',
  children,
  variant = 'primary',
}) =>
```

## Prefetch search page in SearchBar component

<https://nextjs.org/docs/api-reference/next/router#usage-2>
  
```js
useEffect(() => {
    router.prefetch('/search')
}, [router])
```

## use React hook like instead of utility funtion

`usePrice`
In case you need to use other react hooks like `useMemo` inside utility function

## Component

### NavbarRoot:

Add a class if scrollTop > 0

```js
const [hasScrolled, setHasScrolled] = useState(false)

  useEffect(() => {
    const handleScroll = throttle(() => {
      const offset = 0
      const { scrollTop } = document.documentElement
      const scrolled = scrollTop > offset

      if (hasScrolled !== scrolled) {
        setHasScrolled(scrolled)
      }
    }, 200)

    document.addEventListener('scroll', handleScroll)
    return () => {
      document.removeEventListener('scroll', handleScroll)
    }
  }, [hasScrolled])
```

### Container

instead of adding a 'container' CSS class, we use a Container component

### Head and SEO

`Head` is parent component of SEO (actually we can merge Head into SEO to avoid name conflict with Head component of Next) and using in `_app.tsx`. If we want to override default meta value like in `ProductView` component, just use SEO component again.

## Swatch

Using button instead of input radio

### LoadingDots

## Packages

- `body-scroll-lock`: khóa scroll khi modal hiện
- `email-validator`: <https://github.com/manishsaraan/email-validator/blob/master/index.js>




