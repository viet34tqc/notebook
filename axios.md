# Axios

## What is Axios
Axios is a HTTP client library that allows you to make requests to given endpoint.

## Why use Axios

Compare to native fetch API, Axios has some advantages:
- You don't need to convert your request body to JSON string like Fetch API, that saves you one `then()` to access your requested data.
```javascript

```
- Axios has function name that matches any HTTP method, example: to perform a GET request, you use the `.get()` method
- Axios has better error handling. Unlike the Fetch API, where you have to check the status code and throw the error yourself.

## CRUD

`GET` request:
```javascript
// axios.get( url, config: {} );
axios
	.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
	.then((res) => showOutput(res))
	.catch((error) => console.log(error.message));
```
`POST` request:
```javascript
// axios.post( url, data: {}, config: {})
axios
	.post('https://jsonplaceholder.typicode.com/todos', {
		title: 'New todo',
		completed: true,
	})
	.then((res) => showOutput(res))
	.catch((error) => console.log(error.message));
```
DELETE, PUT/PATCH are the same

## Interceptors
Do something with the request before axios sends it to server or do something with the response before it reaches `then`
Request Interceptors:
```javascript
axios.interceptors.request.use(
	(config) => {
		console.log(
			`${config.method.toUpperCase()} request sent to ${config.url} `
		);

		return config;
	},
	(error) => {
		return Promise.reject(error);
	}
);
```

Response Interceptors:
It can be used to log out the user on token expiry (401 status) or you can make request to refresh the token
```javascript
axios.interceptors.response.use( response => {
	// Dispatch any action on success
}, error => {
	if (error.response.status === 401 ) {
		// 1. Redirect the user to login page
		// 2. Or request to refresh the token
	}
})
```

## Custom Headers
Let's say you are working with login system that is using JWT for authentication. First, you send a post request to login. If validation is successfull, you get back a token. Then, you need to send that token in the header to access protected routes. Let's do that with axios:
```javascript
const config = {
	headers: {
		'Content-Type': 'application/json',
		Authorization: 'sometoken',
	},
};

axios
	.post(
		'https://jsonplaceholder.typicode.com/todos',
		{
			title: 'New todo',
			completed: false,
		},
		config
	)
	.then((res) => showOutput(res))
	.catch((error) => console.log(`error.message`, error.message));
```

## Axios Instances
Instances helps you write your request easier
```javascript
const axiosInstance = axios.create({
	baseURL: 'https://jsonplaceholder.typicode.com/todos?_limit=5',
	headers: {
		'Content-Type': 'application/json',
	},
	timeout: 2000 //All request will wait 2 seconds before timeout
});
axiosInstance.get('/comments').then((res) => showOutput(res));
```
In this case, you don't need to write full URL in the `GET` request anymore. That's pretty handy.

## Timeout
```javascript
// After 5s, if the request doesn't request, axios will return a timeout error.
axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5', {timeout: 5000})
```