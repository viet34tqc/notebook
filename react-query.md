# React Query

## Reference

- <https://tigerabrodi.blog/become-expert-in-react-query>
- <https://tsh.io/blog/react-query-tutorial/>
- <https://www.carlrippon.com/>

## Important terms

- querieKey: 
  - Query keys are like dependencies in useEffect
  - They define the unique identity of your data
  - When they change, you get a refetch (for example: queryKey includes filter object and the the filter object changes)
- queryFn: It's a functions returning Promises.

```js
// ❌ This doesn't work
// You need to pass a function
useQuery({
  queryKey: ["todos"],
  queryFn: Promise.resolve({ todos: [] }),
});

// ✅ This works
useQuery({
  queryKey: ["todos"],
  queryFn: () => Promise.resolve({ todos: [] }),
});

// ✅ Common example
useQuery({
  queryKey: ["todos"],
  queryFn: () => fetch("/api/todos").then((r) => r.json()),
});
```

- caching: <https://react-query.tanstack.com/guides/caching>
- staleTime: The duration until a query transitions from fresh to stale. As long as the query is fresh, data will always be read from the cache only, no network request will happen! If the query is stale (which per default is: instantly), you will still get data from the cache, but a background refetch can happen under certain conditions.
    
```js
// Data becomes stale after 5 minutes
useQuery({ staleTime: 1000 * 60 * 5 });
```

- inactive query: No components use the query

```jsx
// Component A
function TodoList() {
  // This query is "active" because the component is using it
  const { data } = useQuery({
    queryKey: ['todos'],
    gcTime: 1000 * 60 * 5 // 5 minutes
  })
  return <div>{data.map(...)}</div>
}

// When TodoList unmounts (user navigates away), the query becomes "inactive"
// If user doesn't come back to TodoList within 5 minutes (gcTime),
// the data is removed from cache
// If they return within 5 minutes, the cached data is still there!
```

- gcTime: The duration until inactive queries will be removed from the cache. This defaults to 5 minutes. Queries transition to the inactive state as soon as there are no observers registered, so when all components which use that query have unmounted.

```js
useQuery({
  queryKey: ['todos'],
  // If this query becomes inactive (no components using it),
  // wait 10 minutes before removing it from cache
  gcTime: 1000 * 60 * 10
})
```

- Invalidation: Telling React Query "hey, this data might be outdated, time to fetch fresh data!

## Why do I need React Query when the browser has built-in fetch API already?

One of the challenges we face when building an application with React is fetching data from the server. The most common way to handle data fetching is to use global state to determine the current status of the fetch operation

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';
// regular fetch with axios
function App() {
 const [isLoading, setLoading] = useState(false);
 const [isError, setError] = useState(false);
 const [data, setData] = useState({});

 useEffect(() => {
  const fetchData = async () => {
   setError(false);
   setLoading(true);

   try {
    const response = await axios('http://swapi.dev/api/people/1/');

    setData(response.data);
   } catch (error) {
    setError(true);
   }
   setLoading(false);
  };
  fetchData();
 }, []);
 return (
  <div className="App">
   <h1>React Query example with Star Wars API</h1>
   {isError && <div>Something went wrong ...</div>}

   {isLoading ? (
    <div>Loading ...</div>
   ) : (
    <pre>{JSON.stringify(data, null, 2)}</pre>
   )}
  </div>
 );
}
export default App;
```

The code above has some problems:

- It requires both `useState` and `useEffect` hooks and three different states for data, loading state and error.
- Server data is not stored in your app and could be stale. You need to figure out a way for caching and invalidating that data.

Fortunately, all those problem can be solved by React Query:

- Code is simpler, no need to use multiple `useState` anymore
- Cache the fetched data. When the component is unmounted, eg: when you click on another link, then you come back, the data is already cached and your component don't need to fetch the data again.

## Refetch

If the query is not stale (fresh), it won't be refreshed
**Stale** queries are refetched automatically in the background when:

- New instances of the query mount
- The window is refocused
- The network is reconnected
- The query is optionally configured with a refetch interval

## React Query and global state managers

There is a huge difference between these two

- React Query is **server-state** library, responsible for managing asynchronous operation between you and server
- On the other hand, Redux, MobX, Zustand, etc. are **client-state** libraries that can be used to store asynchronous data, albeit inefficiently when compared to a tool like React Query

React Query replaces all the boilerplate code used to manage cache data in your client-state with just a few line of code.

## Key benefit of React Query

- ***Window focus refetching***: when a user leaves your app tab, React Query marks the data “stale” and refetches it when that person returns.
- ***Request retry***: you can set an amount of retries for any request to combat random errors.
- ***Prefetching***: if your app needs fresh data after an update request, you can prefetch the query with a specific key, and React Query will update it in the background.
- ***Optimistic Updates***: when you edit or delete an item in a list, you can issue an optimistic update of the list, which will update the data on your local first before send that update to the server.

## How React Query update the data

Under the hood, it uses the user interactions as the hints to know when to update the data. One of those hints is window refocus event. So when you refocus the browser, it detects that the users has came back, and it's time to update stale data in the background.

## useQuery

- Used to fetch the data.
- Input
  - unique key
  - fetching function: any function that return a promise that resolve data or throw error when excuted (async function returns promise, not the data from await)
  - options(optional): Object
- Output: Object
ex:

```javascript
export default function usePosts() {
  return useQuery('posts', () =>
    axios.get('/api/posts').then((res) => res.data)
  )
}
```

## Checking status

<https://github.com/TanStack/query/discussions/6297>

Check `isPending` first, then `isError`, now the data is guaranteed to be available

## `useMutation`

- Used to create/delete/update data
- Input: function for mutation and options ( which will utilize queryClient)
- Output:
  - function as the event handle: (value) => void
  - mutationConfigure Info

```javascript
export default function useCreatePost() {
  return useMutation( (newPost) => axios.post('/api/posts', newPost).then((res) => res.data))
}

