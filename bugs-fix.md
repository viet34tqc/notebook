# Bugs Fix

This is some common bugs I encounter when developing

## Typescript

### Property '...' does not exist on type context '...'

When you create your context without default value like this:

```typescript
interface IAuthContext{
	user: 'string',
	error: 'string'
}

const AuthContext = React.createContext<IAuthContext|null>(null)
```

Then, you retrieve the variable's value from the context, TS will raise an error `Property  does not exist on type context IAuthContext|null`
```typescript
const {user, error} = useContext(AuthContext)
```

This can be fixed by casting the useContext:
```typescript
const {user, error} = useContext(AuthContext) as IAuthContext
```

## NextJS

### Displaying 'Unhandled Runtime error' screen

That means you've forgotten to catch your error when you make request. Please add `try..catch` block or `then().catch()`