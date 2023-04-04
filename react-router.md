# React Router

## Example

<https://scrimba.com/learn/reactrouter6>
<https://www.robinwieruch.de/categories/react-router-6/>
<https://mitanshu.hashnode.dev/react-router-navigate-like-a-pro>
<https://reacttraining.com/blog/react-router-v6-pre/>

## Core Components

Here are the components used when you are working with React Router DOM: `<Routes>`, `<Route>`, `<Outlet />`

There are two ways to setup the routes in react router dom v6:

+ Each group of routes (include parent and child route) is defined in separate component

```jsx
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="users/*" element={<Users />} />
      </Routes>
    </BrowserRouter>
  );
}

function Users() {
  /* All <Route path> and <Link to> values in this
     component will automatically be "mounted" at the
     /users URL prefix since the <Users> element is only
     ever rendered when the URL matches /users/*
  */
  return (
    <div>
      <nav>
        <Link to="me">My Profile</Link>
      </nav>

      <Routes>
        <Route path="/" element={<UsersIndex />} />
        <Route path=":id" element={<UserProfile />} />
        <Route path="me" element={<OwnUserProfile />} />
      </Routes>
    </div>
  );
}
```

+ Define all the routes in top level component and using `<Outlet />` to render nested child routes

```jsx
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="users" element={<Users />}>
          {/* Always set the first child route for parent layout */}
          <Route index element={<UserProfile />} /> 
          <Route path=":id" element={<UserProfile />} />
          <Route path="me" element={<OwnUserProfile />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

function Users() {
  return (
    <div>
      <nav>
        <Link to="me">My Profile</Link>
      </nav>

      <Outlet />
    </div>
  );
}

```

`NavLink`: is another version of `Link`. It add a classname 'active' (default) to the link if it matches the current URL

## Declare the routes using plain JS objects

Besides using those hooks above, React Router DOM v6 ships with another API for routing, that is `useRoutes` hook. This hook let you declare your routes with JS objects instead of React component.

```jsx
function App() {
  // We removed the <BrowserRouter> element from App because the
  // useRoutes hook needs to be in the context of a <BrowserRouter>
  // element. This is a common pattern with React Router apps that
  // are rendered in different environments. To render an <App>,
  // you'll need to wrap it in your own <BrowserRouter> element.
  let element = useRoutes([
    // A route object has the same properties as a <Route>
    // element. The `children` is just an array of child routes.
    { path: '/', element: <Home /> },
    {
      path: 'users',
      element: <Users />,
      children: [
        { path: '/', element: <UsersIndex /> },
        { path: ':id', element: <UserProfile /> },
        { path: 'me', element: <OwnUserProfile /> },
      ]
    }
  ]);

  return element;
}

function Users() {
  return (
    <div>
      <nav>
        <Link to="me">My Profile</Link>
      </nav>

      <Outlet />
    </div>
  );
}
```

## Authorization and authentication

In an application, we more often than not have 2 types of authorization

- Route Authorization: there are 2 ways:
  - Prevent user from accessing the route by redirecting them according to user role: create a ProtectedRoute component and check the user role inside that component
  - Filter route according to user role
- UI Authorization: hide something on the UI
  - Based on user role
  - Based on user's capability (ex: comment can only be deleted by its owner)

2 types of authentication

- All routes are protected and you are using third-party login: then you check the user availability in App.tsx, user data is retrieved from AuthContext, if the user is not logged in then return to third-party login
- Not all routes are protected and you are implementing login yourself: create a ProtectedRoute component and check the user inside that component


## Layout Route

```jsx
const App = () => {
  return (
    <>
      ...

      <Routes>
        <Route element={<Layout />}>
          <Route path="home" element={<Home />} />
          <Route path="users" element={<Users />} />
        </Route>
      </Routes>
    </>
  );
};
```
In `Layout` component, we use `<Outlet />` instead of `children` to render *matched* child route

```jsx
const Layout = () => {
  return (
    <main style={{ padding: '1rem 0' }}>
      <Outlet />
    </main>
  );
};
```

## Protected Route

Example: <https://github.com/remix-run/react-router/tree/dev/examples/auth>

```jsx
const ProtectedRoute = ({ children }) => {
  const { token } = useAuth();

  if (!token) {
    return <Navigate to="/home" replace />;
  }

  return children;
};
```

If you have have a bunch of protected routes with the same authorization method, instead of return `children` like above, you return `<Outlet />` and group all the protected routes like this

