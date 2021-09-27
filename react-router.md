# React Router

## Core Components

```jsx
<BrowerRouter>
	<Link></Link>
	<NavLink></NavLink>
	<Switch>
		<Route>
		</Route>
	</Switch>
</BrowerRouter>
```

`NavLink`: is another version of `Link`. It add a classname 'active' (default) to the link if it matches the current URL

## Hooks

### `useHistory`

Use to redirect
	```js
	const history = `useHistory`()
	history.push( '/' )
	```
### `useParams`

Get params from url, return all the params as properties of an object
```js
const {id} = useParams();
```

### `useRouteMatch`

Often used in the children route to get the path of parent route