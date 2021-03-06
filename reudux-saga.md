# Redux Saga

Is one kind of the Redux Middleware 

## When to use

Use when you need to implement side effect(like: call API) or complex logic

## Terms

### Effects

- A plain javascript object contains intructions for saga middleware to perform some operation like: invoke asynchronous function, dispatch an action to the store, etc...

### Effects creators

- A function provided by redux-saga that return an Effect, like: `takeEvery`, `takeLatest`
- Doesn't perform any executions, the saga middleware executes all the Effects

```js
function* fetchProducts() {
  // We are not excuting the fetch immediately, instead, `call` creates a description of the effect.
  const products = yield call(Api.fetch, '/products')
  // ...
}
```


### Tasks

A task is like a process running in background. You create tasks by using the `fork` function

```js
function* saga() {
  const task = yield fork(otherSaga, ...args)
}
```

### How saga works

Whenever an action is dispatched, the `rootSaga` will be called. `rootSaga` is like the parent of child `saga`. So what's `saga`?

`saga` is a **generator function** that returns **Generator object**. The middleware iterates over `saga` (the generator function) and executes all yielded *Effects* (the result of Effect creator like: `put`, `takeLatest`...). The middleware reads each Effects description and performs the appropriate action (e.g., invoke some asynchronous function, dispatch an action to the store, etc.).

The `sagas` run only one when the action is dispatched the first time. However, effect creators can still wait for that action to run again (i.e: `takeEvery`)

## Notes

### Should we manage all the actions in Saga

No, only the action related to side-effect like: requesting APIs...

### Redux Saga doesn't support async function

## Basic steps

- Create saga middleware
- Add saga middleware to the list of store middlewares
- Saga middleware runs root saga. Root saga is a generator function

Whenever an action is distpatched, saga middleware runs the root saga. In the root saga, redux runs all the `yield` commands, which are the
*saga effects creators*

## Popular Effect Creators

<table>

  <tr>
    <td>takeEvery(pattern, saga, ...args)</td>
    <td>Ch???y saga m???i l???n khi c?? action pattern ???????c dispatch, dispatch bao nhi??u s??? ch???y b???y nhi??u c??i saga</td>
  </tr>
  <tr>
    <td>takeLatest(pattern, saga, ...args)</td>
    <td>Ch???y saga, nh??ng khi c?? action pattern m???i ???????c dispatch, th?? c??i saga tr?????c ???? s??? b??? cancel</td>
  </tr>
  <tr>
    <td>takeLeading(pattern, saga, ...args)</td>
    <td>Ch???y saga khi action pattern ???????c dispatch, nh???ng action ti???p theo s??? b??? cancel cho ?????n khi saga tr?????c ???? ch???y xong.</td>
  </tr>
  <tr>
    <td>put(action)</td>
    <td>Dispatch action t??? saga.</td>
  </tr>
  <tr>
    <td>call(fn, ...args)</td>
    <td>G???i h??m fn v?? truy???n tham s??? args v??o h??m ????</td>
  </tr>
  <tr>
    <td>all([...effects])</td>
    <td>Ch???y t???t c??? effects c??ng m???t l??c</td>
  </tr>
  <tr>
    <td>take(pattern) and fork(fn, ...args)</td>
    <td>M?? h??nh watcher ... worker, ?????i dispatch action pattern th?? s??? th???c hi???n m???t c??i task n??o ????</td>
  </tr>
  <tr>
    <td>throttle(ms, pattern, saga, ...args)</td>
    <td>Throttle c??i action pattern, ?????m b???o ch??? ch???y saga 1 l???n trong kho???ng th???i gian ms</td>
  </tr>
  <tr>
    <td>debounce(ms, pattern, saga, ...args)</td>
    <td>Debounce c??i action pattern, ?????m b???o ch??? ch???y saga 1 l???n sau khi ???? ?????i kho???ng th???i gian ms</td>
  </tr>
  <tr>
    <td>retry(maxTries, delay, fn, ...args)</td>
    <td>C??? g???ng g???i l???i h??m fn n???u faield, sau m???i delay milliseconds, v?? t???i ??a ch??? th??? maxTries l???n.</td>
  </tr>

</table>

**Tips**:

Run API or async function (for easier testing): using `call`
Run normal function: using `fork`, `call` (like event handler). `fork` is non-blocking and `call` is blocking
Dispatch action: `put`
Wait for action to be dispatched: `take`, `takeLatest`, `takeEvery`. Saga will take **action** object as paramater

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

`all(array_of_call)`

Run multiple `call` effect (APIs) (like `Promise.all`)
The APIs are run parallely (**Unlike `Promise.all`** )
*Blocking if there is a blocking effect creator in the array. If not, it's non-blocking*

## put

`put(actionCreator(payload))`

Dispatch an action

### takeEvery

`takeEvery(pattern, callback)`

Run the saga whenever an action is dispatched. For example: you click a button to dispatch an action. You click 3 times, the middleware runs 3 times

### takeLatest

`takeLatest(pattern, callback)`

Run the latest dispatched action. For example: you click a button to dispatch an action. You click 3 times, the middleware runs only the last action.

Example for login action:

- Create an login action and reducer in authSlice
- User click login button: user -> dispatch an action, payload is the credentials.
- redux saga wait for that action and pass that action into callback function
- In the callback, call API to get the user 

From an example above, you can clearly see that redux saga is the middleware that takes care of async function. The async function here could be an API to get the user from action payload