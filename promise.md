## Promise:
An object that may return value when it resolved or return error when it reject

### How to create a promise
```javascript
  const promise = new Promise( (resolve, reject) => {})
```

### `then()`
  Access the data resolved from promise
- Inputs: callback function for success and failure cases of the promise (raraly use, often use catch)
- Output: another promise

### `catch()`
Handle a reject
- Inputs: error object
- Optput: another promise

### `finally()`
Called when everything above is done

## Async/Await

### Async
Put in front of function, then returns a promise

### Await
Put in front of promise, then return the resolving value.
