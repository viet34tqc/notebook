# Redux Saga

Is one of the Redux Middleware

## Terms

### Effects

- A plain javascript object contains intructions for saga middleware

### Effects creators

### Tasks

- A function provided by redux-saga that create Effects, like: `takeEvery`, `takeLatest`
- Doesn't perform any executions, the saga middleware executes all the Effects

### Saga

This is a **generator function** that returns **Generator object**. The middleware iterates over the Generator objects and execute all yielded *Effects*. The middleware examines each Effects description and performs the appropriate action

## Notes

### Should we manage all the actions in Saga
No, only the action related to side-effect like: requesting APIs...

### Redux Sage doesn't support async function

## Basic steps

- Create saga middleware
- Add saga middleware to the list of store middlewares
- Saga middleware runs root saga. Root saga is a generator function

Whenever an action is distpatched, saga middleware runs the root saga. In the root saga, redux runs all the `yield` commands, which are the
*saga effects creator*

## Popular Effect Creators

Tips:

Run API: using `call`
Run normal function: using `fork`, `call`
Dispatch action: `put`
Wait for action to be dispatched: `take`, `takeLatest`, `takeEvery`

## take

`take(action.type)`

- Run the saga when a specified action is dispatched.
- Returns the action object being dispatched
- Non-blocking

## fork

`fork( fn )`

- Run a function, which is generator function or function that returns a Promise
- Non-blocking call

## call

`call(fn, args)`

- Run a function, which is generator function or function that returns a Promise
- Behave Like `await`. **DONT USE `ASYNC` `AWAIT` IN SAGA** because it isn't supported.
- Blocking call

## all

`all(array_of_call)

Run multiple `call` effect (APIs) (like `Promise.all`)
The APIs are run parallely (**Unlike `Promise.all`** )
*Blocking if there is a blocking effect creator in the array. If not, it's non-blocking*

## put

`put(actionCreator(payload))`

Dispatch an action

### takeEvery

Run the saga whenever an action is dispatched. For example: you click a button to dispatch an action. You click 3 times, the middleware runs 3 times

### takeLatest

Run the latest dispatched action. For example: you click a button to dispatch an action. You click 3 times, the middleware runs only the last action.