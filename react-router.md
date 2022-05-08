# React Router

## Example

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
          <Route path="/" element={<UsersIndex />} />
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

## Hooks

### `useNavigate`

Use to redirect
```js
const history = useNavigate()
history( '/' )
```

### `useParams`

Read URL parameters, return all the params as properties of an object
```js
const {id} = useParams();
```

### `useRouteMatch`

Often used in the children route to get the path of parent route