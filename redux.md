# Redux

## Core terms

Below are the terms that describe the structure of Redux.

### Store

A global object that store the state
Created by using function createStore()

- Input: reducer
- Output: state object that has 3 API
  - store.dispatch: dispatch an action
  - store.subscribe
  - store.getState

### Action

An object that has two properties

- Type
- Payload

You can think of an action as an event that describes something.

### Action creator

What: Function that return an action object

```javascript
const increment = () => {
	return { type: 'counter/increment' };
};
```

Why:

- Writting action object is repetitive, tedious and error-prone.
- Useful when dispatch the same action in multiple place

### Dispatch

- Store API. The only way to update the state is to call `store.dispatch()`.
  - Input: an action object
  - Output: void
- Store dispatches an action to return the new state.
- You can think of dispatching actions as "triggering event": `store.dispatch( increment() )`

### Reducer

A function that calculates new state based on previouse state and action

- Input: global state object and action
- Ouput: new state
- Signature: `(state, action) => newState`

You can think of an reducer as an event listener which handles an event based on the received action (event) type.
Reducer must not do any asynchronous logic, cause other side effects. Side effects is any changes that is outside the functions:

- Making AJAX HTTP request
- Generating random number or unique random IDs
- Logging a value to console
  Reducer must not mutate the state, only copy the current state then makes changes to that copy.
  Reducer must not calculate random values, because it makes the results unpredictable

**Tips**
Normally, we have reducers for fetching data, fetching data successfully, fetching data fail. If there is only one data state to update, we can add that data update to the fetching data success. Otherwise, we will have a seperate reducer for updating the data state.

## Redux application data flow

- Initial setup:
  - A Redux store is created using a **root** reducer function.
  - The store calls the root reducer once, saves the return value as its **initial state**
  - All the UI components that has access to the current state of the store use that data to render
- Updates:
  - Some events happens, like user clicking a button
  - Redux dispatches an action to the store
  - Store run the reducer function again with the **previous state** and the **current aciton**, and saves the
    return value as the new state
  - Store notifies all the components that are subscribed about the update
  - Each components check if **the part of the state they need have changed**, if so it re-render to update the UI.

## Redux toolkit

### Slice

A slice is a collection of Redux reducer and actions for a single features

### `configureStore`

Function to create store

- Input: an object that has properties. One of them is reducer.
  - reducer: object(**root reducer**) that take feature name as properties
    feature_1_name: reducer_of_feature_1
    feature_2_name: reducer_of_feature_2
- Output: store that is global state object

```JS
const store = configureStore({ reducer: { counter: counterReducer } });
```

Here we have a **counter** property of state object and **couterReducer** function to be in charge of that property.

### `createSlice`

A function that automatically generate action creators and action types that correspond to the reducers and state. Internally, it uses `createAction` and `createReducer`.

- Input: object
  - `name`: name of the slice
  - `initialState`: initial state object
  - `reducers`: object that includes reducers functions.
    `key: (state, action) => newState`
- `name` and `key` will be used to generate string action type constant: `name/key`
- `todoSlice.actions[key]` is the name an action creator that returns action object: `{type: "name/key"}`
  - If we pass payload as the argument, it will return as `{type: "name/key", payload }`
  - This action creator has a 'type' property. You can get the type directly without invoking it.

### Prepare callback

You can write your reducer function like this:

```js
reducer_function: {
 reducer(state, action) { return state },
 prepare( args ) { return { payload: {args} } }
}
```

Why:

- Do some 'preparation' logic like generating unique ID
- pass in multiple parameters
  How:
- call the action creator (`reducer_function` above)
- prepare function called with whatever parameters were passed in and return the payload
- The payload is passed in the action in the 'reducer' function.

### Flow

- Create a Redux store with `configureStore`
  `configureStore` accepts a reducer function as a named argument
  `configureStore` automatically sets up the store with good default settings
- Provide the Redux store to the React application components
  - Put a React-Redux `<Provider>` component around your `<App />`
  - Pass the Redux store as `<Provider store={store}>`
  - Create a Redux "slice" reducer with createSlice
- Call `createSlice` with a string name, an initial state, and named reducer functions
  - Reducer functions may "mutate" the state using Immer
  - Export default the generated slice reducer and export action creators
- Use the React-Redux `useSelector` and `useDispatch` hooks in React components
  - Read data from the store with the `useSelector` hook
  - Get the dispatch function with the `useDispatch` hook, and dispatch actions as needed

## Redux Middleware

What:

- The middle thing between UI (dispatch action) and the store (receive action)
- Connects to API, receive data then dispatch the real action to the store
  `UI => dispatch() => middleware => dispatch(storeAction) => run the reducer`
  Why:
  Reducers are supposed to be pure. They don't change anything outside its scope, or do any API calls. If you want to work with any APIs, you will need a middleware.
  Where:
