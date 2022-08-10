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

## Module CSS với tailwind

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

- Xử lý sidebar và modal
- Dùng 1 context tên là UIContext truyền state và hàm để đóng mở sidebar
- Các sidebar nằm trong 1 sidebar wrapper. Sidebar được gọi ra tương ứng với với state

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







