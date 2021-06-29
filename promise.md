Promise:
	what:
		- An object that may return value when it resolved or return error when it reject
	How to create a promise
		const promise = new Promise( (resolve, reject) => {})
	then method: handle a resolve 
		- Inputs: callback function for success and failure cases of the promise (raraly use, often use catch)
		- Output: another promise
	catch method: handle a reject
		- Inputs: error object
		- Optput: another promise
	finally: called when everything above is done

Async/Await
	- async function:
		Output: when run, it return a promise
	- what await return
		- await is put in front of promise, then return the resolving value.