- Normal flow:

1. An event occurs
2. Dispatch an action
3. Reducer creates a new state from the change prescribed by the action
4. New state is passed into the React app via props

- With middleware:

1. An event occurs
2. Dispatch an action
3. **Redux notifies all the middleware that there is a new action.**
4. **Middlewares receives the action, do something side-effect like setTimeout or other async logic, then dispatch another action**
5. Reducer creates a new state from the change prescribed by the action
6. New state is passed into the React app via props

### Thunk

What:

- A function that wraps an expression to delay its evaluation

```javascript
let x = 1 + 2; // x is calculated and return immediately.
let foo = () => 1 + 2; // foo delayed the calculation, foo is a thunk.
```

- Not normal function, is a function that returned by another function

### Redux thunk

- A Redux Middleware that looks at every actions, if it's a function, call that function.
- Delay the dispatch of an action of dispatch only if a certain condition is met
- Action creators now return a **function** instead of an object. That **Inner function** is a thunk:
  - Input:
    - dispatch: to dispatch the new action if they need
    - getState: to access the current state.
  - Output: void.

`createAsyncThunk`
When we make an API call, its progress can be in one of 4 states:

- idle: The request hasn't started yet
- loading: The request is in progress
- succeeded: The request succeeded, and we now have the data we need
- failed: The request failed, and there's probably an error message

`createAsyncThunk` will automatically creates action types and action creators for the 'loading/succeeded/failed' status.
Then it automatically dispatchs those actions relatively based on the resulting Promise. It means, after dispatching 'loading', it continues dispatching 'succeeded' ( fullfilled ) status if success, or 'failed' (rejected) if failure

Input:

- prefix for aciton types: 'posts/fetchPosts' will be prefix of action types: 'posts/fetchPosts/pending'
- payload creator: a function that make API calls
  - Input: the params that passed in the action creator when we dispatch it:
    `dispatch(addTodo(text)) // text will be the param of payload creator`
  - Output: Promise with data or Promise with Error (async always return Promise)

Output: action creators and their action types:

- fetchPosts.pending: todos/fetchPosts/pending
- fetchPosts.fulfilled: todos/fetchPosts/fulfilled
- fetchPosts.rejected: todos/fetchPosts/rejected

If success, createAsyncThunk will extract data from API response and pass that data returned from the payload creator into payload

When we dispatch an asynchronous action that created by createAsyncThunk above

- Run async_action_name.pending reducer
- If success, run async_action_name.fullfilled reducer
- Else if failure, run async_action_name.reject reducer

These reducers are extraReducers:

```JS
[ async_action_name.pending ]: ( state, action ) => {
state.status = 'loading';
},
[ async_action_name.fullfilled ]: ( state, action ) => {
state.status = 'succeeded';
state.posts = state.posts.concat( action.payload );
},
```

`createSelector`

Get modified data from other selector

Selector:

- What: is any function that accepts the Redux store state (or part of the state) as an argument, and returns data that is based on that state.
- Name convention: prefix with **select** word, then combine with the description of value being selected: selectTodoById, selectFilteredTodos
- Where: use as `useSelector` argument: `const todos = useSelector( selectFilteredTodos )`

```js
// Input selectors (selector: the function passed to useSelector)
const selectItems = (state) => state.items;
const selectItemId = (state, itemId) => itemId;
// Output selector will take the result from **all** the input selectors as arguments
// And return final result value
// The final result will be cached for the next time
const selectItemById = createSelector(
	[selectItems, selectItemId],
	(items, itemId) => items[itemId]
);

//Then this selector is passed into useSelector
const itemIds = useSelector(selectItemById);
```

Why:
Normal selector like: `state => state.todos` always return the new reference. As a result, the component that uses the `useSelector` to be re-rendered

Notes:
`createSelector` only helpful when derive additional values from original data. If you are just looking up or returning existing data, you can keep the selector as plain function

## React Redux hooks

- `useDispatch`: dispatch an action

Input: global state
Output: return dispatch function

- `useSelector`: read data from store
  Input: a **selector function**: (state) => value_from_state
  - Input: store state
  - Output: value based on the state.
    Ouput: the returned value from the selector above

`useSelector` subscribes to the store, and re-runs the selector after **every** action is dispatched
If the value returned by the selector changes from the last time it runs, useSelector will force the component to re-render with the new data

**Note**: if the value returned by the selector is a new reference, the component is re-render too

```js
// Bad: always returning a new reference
const selectTodoDescriptions = (state) => {
	// This creates a new array reference!
	return state.todos.map((todo) => todo.text);
};
```
