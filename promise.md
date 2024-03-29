# Promise

<https://www.builder.io/blog/promises>
<https://www.youtube.com/watch?v=qW3raOCefms>
  
- A promise is an object that encapsulates the result of an asynchronous operation.
- Has 3 states:
  - pending: initial state, neither fulfilled nor rejected.
  - fulfilled: meaning the asynchronous operation was completed successfully
  - rejected: meaning the asynchronous operation was failed.

## How to create a promise

**NOTE**: when a promise is created, the callback in promise is executed immediately

```javascript
const promise = new Promise( (resolve, reject) => {
  // Any code inside here will be excuted immediately
})
```

So, in case you have an API and you don't want to call it right when the promise is created, just wrap it in a function

```javascript
const creatPromise = () => new Promise( (resolve, reject) => {
  // Any code inside here will be excuted immediately
})
```

## `Promise.resolve(value)` and `Promise.reject(value)`

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

## `then()`

Access the fulfilled value from promise
- Inputs: callback function for success and failure cases of the promise (raraly use, often use catch)
- Output: another promise

If we put a promise inside then, it will do nothing even if that promise is resolved

```js
const doSomething = () => Promise.resolve('hello')
const doSomethingElse = () => Promise.resolve('world')

doSomething().then(doSomethingElse()).then((c) => {console.log('c', c)}) // c return hello
```

If the callback returns a resolved promise instead of a value, the next then will take that resolved value as the input

```js
doSomething().then(doSomethingElse).then((d) => {console.log('d', d)}) // d is world
```

## `catch()`

Access the rejection error from promise
- Inputs: error object
- Optput: another promise

## `finally()`

Called when everything above is done

## `Promise.all` and `Promise.race`

Both take an array of promises
`Promise.all` return a single Promise that resolves an array of result of input promises. If one of the input promises fails, it will jump to the `catch` block
`Promise.all` doesn't run the input promises in parallel. It depends on your CPU.

`Promise.race` return the first resolved or reject promise

## Async/Await

## Async

**Syntax**:

```javascript
async function() {
};
```

Put in front of function, then **returns a promise**. This promise will be resolved with the **value** returned by the async function or rejected with an error that is thrown inside async function

If the **value** is not a promise, it will be wrapped in promise.

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

If you do return a promise for a value from within an async function, you will receive a promise for the value (not a promise for a promise for the value).

<https://www.youtube.com/watch?v=zT0k4w_K4uM>

```js
function newPromise() {
  return Promise.resolve(2);
}

async function secondPromise() {
  // It doesn't matter whether you put an `await` here.
  return newPromise();
}

async function run() {
  console.log(secondPromise()); // secondPromise returns Promise<Promise<number>> that is flatted into Promise<number> 
  const rs = await secondPromise();
}
```

## Await

Can only be used inside `async` function.
**Syntax**:

```javascript
const value = await expression;
```

`expression` is a `Promise` or any value. If a value, it convert to `Promise.resolve(expression)`, which is a `Promise`;

`Await` pause the `async` function ultil a `Promise` is fulfilled or rejected.

- If rejected, `await` throw an error.
- If fulfilled, it resumes the `async` function execution and **return the value of the fulfilled Promise**

You can catch the error right after await, without using `try...catch`

```js
const value = await expression.catch((e) => {})
```

## Loop with promises

Using `async/await`

```js
const promise1 = new Promise(res => {
  setTimeout(res, 1000, 'first');
});
const promise2 = new Promise(res => {
  setTimeout(res, 2000, 'second');
});

(async () => {
  for (const item of [promise1, promise2]) {
    console.log(await item);
  }

  // Or
  for await (const item of [promise1, promise2]) {
    console.log(item);
  }
})();
```

## Handle concurrent promises

[https://www.builder.io/blog/promises](https://www.builder.io/blog/promises)

Use `Promise.allSettled()`

```jsx
// First create a utility funtion
function handleResults(results) {
  const errors = results
    .filter(result => result.status === 'rejected')
    .map(result => result.reason)

  if (errors.length) {
    // Aggregate all errors into one
    throw new AggregateError(errors)
  }

  return results.map(result => result.value)
}

// Usage

async function getPageData() {
  const results = await Promise.allSettled([
    fetchUser(), fetchProduct()
  ])

  try {
    const [user, product] = handleResults(results)
  } catch (err) {
    for (const error of err.errors) {
      handle(error)
    }
  }
}
```

Or we await separately after instantiation

```js
async function getPageData() {
  // We fire all the promises first, so they can run both in the same time, instead of waiting for each other
  const userPromise = fetchUser().catch(onReject)
  const productPromise = fetchProduct().catch(onReject)

  // Try/catch each
  try {
    const user = await userPromise
  } catch (err) {
    handle(err)
  }
  try {
    const product = await productPromise
  } catch (err) {
    handle(err)
  }
}
```
