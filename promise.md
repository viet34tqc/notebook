## Promise:

- A promise is an object that encapsulates the result of an asynchronous operation.
- Has 3 states:
  - pending: initial state, neither fulfilled nor rejected.
  - fulfilled: meaning the asynchronous operation was completed successfully
  - rejected: meaning the asynchronous operation was failed.

### How to create a promise

```javascript
  const promise = new Promise( (resolve, reject) => {})
```

### `Promise.resolve(value)` and `Promise.reject(value)`

Both returns a promise. `value` can be a promise or a non-promise.
`Promise.resolved()` returns a promise with resolved value. If value is a promise, it will be the promise with the resolved value of the passed promise.
`Promise.reject()` retuns a promise with reject value.

```javascript
Promise.resolve('Success').then(function(value) {
  console.log(value); // "Success"
}, function(value) {
  // not called
});
```

```javascript
Promise.rejected('Failed').then(function(value) {
  console.log(value); // not called
}, function(value) {
  console.log(value); // "Failed"
});
```

### `then()`

Access the fulfilled value from promise
- Inputs: callback function for success and failure cases of the promise (raraly use, often use catch)
- Output: another promise

If we put a promise inside then, it will do nothing even if that promise is resolved

```js
const doSomething = () => Promise.resolve('hello')
const doSomethingElse = () => Promise.resolve('world')

doSomething().then(doSomethingElse()).then((c) => {console.log('c', c)}) // c return hello
```

If we the callback return a resolved promise instead of a value, the next then will take that resolved value as the input

```js
doSomething().then(doSomethingElse).then((d) => {console.log('d', d)}) // d is world
```

### `catch()`

Access the rejection error from promise
- Inputs: error object
- Optput: another promise

### `finally()`
Called when everything above is done

### `Promise.all` and `Promise.race`

Both take an array of promises
`Promise.all` return a single Promise that resolves an array of result of input promises. If one of the input promises fails, it will jump to the `catch` block
`Promise.all` doesn't run the input promises in parallel. It depends on your CPU.

`Promise.race` return the first resolved or reject promise

## Async/Await

### Async

**Syntax**:
```javascript
async function() {
};
```
Put in front of function, then **returns a promise**. This promise will be resolved with the **value** returned by the async function or rejected with an error that is thrown inside async function

If the **value** is not a promise, it will be converted to a promise.
```javascript
async function foo() {
   return 1
}
```
...is similar to:
```javascript
function foo() {
   return Promise.resolve(1)
}
```

### Await

Can only be used inside `async` function.
**Syntax**:
```javascript
const value = await expression;
```
`expression` is a `Promise` or any value. If a value, it convert to `Promise.resolve(expression)`, which is a `Promise`;

`Await` pause the `async` function ultil a `Promise` is fulfilled or rejected.
- If rejected, `await` throw an error.
- If fulfilled, it resumes the `async` function execution and **return the value of the fulfilled Promise**

