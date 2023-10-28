# Websockets

- <https://dev.to/ibrahzizo360/unlocking-the-potential-of-web-sockets-in-modern-web-development-5ef>
- <https://www.youtube.com/watch?v=8ARodQ4Wlf4>

## Why use Websockets

To have persistent connection with the server like in games, chatting applications...

Before Websockets, we use a technique called polling. There are 2 types of polling, **short polling** and **long polling**

### Short polling

We will send periodic AJAX requests to the server, for example every 10 seconds.

Downsides: Server need is bombed with requests every 10 seconds, even if there is no messages => bad performance.

### Long polling

Send requests to the server and keep connection open ultil new data

The flow:

- A request is sent to the server.
- The server doesn't close the connection until it has a data to send.
- The browser makes a new request immediately when it receives new data.

If the connection is lost, because of, say, a network error, the browser immediately sends a new request.

## Advantage of Websockets

- WebSocket protocol uses persistent connections rather than a continuous HTTP request/response cycle. WebSockets require less bandwidth and provide lower latency compared to HTTP, reducing the load on both the client and the server.
- WebSocket is an event-driven technology, data is pushed as soon as it becomes available, without any need for polling
- WebSocket provides a full-duplex, bidirectional communication channel. This means once a Web Socket connection is established, both server and client can send low-latency messages at the same time (half-duplex means only one device can send message at a time)

## Websockets handshake

- Client send a HTTP GET request with `Upgrade` header to upgrade from HTTP 1.1 to websockets
- Server responses with `101 switching protocol` status to let the client know that server is switching protocol

## Websockets code flow

- First, server set up a listener and wait for the client to start the connection

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


