# F8 UI

## `placeholder-shown` selector

Dùng khi select input khi input được gõ text (input có placeholder)

## GlobalStyles

Khai báo 1 component chứa global style làm component cha lớn nhất

## Lấy lại props khi đã destructure

Tạo 1 biến props bên trong component

```jsx
function Button({props: {
    to,
    href,
    rightIcon,
    onClick,
    ...passProps
}}) {
    let Comp = 'button';
    const props = {
        onClick,
        ...passProps,
    };
```

## CSS Modules với clsx

Nếu props dạng boolean thì khi khai báo phải phải để dạng property của object, còn props dạng text thì có thể để ngoài

```js

const classes = clsx(styles.wrapper, styles[size], {
    [styles.disabled]: disabled,
});
```

## Loại bỏ onClick khi button bị disabled

```js
if (disabled) {
    Object.keys(props).forEach((key) => {
        if (key.startsWith('on') && typeof props[key] === 'function') {
            delete props[key];
        }
    });
}
```
## Fallback image

Sử dụng prop `onError` để set fallback image

```jsx
const Image = forwardRef(({ src, alt, className, fallback: customFallback = noImage, ...props }, ref) => {
    const [fallback, setFallback] = useState('');

    const handleError = () => {
        setFallback(customFallback);
    };

    return (
        <img
            className={clsx(styles.wrapper, className)}
            ref={ref}
            src={fallback || src}
            alt={alt}
            {...props}
            onError={handleError}
        />
    );
});

export default Image;

```

## Folder structure

Mỗi component là 1 folder, tên folder là tên component. Trong folder có 1 file chứa tên của component và 1 file `index.js`

```js
// index.js
export { default } from './AccountItem';
```
File `index.js` có nhiệm vụ export tất cả các component trong thư mục ra ngoài. 
Khi import component, thay vì viết
```js
import AccountItem from '@components/AccountItem/AccountItem';
```
Ta sẽ viết ngắn hơn
```js
import AccountItem from '@components/AccountItem';
```

## Overflow children component khi set max-height cho parent component

Component PopperMenu có 2 children: header và body.

Lúc này cần set `display: flex` và `flex-direction` cho parent. header phải set `flex-shrink: 0` để không bị body đẩy lên.
