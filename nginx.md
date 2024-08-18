# Nginx

## Why Nginx

<img src="https://i.imgur.com/5CyJPOc.png">

Nginx is a server that 

- serve content directly when there is a request
- Behave as reverse proxy
  - sits in front of server to hide the server's identity. Clients only interact with the reverse proxy and may not know about the real server
  - accepts a request from the client, forwards the request to web servers, and returns the results to the client as if the proxy server had processed the request.

Nginx is used for:

- Protecting servers
- Load balancing
- Caching static contents like CDNs. At first, when client send request to webserver, proxy server takes that request, redirects it to webserver, receives response, cache the response and sends it back to client
- Encrypting and decrypting SSL communications

## Nginx configuration

Each `{}` is a **`block`** in `nginx.conf` is called a context. Below we have `http` context to deal with http request and `events` context

```bash
http {}

events {}
```

Inside a block we can have nested block or **`directive`** which is a key value pair

```bash
http {
	// Setup nginx server
	server {
		listen 8080 // The server listens to port 8080
		root path_to_your_directory // the path to the directory where we want to serve when we go to the port above
	}
}
```

### Mime types

By default, nginx doesn't understand file extension, so all the files will have the content type as 'text/plain'. We need to defines the content type for each file extension in the `types` directive

```bash
http {
	// ...

	types {
		text/css	css // file's extension is css will be served as text/css. Otherwise, it would be the default content type text/plain
		text/html	html
	}
}
```

Listing all the types like this is not ideals. Nginx already has `mime.types` that we can includes into configuration

```bash
http {
	// ...

	include mime.types;
}
```

### Location block

Inside `server` block, we can set a location block to define a route

```bash
location URI {}
```

There are 3 ways to config URI

- Regex match

```bash
location ~ /count/[0-9] {
	root path;
}
```

`~` means that you will be using regex here

- Prefix match: `location /test` will match /testing or /tested
- Exact match : `location = /test`

There are also 2 ways to response when user access the route

- **Using `return`**

```bash
location /test {
	# return HTTP_STATUS_CODE "response content"
	return 200 "Response content";
}

location /not-found {
	return 404 "Response not found";
}
```

- Using `root`

```bash
location /test {
	root /var/www/home;
}
```

When user access '/test', we are telling nginx to find the `index.html` (default) in `/var/www/home + /test = /var/www/home/test` directory. In case, there is no `index.html` in the directory, we can use `try_files`

```bash
location /test {
	root /var/www/home;

	// If you use another file instead of index file, nginx will look for it here
	// index.html here serves as the last fallback which is the index.html in the root folder.
	// Otherwise, it returns 404
	try_files /var/www/home/test.html index.html =404;
}
```

### Redirect

We can redirect a route to another route by using a location again

```bash
http {
	server {
		...

		location /this-need-to-redirected {
			return 307 /another-route
		}
	}
}
```

## Reverse proxy

For simplicity, reverse proxy is the server that client will interact with, rather than the web server. All the requests and response are passed through proxy

Here is the basic setup of a reverse proxy

```bash
location /test {
  proxy_pass 'http://localhost:7777/';
}
```

If we remove the slash in the end, nginx will redirect to `http://localhost:7777/test`


