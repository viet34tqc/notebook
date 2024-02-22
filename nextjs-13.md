# NextJS 13

## References

- Concept: <https://demystifying-rsc.vercel.app/> and [official documentaion](https://nextjs.org/docs/getting-started/react-essentials)
- Caching: <https://www.youtube.com/watch?v=VBlSe8tvg4U>

## What do React Server Components do?

- <https://www.joshwcomeau.com/react/server-components/>
- <https://www.webscope.io/blog/so-why-server-components>
- <https://vercel.com/blog/understanding-react-server-components>
- <https://medusajs.com/blog/client-server-transition-learnings-nextjs-14-server-components/>

TLDR

- RSC reduces bundle size on the client, because the rendered Server Components are not included in the JS bundle (they never hydrate or re-render) => faster TTI
- Decreasing data fetching time because the code is often deployed on the same VPS with DB
- Reduce the number of client request and prevent client-side data fetching waterfalls. If we fetching in the client components, it will lead to a data fetching waterfall
- Better for security, we can hide the API keys and other secret keys from client

When to use: any component can be a Server Component, **unless** it needs client-side interactivity

Server components **fetch the data and render entirely on the server**, and the resulting HTML is streamed into client-side component. This process eliminates the need for client-side re-rendering, thereby improving performance.

By moving the majority of your application code to the server, RSCs help to prevent client-side data fetching waterfalls, where requests stack up against each other and have to be resolved in serial before the user can continue. Server-side fetches have a much smaller overhead, as they don't block the whole client and they resolve much more quickly.

When an RSC needs to be re-rendered, due to state change, it refreshes on the server and seamlessly merges into the existing DOM without a hard refresh. As a result, the client state is preserved even as parts of the view are updated from the server.

## Streaming

When streaming, you can progressively send UI from server to client, without needing to wait until all of your data has been loaded

On a page, each component can be considered a chunk. Static components (e.g. layout, product information) that don't rely on data can be pre-rendered at build time and sent first, then React can start hydration earlier.

Components that require data fetching (e.g. reviews, related products) can be sent in the same server request after their data has been fetched. This is done by wrapping the components that require dynamic data within `<Suspense>` boundaries and providing a fallback. As the user views the page, the server streams in the dynamic data, replacing the static fallbacks.

## Server actions

- define a server action
  - server action accepts a `formData` as param
  - Then you can send POST request to the server and `revalidatePath`
- In client form, add the server action to the `action` attribute of form
