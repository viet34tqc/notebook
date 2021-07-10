React Typescript

- Setup: Use create-react-app with --template typescript
- Component Props
You can declare Component props with an interface like this:
interface Person {
	firstName: string;
	lastName: string;
}
interface InputProps {
	text: string; // this prop is required and its type is string
	fn?: () => string; // this prop with ? is optional and its type is a function that returns a string
	person?: Person; // You can also create another interface and set it as the type of the prop.
	handleChange?: React.ChangeEventHandler<HTMLInputElement> | undefined;
}
And use it in two ways
const TextField: React.FC<InputProps> = () => {}
or 
const TextField = ({ handleChange }: InputProps) => {}

Component Hooks:
- useRef: always to set the initial value for the useRef hook
	const TextField: React.FC<InputProps> = ({handleChange}) => {
		const inputRef = useRef<HTMLInputElement | null>(null);
		const divRef = useRef(null);
		return (
			<div ref={divRef}>
				<input type="text" ref={inputRef} onChange={handleChange} />
			</div>
		);
	};
	Otherwise, there will be an error alert:

- useState:
	You can set the type of the state in the '<>' sign, '|' means or.
	const [count, setCount] = useState<number | null>(null); // The type of the 'count' state is either number or null

	If you use another type, like: useState('5'), it will raise an error.

Let's say I want to use the onChange method of input and take it as a prop
const TextField: React.FC<InputProps> = ({handleChange}) => {
	const [count, setCount] = useState<number | null>(null);
	const inputRef = useRef(null);
	const divRef = useRef(null);
	return (
		<div ref={divRef}>
			<input type="text" ref={inputRef} onChange={handleChange} />
		</div>
	);
};
And add that handleChange prop:
interface InputProps {
	text: string;
	fn?: () => string;
	person?: Person;
	handleChange?: () => void;
}

It's ok. However, when you declare handleChange in the parent component, and using the event object, there will be a problem.

function App() {
	const handleChange = (e): void => {
		console.log(e);
	};

	return (
		<div className="App">
			<TextField text="Hello" handleChange={handleChange} />
			<ReducerExample />
		</div>
	);
}

That's because we have declared the wrong type for the handleChange prop in TextField. Let's get back to the onChange in the TextField component. Hover on the text 'onChange', and you will get the full definition of the onChange's handle:

Look for the first colon, you will see that the type of the onChange is: . Copy it and paste into the handleChange's type declaration and into the parent component. The error message will be gone.

useReducer

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