// createPost is used for submit event
// createPostInfo is an object with isLoading, isError, isSuccess properties
const [createPost, createPostInfo] = useCreatePost()
```

- Options:
  - `onMutate`: function fires before `useMutation`. It's quite helpful when you want to run optimistic updates on local cache and update data for the UI before the mutation happens on the server (<https://tanstack.com/query/v4/docs/guides/optimistic-updates>)
  - `onSuccess`: function runs when the mutation is successfull. `queryClient.invalidateQueries` is often used here, so the data in another query is refetched in the background.

  ```javascript
  const mutation = useMutation(createPostArticle, {
    onSuccess: data => {
      queryClient.invalidateQueries('articles')
    }
  })
  ```

  - `onError`: fires when the mutation encounters an error

### What to do after mutation

You might want to `invalidateQueries`. Let's say you have a modal to add post and a list of post on the same page. Now you want to create a post and update the list asap. Then you need to `invalidateQueries` the query for `posts` after create post.

When using `invalidateQueries`, it will call another API for the query to get the latest data, update the cached data and display screen. If the current component contains invalidated query, it will re-render

You can also use `setQueryData` to manually update the cached data for the query. The advantage of `setQueryData` is that it doesn't make an additional API like `invalidateQueries`. However, in the end we still want to invalidate the query in `onSuccess` or `onSettled`

```js
  onSuccess: (data) => {
    // data is the response from mutationFn. If response doesn't return it, you need to find another way to get the ID
  queryClient.setQueryData(discussionKeys.all(), (previous: any) =>
    previous.filter((d: Discussion) => d.id !== data.id)
  );
}
```

If the query include a lot of params like a filters, your best bet is invalidate the whole query without any subkeys <https://tkdodo.eu/blog/effective-react-query-keys>

```js
const { filters } = useFilterParams()

return useMutation(updateTitle, {
  onSuccess: (newTodo) => {
    queryClient.setQueryData(['todos', 'detail', newTodo.id], newTodo)

    // ✅ update the list we are currently on instantly
    queryClient.setQueryData(['todos', 'list', { filters }], (previous) =>
      previous.map((todo) => (todo.id === newTodo.id ? newtodo : todo))
    )

    // 🥳 invalidate all the lists, but don't refetch the active one
    queryClient.invalidateQueries({
      queryKey: ['todos', 'list'],
      refetchActive: false,
    })
  },
})
```

## useQueryClient

This hook return the queryClient instance. This instance helps a lot with caching

```javascript
import { useQueryClient } from 'react-query'

const queryClient = useQueryClient()
```

- ***inValidateQueries***: mark a query as stale and potentially refetch them.

```javascript
export default function useCreatePost() {
  return useMutation(
    (values) => axios.post('/api/posts', values).then((res) => res.data),
    {
      onSuccess: () => {
        queryClient.invalidateQueries('posts') // Tell React Query the data with key 'posts' is stale, and React Query will update the data wherever using useQuery to fetch the posts
      },
    }
  )
}
```

## Parallel queries

Excute mutiple queries simultaneously. It accepts an array of query options object - like `useQuery` hook - to return an array of query result.

  ```javascript
 const results = useQueries([
   { queryKey: ['article', 1], queryFn: createFetchArticle },
   { queryKey: ['article', 2], queryFn: createFetchArticle },
 ])
  ```

## Dependent queries

This is useful when you want to use the data returned from the first query to execute the second query. Do that with `enabled` option in `useQuery`

```javascript
const { data: user } = useQuery(['user', email], getUserByEmail)

const userId = user?.id

