# React Server Component

<https://www.mux.com/blog/what-are-react-server-components>

## Why Server-side rendering is not enough

<https://vercel.com/blog/understanding-react-server-components>

- To fetch data, traditional client side react app requires more round trip because the fetching data has to be put into `useEffect` which only run after UI rendering
- It goes the same with Server-side rendering if your pre-rendered component requires another data fetching. For example, in your pre-rendered blog, you need to fetch the comments for that blog page. The only thing SSR does is to make initial page load faster.
- All JavaScript must download from the server before the client can be hydrated with it, even if it's streamed to the browser asynchronously (using `Suspense`). As app complexity increases, so does the amount of code the user downloads.
- All hydration has to complete on the client before anything can be interacted with.

## RSC comes to rescue

RSC will take care the fetching data and leave the interaction for client-side component. And your app can ship both RSCs and client-side components together, which saves save a lot of code for fetching data compared to traditional client rendering or SSR.

Fetching data on the server offers more benefits:

- Access to backend data is more securely
- Prevent client-side data fetching waterfalls
- We can send less JavaScript to the client, leading to smaller bundle sizes and less work during hydration.

## Tradeoff

- Can't use things related to client (hooks, local storage)
- Can't use CSS-in-JS

## Server actions

What if you need to fetch data on the server in response to a user’s action on the client (like a form submission)? 

The client can send data to the server, and the server can do its fetching or whatever, and stream the response back to the client just like it streamed that initial data. This two-way communication is called server actions.

## How do you make a Server Component a child of a Client Component?

In normal flow, you can import client component inside server component but the reverse is not allowed.

To use Server component inside client component, pass Server Components as children or props instead of importing them

To do this properly, we have to go up a level to the nearest Server Component — in this case, ServerPage — and do our work there.

```tsx
import ClientComponent from './ClientComponent.js'
import ServerComponentB from './ServerComponentB.js'

/** 
  * The first way to mix Client and Server Components
  * is to pass a Server Component to a Client Component
  * as a child.
  */
function ServerComponentA() {
  return (
    <ClientComponent>
      <ServerComponentB />
    </ClientComponent>
  )
}
```

