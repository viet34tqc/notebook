# React Tips

## Never update state during render phase

<https://alexsidorenko.com/blog/onclick-too-many-re-renders/>
It could lead to infinite loop. Update the state like this is the same as calling the callback function inside click handler

```jsx
<button onClick={setState(count + 1)}>Click</button> /*DONT do that*/
```

## Avoid using props as initial state of useState

- <https://sentry.io/answers/using-props-to-initialize-state/>
- <https://prateeksurana.me/blog/why-you-should-avoid-using-state-for-computed-properties/>

When we use props as initial state, that state won't react to props update. The reason is because `useState` called only once when the component mounts.

## Stop passing setter function to child component

<https://matanbobi.dev/posts/stop-passing-setter-functions-to-components>

```jsx
function Form() { 
    const [formData, setFormData] = useState({ name: '' }); 
    return (
        <Input name={formData.name} setFormData={setFormData} />
    ); 
};
```

Why: An abstraction leak occurs when a component knows too much about the internal implementation of another component. In this case, the Input component assumes:

- The parent component is using useState.
- The state contains a name field directly, alongside other data.
- The parent will always maintain the same state structure.

These assumptions make the child component tightly coupled to the parent, so any change in the parent's state structure or management mechanism requires updates to the child. And this lead to:

- Fragility: Changes to the parent's logic break the child component, creating a maintenance headache.
- Reduced Reusability: The child is tied to a specific parent implementation, limiting its use in other contexts.
- Loss of Clarity: Passing raw `setState` makes it 

Instead, define the setter function in the parent component

```jsx
// Form.jsx 
function Form() { 
    const [formData, setFormData] = useState({ name: '' }); 
    const handleNameChange = (name) => { 
        setFormData((prevState) => ({...prevState, name}));
    };
    
    return (
        <Input name={formData.name} onChange={handleNameChange} />
    ); 
};


// Input.jsx 
function Input({ name, onChange }) { 
    const handleInputChange = (event) => { 
        onChange(event.target.value);
    }; 
    
    return ( 
        <input type="text" value={name} onChange={handleInputChange} /> 
    ); 
};
```

## Conditionally swap component

<https://julesblom.com/writing/neat-react-tricks>

We use ternary operator on component name, not the components themselves for better code reading

```jsx
function UsersManagement({ users, isNewAPI }) {
  // ...

  // capitalized variable name!
  const UserPermissions = isNewAPI ? UserPermissions : NewUserPermissions;

  return (<UserPermissions
    permission={permission}
    users={users}
    currentUser={currentUser}
  />)
}
```

## Conditional custom hook

<https://www.youtube.com/watch?v=HLhn6dDu88I>

Use a flag variable to turn it on or off

<img src="https://i.imgur.com/o6ojSuL.png">

## Working with multiple states

Case 1: States are input state. However, this only works for jsx file because we don't know which type of event handler

<https://www.joshwcomeau.com/react/data-binding/>

```js
 const [formData, setFormData] = useState({
    firstName: "",
    lastName: "",
    email: "",
  });
 
  const onInputChange = (event) => {
    const { name, value } = event.target;
    setFormData({ ...formData, [name]: value });
  };
 
  return (
    <form>
      <label>
        First name:
        <input
          type="text"
          name="firstName"
          value={formData.firstName}
          onChange={onInputChange}
        />
      </label>
      ...
```

## Set empty string as initial value for input text and textarea

When the input is controlled, we should pass initial value as empty string rather than `undefined`. If value attribute is `undefined`, React will omit it, then the input becomes uncontrolled

## Use callback refs to interact with DOM nodes to avoid using `useEffect`

<https://tkdodo.eu/blog/avoiding-use-effect-with-callback-refs>

