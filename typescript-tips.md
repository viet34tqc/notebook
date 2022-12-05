# Typescript tips

## Type for lookup table object

<https://www.lloydatkinson.net/posts/2022/react-conditional-rendering-with-type-safety-and-exhaustive-checking>

```tsx
import { ReactNode } from 'react';

const Apple = () => <span>🍎🍏</span>;
const Kiwi = () => <span>🥝</span>;
const Cherry = () => <span>🍒</span>;
const Grape = () => <span>🍇</span>;

const icon: Record<Fruit, ReactNode> = {
    apple: <Apple />,
    kiwi: <Kiwi />,
    cherry: <Cherry />,
    grape: <Grape />,
};
```
