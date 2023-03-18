## Reset route component

Use a React key to tell React to remount the component. To do this for all pages, you can use a custom app:

```js
// pages/_app.js
import { useRouter } from 'next/router'

export default function MyApp({ Component, pageProps }) {
  const router = useRouter()
  return <Component key={router.asPath} {...pageProps} />
}
```