const { isIdle, data: projects } = useQuery(
  ['projects', userId],
  getProjectsByUser,
  {
    // The query will not execute until the userId exists
    enabled: !!userId,
  }
)
```

## Lazy queries

<https://tanstack.com/query/v4/docs/guides/disabling-queries>

```jsx
function Todos() {
  const [filter, setFilter] = React.useState('')

  const { data } = useQuery({
      queryKey: ['todos', filter],
      queryFn: () => fetchTodos(filter), // We can also access the query key in queryFn like this: ({queryKey}) => fetchTodos(queryKey[0])
      // ⬇️ disabled as long as the filter is empty
      enabled: !!filter
  })

  return (
      <div>
        // applying the filter will enable and execute the query
        <FiltersForm onApply={setFilter} />
        {data && <TodosTable data={data}} />
      </div>
  )
}
```

If you want to show a loading spinner, use `isInitialLoading` rather than `isLoading`

## Paginated query

You just need to pass the page number to the `queryKey` array and use `keepPreviousData` option in the `useQuery` hook. Afterward, when you jump between pages, you get cache data from the first one with new data loading in the background. The UI won't reload when you come back to the previous page when the data is the same.

```javascript
const query = useQuery(['articles', page], () => createFetchArticles(page), { keepPreviousData : true })
```

## React Query and Typescript

<https://tkdodo.eu/blog/react-query-and-type-script>

- `useQuery`

You don't have to pass in any generic to `useQuery`. Instead, you'd better carefully define the type of return data from `queryFn`

- Error

```ts
if (groups.error instanceof Error) {
  return <div>An error occurred: {groups.error.message}</div>
}
```

## `initialData`

We often use `initialData` to pre-populate a query with data from another query. A good example of this would be searching the cached data from a todos list query for an individual todo item, then using that as the initial data for your individual todo query:

```js
function Todo({ todoId }) {
  const result = useQuery(['todo', todoId], () => fetch('/todos'), {
    initialData: () => {
      // Use a todo from the 'todos' query as the initial data for this todo query
      return queryClient.getQueryData(['todos'])?.find(d => d.id === todoId)
    },
    staleTime: 60 * 1000, // By default, it is zero, then the query is refetch immediately which is waste
    // The time when initialData was last updated. With this value, the query can decide for itself whether the data needs to be refetched again or not because the query 'todos' might be old.
    // React query will compare the substract of Date.now() and this value with staleTime.
    initialDataUpdatedAt: queryClient.getQueryState(['todos'])?.dataUpdatedAt,
  })
}
```

## Handle errors

While most utilities like `axios` automatically throw errors for unsuccessful HTTP calls, some utilities like fetch do not throw errors by default. If that's the case, you'll need to throw them on your own. Here is a simple way to do that with the popular fetch API

```js
// Using axios
useQuery(['todos', todoId], async () => {
   const response = await axios.get('/todos/' + todoId)
   return response.data
})

// Using fetch
useQuery(['todos', todoId], async () => {
   const response = await fetch('/todos/' + todoId)
   if (!response.ok) {
     throw new Error('Network response was not ok')
   }
   return response.json()
})
```

## React Query and React Router

React Router will unmount your component when you navigate away from it

When a component that uses a react-query hook mounts, it will trigger a fetch (because of mounting). However, if you already have data in the cache for this specific key, you will instantly get that data returned as "stale data", and the fetch will happen in the background only.

If you want to avoid the request going out, you can either:

- set refetchOnmount to false so that there will be no refetch when the component mounts
- set a staleTime on your query

I would suggest setting the staleTime: It will tell react-query for how long the data should be considered "fresh". If you set it to 5 minutes, there will be no background-refetch for 5 minutes - the data will always come from the cache directly. staleTime defaults to 0, so you will always get background refetches.

## React Query and useEffect and useState

Cases: you have a load more button to fetch more posts. The posts here need to be a state because it will change whenever the button clicks.

First, you have to put `useEffect` and `useState` above the conditional checks (`isLoading` and `isError`).
Then, The data get from `useQuery` have to be an dependency of `useEffect`. Inside the `useEffect` block, there should be a check for undefined `useQuery` data.

## Hydration and Dehydration with NextJS

<https://prateeksurana.me/blog/mastering-data-fetching-with-react-query-and-next-js/>

- Dehydration refers to creating a frozen representation of the cache. This can be later hydrated on the browser with React Query's hydrate methods. This is useful if you want to store the cache for later use, for instance in localstorage or in our case sending the cache from server to client.
- Hydration lets you add any previously dehydrated state to the cache on a QueryClient instance with the full functionality of the library when the app is rendering on the browser.

React Query lets you fetch any number of queries you want during any of the Next.js pre-rendering steps and then dehydrate those queries. This allows you to pre-render your markup that will be available with all the data on page load and once the page renders on the client, React Query will hydrate those dehydrated queries with the full functionality of the library.
