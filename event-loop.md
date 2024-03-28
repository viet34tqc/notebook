# Event loop

## References

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

Javascript can only execute one statement at a time. In order to do that, Javascript uses
	- Call Stack
		- What: is the place where statements run
		- How:
			- The statement is added to the stack
			- statement run
			- remove that statement from the stack
			- Add another statement and so on...
			- If `setTimeout` or `setInterval` exists:
				- add `setTimeout` or `setInterval` to the stack
				- run `setTimeout` or `setInterval`
				- add callback function to the **web API**
				- the timer runs for a period of time that equals to the second arguments then the callback functions is passed to the **task queue**.
				- remove `setTimeout`
				- Run other statements until the end of the script
			- If promise exists, when promise resolves by running `resolve` or `reject`, the callback we pass to `then` or `catch` is pushed to **microtask** queue
	- This is the time for the `event loop` to do its only task: **connect the queues to the call stack**.
	- First it checks if the call stack is empty, then it adds the pending statement in the queues to call stack, starting from `mircotask` queue first. The statement in `mircotask` queue has higher priority than `macrotask` queue

