# React Typescript

- <https://www.freecodecamp.org/news/typescript-for-react-developers/>
- <https://weser.io/blog/clean-react-with-typescript>

## Special type

<https://dev.to/itswillt/explaining-reacts-types-940>

- `React.ReactNode`: type for React component. You can set this type for `children`
- `ElementRef<'div'>`: returns type refs for HTMLElement.
- `JSX.IntrinsicElement<T>`: object type for HTML element. It is like Record<[name_of_HTML_elements], [default_props_of_elements]>
- `ElementType<T>`: is bigger than `JSX.IntrinsicElement`. It accepts React component
- `<Component />` has the type of `JSX.Element`
- Function component has the type of `() => JSX.Element`

## Component Props

You can declare Component props with an interface like this:

```typescript
interface InputProps {
	text: string; // this prop is required and its type is string
	fn?: () => string; // this prop with ? is optional and its type is a function that returns a string
	person?: Person; // You can also create another interface and set it as the type of the prop.
	handleChange?: React.ChangeEventHandler<HTMLInputElement> | undefined;
}
```
And use it like this:

```typescript
const TextField = ({ text, handleChange }: InputProps) => {}
```

## `PropsWithChildren`

```jsx

import React, { PropsWithChildren } from "react";

type ComponentProps = {
  someProperty: string;
};

// ✅
function Component({ someProperty, children }: PropsWithChildren<ComponentProps>) {
  // ...
}
```

## Extend a HTML element type

If you want to create a custom component that behaves like an `input`, you can use `React.ComponentProps`:

```tsx

type InputProps = React.ComponentProps<'input'>

const Input = React.forwardRef(
  (props: InputProps, ref: React.Ref<HTMLInputElement>) => {
    return <input {...props} ref={ref} />
  }
)
```

## Component Hooks:

### useRef

When working with HTML element, always set defaultValue as `null` instead of `undefined` because useRef doesn't accept `undefined` as default value 

```javascript
const TextField: React.FC<InputProps> = ({handleChange}) => {
	const inputRef = useRef<HTMLInputElement>(null);

	return (
		<div ref={divRef}>
			<input type="text" ref={inputRef} onChange={handleChange} />
		</div>
	);
};
```

### useState:

You can set the type of the state in the `<>` sign
```typescript
const [count, setCount] = useState<number | null>(null); // The type of the 'count' state is either number or null
```
If you use another type, like: `useState('5')`, it will raise an error.

### useReducer

```TS

import React, { useReducer } from 'react';

type Actions =
	| { type: 'add'; text: string }
	| {
			type: 'remove';
			idx: number;
	  };

interface Todo {
	text: string;
	complete: boolean;
}

type State = Todo[];

const TodoReducer = (state: State, action: Actions) => {
	switch (action.type) {
		case 'add':
			return [...state, { text: action.text, complete: false }];
		case 'remove':
			return state.filter((_, i) => action.idx !== i);
		default:
			return state;
	}
};

export const ReducerExample: React.FC = () => {
	const [todos, dispatch] = useReducer(TodoReducer, []);

	return (
		<div>
			{JSON.stringify(todos)}
			<button
				onClick={() => {
					dispatch({ type: 'add', text: '...' });
				}}
			>
				+
			</button>
		</div>
	);
};

```

## `fowardRef`

```tsx
export const ToolbarButton = forwardRef<HTMLButtonElement, Props>(
  (props, ref): JSX.Element => {})
```

## Event

You can use DOM event handler types or `ComponentProps`
Let's say I want to use the `onChange` method of input and take it as a prop

```typescript
const TextField = ({handleChange}: InputProps) => {
	const [count, setCount] = useState<number | null>(null);
	const inputRef = useRef(null);
	return (
		<div>
			<input type="text" ref={inputRef} onChange={handleChange} />
		</div>
	);
};
```
The type of `handleChange` needs match up with React's type definitions. In our case, we need to use React's `ChangeEvent`
```typescript

function App() {
	const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
		console.log(e);
	};

	// You can just hover over the handleChange attribute and assign the type as the returned type of event handler
	const handleChange: ChangeEventHandler<HTMLInputElement> = (e) => {
		console.log(e);
	};

	return (
		<div className="App">
			<TextField text="Hello" handleChange={handleChange} />
		</div>
	);
}
```

If there is no matching type definition, you can use `SyntheticEvent`

## Extract Component props

```ts
type Extraction = React.ComponentProps<typeof ComponentToExtract>
```

## Form Submit events

```ts
const handleLogin = (event: FormEvent<HTMLFormElement>) => {
	event.preventDefault();
	const formData = new FormData(event.currentTarget);
	const username = formData.get('username');
	const password = formData.get('password');
};
```

## Context

<https://kentcdodds.com/blog/how-to-use-react-context-effectively>
NextJS DJ Events
