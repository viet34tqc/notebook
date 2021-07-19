React Router:

Core Components
BrowerRouter
	<Link></Link>
	<NavLink></NavLink>
	<Switch>
		<Route>
		</Route>
	</Switch>

`NavLink`: is another version of `Link`. It add a classname 'active' (default) to the link if it matches the current URL

Hooks
	useHistory: 
		const history = useHistory()
		- Redirect
			history.push( '/' )