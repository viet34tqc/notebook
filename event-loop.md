Event loop
Synchronous and Asynchronous
	Synchronous:
		Javascript is single-threaded programming languaage, it can only process one statement at a time.
		Asume we make an action like requesting an API. This action can take an amount of time, depending on size of data, the speed of network...
		If we make that kind of action in the synchronous model, the browser cannot do other things like scrolling or clicking the button. It's called blocking
	Asynchronous:
		In order to prevent blocking behavior, the browser has many Web APIs that are asynchronous (setTimout, async/await), meaning they can run parallel with other operations. It allows the user to continue using browser while the asynchronous is being processed.
Event loop
	Javascript can only execute one statement at a time, and that statement is decided by event loop.
	In order to do that, Event loop uses
		- Call Stack
			- What: Holds the state of what function is currently running
			- How: 
				- The statement is added to the stack
				- statement run
				- remove that statement from the stack
				- Add another statement and so on...
				- If setTimeout exists:
					- add setTimeout to the stack
					- run setTimeout
					- add callback function to the **queue**
					- remove setTimeout
					- Run another statement
				- The event loop checks the **queue** for any pending statements, add statement to the stack and run
		- Queue:
			- When the call stack is empty, event loop checks the queue for pending statements, starting from oldest messages
			- Once it find ones, it add it to the stack
			- has two types:
				- message queue or task queue: handle setTimeout, lower priority
				- job queue or microtask queue: handles promises, higher priority