```jsx
<Route element={<ProtectedRoute user={user} />}>
    <Route path="home" element={<Home />} />
    <Route path="dashboard" element={<Dashboard />} />
</Route>
```

So far, the protected routes only deal with authenticated user as authorization process. We need to extend it to handle permission and roles too. That means the condition for the routes need to be dynamic. Therefore, we need to pass an additional prop as a boolean to check that condition.

```jsx
const ProtectedRoute = ({
  isAllowed,
  redirectPath = '/landing',
  children,
}) => {
  if (!isAllowed) {
    return <Navigate to={redirectPath} replace />;
  }

  return children ? children : <Outlet />;
};
```

```jsx
const App = () => {
  ...
  <Route element={<ProtectedRoute isAllowed={!!user} />}>
    <Route path="home" element={<Home />} />
    <Route path="dashboard" element={<Dashboard />} />
  </Route>

  <Route
    path="analytics"
    element={
      <ProtectedRoute
        redirectPath="/home"
        isAllowed={
          !!user && user.permissions.includes('analyze')
        }
      >
        <Analytics />
      </ProtectedRoute>
    }
  />
```

## Index route

Index route is like the root route. In the top parent route, it's '/'. In the child route which nested other grand child route, it's the child route itself like this

```jsx
// In top parent route
<Route index element={<Home />} />

// In child route with nested grand child route
<Routes>
  <Route index element={<UserList users={users} />} />
  <Route path=":userId" element={<UserItem users={users} />} />
</Routes>
```

## Nested grand child routes

We use descendant routes

```jsx
// In top-level route
// trailing asterisk (*) indicates that this is the route itself and its decendant route
<Route path="users/*" element={<Users users={users} />} />

// In users route
<Routes>
  <Route index element={<UserList users={users} />} />
  <Route path=":userId" element={<UserItem users={users} />} />
</Routes>

```

## Active link style

```jsx
const style = ({ isActive }) => ({
    fontWeight: isActive ? 'bold' : 'normal',
});
<NavLink to="/home" style={style}>Home</NavLink>
```

## Relative path

Here is the component `User` for `user` route with 2 nested routes. We are using absolute path like 'user/profile` for the child route
```jsx
const User = () => {
  return (
    <>
      <h1>User</h1>

      <nav>
        <Link to="/user/profile">Profile</Link>
        <Link to="/user/account">Account</Link>
      </nav>
    </>
  );
};
```

React Router 6 support relative path, that means you can exclude the '/user/' part if the parent route is 'user'. Then, you can rewrite the child route like so

```jsx
<Link to="profile">Profile</Link>
<Link to="account">Account</Link>
```

## Redirect

### Redirect to other page if unauthenticated

In react router v6, `<Navigate />` component is support. Let's say, we need to redirect if authentication fails, just return `<Navigate to="/login/" />` component

```jsx
const ProtectedRoute = ({ children }) => {
  const { token } = useAuth();

  if (!token) {
    return <Navigate to="/home" replace />;
  }

  return children;
};
```

### Redirect to the previous pages if authenticated

If you open an application at a protected route, but you are not logged in, you get a redirect to the Login page. After the login, you will get a redirect to the desired protected route.

In order to do that, you have to remember the location from where the redirection happened. Here, we use `useLocation` to grab the current location before redirecting the user.

```jsx
const ProtectedRoute = ({ children }) => {
  const { token } = useAuth();
  const location = useLocation();

  if (!token) {
    return <Navigate to="/home" replace state={{ from: location }} />;
  }

  return children;
};
```

Then we grab the state of location on 'home' route. When a login happens, we can take that location of previous page to redirect the user to desired route.

```js
const navigate = useNavigate();
const location = useLocation();

const handleLogin = async () => {
  const token = await fakeAuth();

  setToken(token);

  const origin = location.state?.from?.pathname || '/dashboard';
  navigate(origin);
};
```

Utilizing `useNavigate` and `useLocation` like this, we can also pass data from page A to page B, ex: display message sent from page A on page B after doing asynchronous task from page A.

## Hooks

### `useNavigate`

Use to redirect
```js
const navigate = useNavigate()
navigate( '/' )
```

### `useParams`

```js
const { userId } = useParams();
```


### `useSearchParams`

The useSearchParams hook is used to read and modify the query string in the URL for the current location

```js
const [searchParams, setSearchParams] = useSearchParams();
const searchTerm = searchParams.get('name') || '';
```

### `useRouteMatch`

Often used in the children route to get the path of parent route