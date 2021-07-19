#How to use useCallback hook

## Referential equality
If you are new to Javascript, it wont take long before you learn why this is the case:
```javascript
true === true // true
false === false // true
1 === 1 // true
'a' === 'a' // true
{} === {} // false
[] === [] // false
() => {} === () => {} // false
const z = {}
z === z // true
```
So, when you define an object in a React function component, it is not going to be referentially equal to the last time that same object was defined (even if it has all the same properties with all the same values).

## Why we need to use useCallback
When we are in need of handling an event, we often create another function inside React funciton components. Let's say we need to handle a click event:
```javascript
function MyComponent() {
  // handleClick is re-created on each render
  const handleClick = () => {
    console.log('Clicked!');
  };

  // ...
}
```
handleClick is a different function on every rendering of MyComponent.

Normally, the creation of this function on each rendering is not a problem. However, in some cases, you need to maintain that function between renderings:
- When the function is a dependancy of other hook, e.g `useEffect(..., [callback])`
- A function component wrapped inside `React.memo()` acceps a function as prop (Please check out the example below)

That's when the `useCallback(callbackFun, [deps]` is helpful, given the same dependency values `deps`, the hook returns (memorizes) the function ***instance*** between rendering

## Good use case

Imagine you have two components. The first is a child component that render a big list of items:
```javascript
function BigList({term, onItemClick}) {
	const items = getFromAPI(term)
	const renderedItem = items.map( item => <div onClick={onItemClick}>{item}</div>)
}
export default React.memo(MyBigList);
```
The list could be big, maybe hundred of items. To prevent useless list rendering, you wrap it into React.memo
The `term` and `onItemClick` prop is passed from its parent components:
```javascript
import {useCallback} from 'react'
function Parent() {
	const onItemClick = useCallback( (event) => {
		console.log( 'You clicked', event.currentTarget)
	}, [term])

	return (
		<BigList
			onItemClick={onItemClick}
			term={term}
		/>
	)
}
```
onItemClick is memorized by `useCallback`. As long as the `term` is the same, `useCallback` returns the same function
When Parent component re-renders, `onItemClick` remains the same and don't break the memoization of `BigList`

There is another example you can consider here:
```javascript
const CountButton = React.memo(function CountButton({onClick, count}) {
  return <button onClick={onClick}>{count}</button>
})
function DualCounter() {
  const [count1, setCount1] = React.useState(0)
  const increment1 = React.useCallback(() => setCount1(c => c + 1), [])
  const [count2, setCount2] = React.useState(0)
  const increment2 = React.useCallback(() => setCount2(c => c + 1), [])
  return (
    <>
      <CountButton count={count1} onClick={increment1} />
      <CountButton count={count2} onClick={increment2} />
    </>
  )
}
```

## Bad use case
Let's look at another example
```javascript
import { useCallback } from 'react';

function MyComponent() {
  // Contrived use of `useCallback()`
  const handleClick = useCallback(() => {
    // handle the click event
  }, []);

  return <MyChild onClick={handleClick} />;
}

function MyChild ({ onClick }) {
  return <button onClick={onClick}>I am a child</button>;
}
```

In this case, `MyChild` component is light and its re-rendering doesn't create performance issue. By using `useCallback` here, you increase code complexity:
- You have to keep the deps of useCallback(..., deps) in sync with what you’re using inside the memoized callback.
- You have to allocate additional memory for the dep, as well as calling the `useCallback` function

## Summary
Any optimization add complexity. Any optimization added too early is a risk because the optimized code may change many times. The appropriate use case of `useCallback` is to memorize the callback functions that are supplied to memoized heavy child component.

Last words, MOST OF THE TIME YOU SHOULD NOT BOTHER OPTIMIZING UNNECESSARY RERENDERS.

## Reference
<https://kentcdodds.com/blog/usememo-and-usecallback>