We often use callback Ref when we want to access the DOM Element or React Component at a specific point of time, such as when the component mounts or unmounts
This can be particularly useful when you're working with third-party libraries that require direct DOM access, or when you need to perform complex DOM manipulations, or when you need to <https://react.dev/learn/manipulating-the-dom-with-refs#how-to-manage-a-list-of-refs-using-a-ref-callback>[manage a list of refs].

Passing a ref from `useRef` (a RefObject) to a React element is just syntactic sugar for:

```js
<input
  ref={(node) => {
    ref.current = node;
  }}
  defaultValue="Hello world"
/>
```

```jsx
function MeasureExample() {
  const [height, setHeight] = React.useState(0)

  // we use `useCallback` to avoid creating ref every render
  const measuredRef = React.useCallback(node => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height)
    }
  }, [])

  // Other solution with useRef as long as there is no dependancy in the function inside `useRef`
  /*const measuredRef = React.useRef(node => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height)
    }
  })*/
  // Then <h1 ref={measuredRef.current}>Hello, world</h1>

  return (
    <>
      <h1 ref={measuredRef}>Hello, world</h1>
      <h2>The above header is {Math.round(height)}px tall</h2>
    </>
  )
}
```

In this example, we use only one hook instead of `useRef` and `useCallback`

## Pass params with `history.push` in react-router-dom

<strong>Passing parameters with history push:</strong>

```jsx
import { useHistory } from "react-router-dom";

const FirstPage = props => {
    let history = useHistory();

    const someEventHandler = event => {
       history.push({
           pathname: '/secondpage',
           search: '?query=abc',
           state: { detail: 'some_value' }
       });
    };

};
```

<strong>Accessing the passed parameter using useLocation from 'react-router-dom':</strong>

```jsx
import { useEffect } from "react";
import { useLocation } from "react-router-dom";

const SecondPage = props => {
    const location = useLocation();

    useEffect(() => {
       console.log(location.pathname); // result: '/secondpage'
       console.log(location.search); // result: '?query=abc'
       console.log(location.state.detail); // result: 'some_value'
    }, [location]);

};
```

## Using `react-portal` with `ref`

<https://jjenzz.com/avoid-global-state-colocate/>

When you want to portal a component to another react component, the destination component must be inside the dom first. Normally, we portal the component to body. The body is always in the DOM in advanced and we can access it via `document.body`. Here, we portal to a React component, so we need to trigger it first

```tsx
const SidebarPortal = ({ children }: PropsWithChildren) => {
  const [sidebar] = useContext(SidebarContext);
  return sidebar ? ReactDOM.createPortal(children, sidebar) : null;
};

const Sidebar = () => {
  const [, setSidebar] = useContext(SidebarContext);
  return (
    <aside
      ref={setSidebar}
      style={{
        background: 'pink',
        height: '100vh',
      }}
    ></aside>
  );
};

// Use case
<SidebarPortal>
  {/*RectangleEditForm is the component that we want to portal to the sidebar*/}
  <RectangleEditForm />
</SidebarPortal>
```

## Remove Contradicting State

<https://profy.dev/article/react-usestate-pitfalls>
This is the case when we have too much state too update and those states relates to each other, for example: loading state, error state and data state can come together

Solution: using `useReducer`, put all states in a wrapper state and update that big state.

## Escape hatch (using less dependancies)

### The latest ref

<https://tkdodo.eu/blog/refs-events-and-escape-hatches>

```jsx
export const useDebouncedState = (callback, delay) => {
  const [value, setValue] = React.useState('')
  // 👇 store callback in a ref
  const ref = React.useRef(callback)

  // 👇 update the ref when the callback changes
  React.useLayoutEffect(() => {
    ref.current = callback
  }, [callback])

  React.useEffect(() => {
    const timeoutId = setTimeout(() => {
      if (value) {
        // 👇 use the ref instead of the callback
        ref.current(value)
      }
    }, delay)

    return () => {
      clearTimeout(timeoutId)
    }
  // 👇 no need to include the callback
  }, [value, delay])

  return [value, setValue]
}
```
