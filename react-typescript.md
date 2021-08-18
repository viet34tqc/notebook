# React Typescript


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

## Component Hooks:

### useRef

Always to set the initial value for the useRef hook. Otherwise, there will be an error alert.
```javascript
const TextField: React.FC<InputProps> = ({handleChange}) => {
	const inputRef = useRef<HTMLInputElement | null>(null);
	const divRef = useRef(null);
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

	Lets look at this example

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

Context:
	Declare Context Provider Props
	Declare interface Context default
	Declare default Context data

	interface ProgressContextProviderProps {
	    children: ReactNode
	}
	interface ProgressContextDefault {
		lastTime: string
		status: string
	}
	const progressDefaultData = {
		lastTime: '30/5/2021',
		status: 'In Progress',
	};
	export const ProgressContext = createContext<ProgressContextDefault>(progressDefaultData);

	const ProgressContextProvider = ({ children }: ProgressContextProviderProps) => {

## Event
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
	const handleChange = (e: ChangeEvent<HTMLInputElement) => {
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