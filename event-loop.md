# Event loop

## References

- <https://www.youtube.com/watch?v=eiC58R16hb8>
- <https://www.lydiahallie.com/blog/promise-execution>
- <https://blog.xnim.me/event-loop-and-render-queue>
- <https://dmitripavlutin.com/javascript-promises-settimeout/>

## Synchronous and Asynchronous

- Synchronous:
Javascript is single-threaded programming languaage, it can only process one statement at a time.
Asume we make an action like requesting an API. This action can take an amount of time, depending on size of data, the speed of network...
If we make that kind of action in the synchronous model, the browser cannot do other things like scrolling or clicking the button. It's called blocking
- Asynchronous:
In order to prevent blocking behavior, the browser has many Web APIs that are asynchronous (setTimout, async/await), meaning they can run parallel with other operations. It allows the user to continue using browser while the asynchronous is being processed.

## Event loop

Javascript is single-threaded, it can only execute one statement at a time.

- First, all the statements in the script are pushed into call stack. The call stack works as LIFO, that means that the last statement that gets pushed into the stack is the first to be popped out when the function returns. When a statement is pushed in call stack, it is run immediately. If it's a function, the function remains in the call stack until all the statements in its body are removed from call stack.
- If call stack encounters asynchronous task like `setTimeout`:
 - add `setTimeout` to the stack
 - run `setTimeout`
 - add callback function to the **web API** with a timer. Web APIs provides apis to allow interact with browser features (like timer function, client-side storage, making http requests....) 
 - the timer runs for a period of time that equals to the second arguments then the callback functions is passed to the **macrotask** queue. So the delay is not the time the callback is pushed into the callstack, it's the time it's pushed into task queue
 - remove `setTimeout`
- If `fetch` exists, the operation of `fetch` is handled by Web API. It creates a promise object which by default is pending with `undefined` result. It also initiates a background network request that is handled by browser. When server returns some data, it passes that data to the callback in `then` and moves it to the **microtask** queue.
- If `promise` exists
  - The body of promise is run immediately, like we are calling a normal function 
  - when `promise` resolves by running `resolve` or `reject`, the callback we pass to `then` or `catch` or `finally()` is pushed to **microtask** queue. If the promise is not resolved or rejected => the promise will be in pending status forever and callback of then will never be run.
- If we execute `async` function, 
  - it runs like normal function, that means its body are push into call stack
  - If `await` exists inside `async`,
    - run the promise that we are awaiting 
    - the rest of function body is pushed to **microtask** queue, only if the function we are awaiting is resolved or rejected. In this example: `await asyncFunc(); console.log(2)`, `console.log(2)` is pushed to `microtask` queue

```ts
const wait = () =>
  new Promise(res => {
    console.log('asyncFn');
    setTimeout(() => console.log('123213'), 3000);
  });
const asyncFn = async () => {
  await wait();
  console.log('data', 'after await'); // This line is never called because the promise doesn't resolve anything. `wait` is always in pending state, then what is after `await wait()` will never be pushed into the microstask queue.
};
console.log('first');
asyncFn();
console.log('last');

```

```js
async async1() {
  console.log(1)
}

async1()

console.log('start')

new Promise((resolve, reject) => {
  console.log(2)
})

// Print 1 -> 'start' -> 2
```

- Then `event loop` to do its only task: **connect the queues to the call stack**.
- First it checks if the call stack is empty, then it adds the pending statement in the queues to call stack, starting from `mircotask` queue first. The statement in `mircotask` queue has higher priority than `macrotask` queue. If call stack and `microtask` queue is empty, then the tasks in macrotask queue are pushed into callstack

