# Nginx

## Why Nginx

<img src="https://i.imgur.com/5CyJPOc.png">

Nginx is a server that 

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
http {
	// Setup nginx server
	server {
		listen 8080 // The server listens to port 8080
		root path_to_your_directory // the path to the directory where we want to serve when we go to the port above

		location /your-route {
			root the_same_as_root_path // nginx automatically adds /your-route to the path here

			// If you use another file instead of index file, nginx will look for it here
			// index.html here serves as the last fallback which is the index.html in the root folder.
			// Otherwise, it returns 404
			try_files another_path_of_your_file/not_index.html index.html =404;
		}
	}
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
