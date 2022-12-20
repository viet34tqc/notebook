# Tozo

Link github: <https://github.com/pgjones/tozo>

## Add initialData when call request for single item

Normally, we will call the request for the list first then we navigate to single item page. So, we might not need additional request for the single item. That's where `initialData` comes in. We often use `initialData` with `staleTime` to prevent refetching the query again on mount (this is default behaviour).

```ts
export const useTodoQuery = (id: number) => {
  const queryClient = useQueryClient();
  return useQuery<Todo>(
    ["todos", id.toString()],
    async () => {
      const response = await axios.get(`/todos/${id}/`);
      return new Todo(response.data);
    },
    {
      initialData: () => {
        return queryClient
          .getQueryData<Todo[]>(["todos"])
          ?.filter((todo: Todo) => todo.id === id)[0];
      },
      staleTime: STALE_TIME, // Stale time prevent refetching the query again on mount.
    },
  );
};
```

## Private route

```jsx
const AuthContext = createContext<IAuth>({
  authenticated: true,
  setAuthenticated: (value: boolean) => {},
});

const RequireAuth = ({ children }: IProps) => {
  const { authenticated } = useContext(AuthContext);
  const location = useLocation();

  if (authenticated) {
    return <>{children}</>;
  } else {
    return <Navigate state={{ from: location }} to="/login/" />;
  }
};

```

## Check axios error

`if (axios.isAxiosError(error))`