# Websockets

## Why use Websockets

To have persistent connection with the server. Before Websockets, we use a technique called polling. There are 2 types of polling, **short polling** and **long polling**

### Short polling

We will send periodic requests to the server, for example every 10 seconds.

Downsides: Server need is bombed with requests every 10 seconds, even if there is no messages => bad performance.

### Long polling

The flow:

- A request is sent to the server.
- The server doesn't close the connection until it has a data to send.
- The browser makes a new request immediately.

If the connection is lost, because of, say, a network error, the browser immediately sends a new request.

## Websockets flow

- First, server creates a hook and wait for the client to start the connection

```js
ws.on('connection', (ws) => console.log('connection start')) // Each connection will have a `ws` instance
```

- Client sends request to start the connection

```js
ws.addEventListener('open', () => console.log('Connected to server') )
```

- Server waits for client to send messages

```js
ws.on('connection', (ws) => {
	console.log('connection start')
	ws.on('message', data => console.log('Client has sent us: ' + data))
}) 
```

- Client sends message to server via Websockets

```js
ws.addEventListener('open', () => {
	console.log('Connected to server')
	ws.send('Message from client')
} )
```

- Now server want to send message to client

```js
ws.on('connection', (ws) => {
	console.log('connection start')
	ws.on('message', data => console.log('Client has sent us: ' + data))

	// Send message to the client
	ws.send('Message from server');
}) 
```

- Client listen to message from server

```js
ws.addEventListener('message', (e) => console.log('Server has sent us: ' + e.data))
```


