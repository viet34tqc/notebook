# Event loop

## References

<https://dmitripavlutin.com/javascript-promises-settimeout/>

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
		- What: Holds the state of what function is currently running
		- How:
			- The statement is added to the stack
			- statement run
			- remove that statement from the stack
			- Add another statement and so on...
			- If `setTimeout` exists:
				- add `setTimeout` to the stack
				- run `setTimeout`
				- add callback function to the **web API**
				- remove `setTimeout`
				- Run other statements until the end of the script
	- Web API:
		- What: is the place that the async operations (promises, `setTimeout`, `setInterval`) callbacks are waiting to complete.
		- How:
		  - For `setTimeout` and `setInterval` callbacks, the timer runs for a period of time that equals to the second arguments then the callback functions is passed to the **queue**.
		  - Promises callbacks are passed immediately to the **queue**.
  		  - This is the time for the event loop to do its only task: **connect the queue with the call stack**.
	- Queue:
		- FIFO
		- When the call stack is empty, event loop checks the queue for pending statements, starting from oldest messages
		- Once it find ones, it adds it to the stack
		- Queue has two types:
			- message queue or task queue or **macrotask queue**: handle `setTimeout`, `setInterval`, lower priority
			- job queue or **microtask queue**: handles promises callbacks (`then` callbacks), higher priority

