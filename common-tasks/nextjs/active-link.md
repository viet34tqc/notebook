# Active link in NextJS

If you want to add style to NextJS Link, those styles must be applied to an `a` tag inside NextJs `Link` component
```jsx
const router = useRouter()

// legacyBehavior = true to make Link component not behave as a tag
// passHref = true: pass the href from Link to child component which is an a tag here.
<Link href='/' passHref legacyBehavior>
	<a className={router.pathname === '/' ? 'active': ''}>Home</a>
</Link>
```

Or you can wrap `Link` inside a HTML tag

```jsx
<li
    key={path}
    className={cn(
   
        underline: activeBrand?.id === id,
    )}
  >
    <Link />
```
