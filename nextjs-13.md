# NextJS 13

## References

- Concept: <https://demystifying-rsc.vercel.app/> and [official documentaion](https://nextjs.org/docs/getting-started/react-essentials)

## What do React Server Components do?

- <https://www.joshwcomeau.com/react/server-components/>
- <https://vercel.com/blog/understanding-react-server-components>

Server components **fetch the data and render entirely on the server** (SSR only render the HTML on the server then we will send a nother request to fetch the data), and the resulting HTML is streamed into client-side component. This process eliminates the need for client-side re-rendering, thereby improving performance.

By moving the majority of your application code to the server, RSCs help to prevent client-side data fetching waterfalls, where requests stack up against each other and have to be resolved in serial before the user can continue. Server-side fetches have a much smaller overhead, as they don't block the whole client and they resolve much more quickly.

When an RSC needs to be re-rendered, due to state change, it refreshes on the server and seamlessly merges into the existing DOM without a hard refresh. As a result, the client state is preserved even as parts of the view are updated from the server.

## Server actions

- define a server action
  - server action accepts a `formData` as param
  - Then you can send POST request to the server and `revalidatePath`
- In client form, add the server action to the `action` attribute of form
