# React server component

## References

- <https://react.gg/visualized#slot-pattern>
- <https://tigerabrodi.blog/why-is-react-server-components-actually-beneficial-full-history>
- <https://focusreactive.com/breaking-down-next-js-14>
- Concept: <https://demystifying-rsc.vercel.app/> and [official documentaion](https://nextjs.org/docs/getting-started/react-essentials)
- Caching: <https://www.youtube.com/watch?v=VBlSe8tvg4U>

## What do React Server Components do?

React Server Components are executed in server in order to generate flight (payload) data, and then it is sent to the client to build and update the Virtual DOM.

To do this, React serialize the React element tree on server executed components and send that serialized string to client. On client, this payload will be deserialized

When you build your React application with server components enabled, the server component code is bundled separately. This server bundle is deployed to your server environment and is used exclusively there, meaning that the final client bundle that runs in the browser does not include any server component code.

## Why RSC

- <https://www.mux.com/blog/what-are-react-server-components>
- <https://www.joshwcomeau.com/react/server-components/>
- <https://www.webscope.io/blog/so-why-server-components>
- <https://vercel.com/blog/understanding-react-server-components>
- <https://medusajs.com/blog/client-server-transition-learnings-nextjs-14-server-components/>

Concept: <https://saewitz.com/the-mental-model-of-server-components>

TLDR

- RSC reduces bundle size on the client, because the rendered Server Components are not included in the JS bundle (they never hydrate or re-render) => faster TTI
- Decreasing data fetching time because the code is often deployed on the same VPS with DB
- Reduce the number of client request and prevent client-side data fetching waterfalls. If we fetching in the client components, it will lead to a data fetching waterfall
- Better for security, we can hide the API keys and other secret keys from client

Server components **fetch the data and render entirely on the server**, and the resulting HTML is streamed into client-side component. This process eliminates the need for client-side re-rendering, thereby improving performance.

By moving the majority of your application code to the server, RSCs help to prevent client-side data fetching waterfalls, where requests stack up against each other and have to be resolved in serial before the user can continue. Server-side fetches have a much smaller overhead, as they don't block the whole client and they resolve much more quickly.

When an RSC needs to be re-rendered, due to state change, it refreshes on the server and seamlessly merges into the existing DOM without a hard refresh. As a result, the client state is preserved even as parts of the view are updated from the server.

## When Should You Use RSCs?

- If I have a highly interactive app, use Client Components.
- On the other hand, if I had a lot of DB access and complex logic, I might offload that to the server by doing it on a Server Component.

## Server component and client component

- A Server Component runs exclusively on the server. This code will not re-run on the user's device; the code won't even be included in the JavaScript bundle
- A Client Component is a component that runs on both the server and client. It is pre-rendered on server and hydrated on client. Every React component you've ever written in 'traditional' (pre-RSC) React is a Client Component. It's a new name for an old thing.

## Streaming

When streaming, you can send UI in small chunks from server to client, without needing to wait until all of your data has been loaded

On a page, each component can be considered a chunk. Static components (e.g. layout, product information) that don't rely on data can be pre-rendered at build time and sent first, then React can start hydration earlier.

Components that require data fetching (e.g. reviews, related products) can be sent in the same server request after their data has been fetched. This is done by wrapping the components that require dynamic data within `<Suspense>` boundaries and providing a fallback. As the user views the page, the server streams in the dynamic data, replacing the static fallbacks.

## Server actions

- define a server action
  - server action accepts a `formData` as param
  - Then you can send POST request to the server and `revalidatePath`
- In client form, add the server action to the `action` attribute of form

## 'use server', 'use client'

<https://overreacted.io/functional-html>

- `use server`: 
  - mark server action as exported from server and be callable from FE. FE sees them as async functions that call the backend via HTTP.
  - It says: when you try to serialize this function, turn it into a Server Reference—an address that the client can use to call this function
- `use client`: exports client functions to the server. Under the hood, the backend code sees them as references like '/src/frontend.js#LikeButton'. BE doesn't get the real function, just a reference to the function. Then the client functions can be rendered as JSX tags and will ultimately turn into `<script>` tags. (You can optionally pre-run those scripts on the server to get their initial HTML.)

```
<!-- Optional: Initial HTML -->
<button class="liked">
  8 Likes
</button>
 
<!-- Interactivity -->
<script src="./frontend.js"></script>
<script>
  const output = LikeButton({
    postId: 42,
    likeCount: 8,
    isLiked: true
  });
  // ...
</script>
```

These directives express the network gap within your module system. They let you describe a client/server application as a single program slit into two environments.
