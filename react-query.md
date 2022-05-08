# React Query

## Reference

<https://tsh.io/blog/react-query-tutorial/>
<https://www.carlrippon.com/>

## Important terms

- queries: <https://react-query.tanstack.com/guides/queries>
- queriesKey: <https://react-query.tanstack.com/guides/query-keys>
- caching: <https://react-query.tanstack.com/guides/caching>
- staleTime: The duration until a query transitions from fresh to stale. As long as the query is fresh, data will always be read from the cache only, no network request will happen! If the query is stale (which per default is: instantly), you will still get data from the cache, but a background refetch can happen under certain conditions.


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

## Hooks

### useQuery

- Used to fetch the data.
- Input
  - unique key
  - fetching function: asynchronous function responsible for getting data or return error
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
- Usage:
  - Parallel queries: excute mutiple queries simultaneously. It accepts an array of query options object - like `useQuery` hook - to return an array of query result.
  ```javascript
	const results = useQueries([
	  { queryKey: ['article', 1], queryFn: createFetchArticle },
	  { queryKey: ['article', 2], queryFn: createFetchArticle },
	])
  ```
	- Dependent queries
	This is useful when you want to use the data returned from the first query to execute the second query. Do that with `enabled` option in `useQuery`.
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
	- Paginated query:
	You just need to pass the page number to the `queryKey` array and use `keepPreviousData` option in the `useQuery` hook. Afterward, when you jump between pages, you get cache data from the first one with new data loading in the background. The UI won't reload when you come back to the previous page when the data is the same.
	```javascript
	const query = useQuery(['articles', page], () => createFetchArticles(page), { keepPreviousData : true })
	```

### useMutation

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
  - `onMutate`: function fires before `useMutation`. It's quite helpful when you want to run optimistic updates on local cache and update data for the UI before the mutation happens on the server.
  - `onSuccess`: function runs when the mutation is successfull. `queryClient.invalidateQueries` is often used here, so the data in other places is refetched in the background.
  ```javascript
  const mutation = useMutation(createPostArticle, {
	  onSuccess: data => {
	    queryClient.invalidateQueries('articles')
	  },
	})
  ```
  - `onError`: fires when the mutation encounters an error

### useQueryClient
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

### useFetching

Check if React Query is fetching new data

## React Query and React Router

React Router will unmount your component when you navigate away from it

When a component that uses a react-query hook mounts, it will trigger a fetch (because of mounting). However, if you already have data in the cache for this specific key, you will instantly get that data returned as "stale data", and the fetch will happen in the background only.

If you want to avoid the request going out, you can either:

- set refetchOnmount to false so that there will be no refetch when the component mounts
- set a staleTime on your query

I would suggest setting the staleTime: It will tell react-query for how long the data should be considered "fresh". If you set it to 5 minutes, there will be no background-refetch for 5 minutes - the data will always come from the cache directly. staleTime defaults to 0, so you will always get background refetches.

## React Query and useEffect and useState

Cases: you have a load more button to fetch more posts. The posts here need to be a state because it will change whenever the button load clicks.

First, you have to put `useEffect` and `useState` above the conditional checks (`isLoading` and `isError`).
Then, The data get from `useQuery` have to be an dependency of `useEffect`. Inside the `useEffect` block, there should be a check for undefined `useQuery` data.