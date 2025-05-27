# CSR, SSG, SSR

<https://reacttraining.com/blog/react-architecture-spa-ssr-rsc>

## Client-side rendering (Plain React.js app)

All the rendering happens in the browser (client). It loads a single HTML page and dynamically updates the content as the user interacts with the app, without requiring a full page reload.

- Initial Load: App is not rendered
- Browser parse HTML and JS loaded
- React renders the tree and generates all the DOM nodes

If you are trying to visit a react application that has a really large bundle on a slow connection, you might see a flash of white on the page

Pros:

- Fast TTFB because most of computation happens on client side
- Lower server usage

Cons: <https://reacttraining.com/blog/understanding-spas-and-their-shortcomings>

- User get a blank page while waiting for JS code to run
- Performance: bundle size can be huge if we don't use lazy-loading to split the bundle. Hence, metrics like First Paint, First Contentful Paint, Largest Contentful Paint, and Time-to-Interactive require more time.
- Data fetching issue: When you want to navigate to this second page, you don't have the JS code for it yet. When the user goes to that page, we start the download process. When the JS code arrives, we now start to render the page which has data-fetching inside the component. This means we'll then start to fetch data for that page after we already had to wait for the JS in the first place
- SEO: not good for SEO because Google bot cannot crawl any data at first load because it can only sees a div

## Pre-rendering HTML

<https://www.webscope.io/blog/server-components-vs-ssr>

Pre-rendering is the default mechanism for rendering page in NextJS. The HTML is dynamically pre-rendered on the server and then sent to the client along with JS bundle(s) and other assets. Then on the client, the HTML is hydrated with JS.

There are two type of pre-rendering:

- Static pre-rendering (Static-site generation)
- Dynamic pre-rendering (Server-side rendering)

### Static-site generation

In static-site generation, The HTML is generated at build time and is reused for each request.

Why:
Client-side rendering gives user a blank page at first. That's not good for UX. SSG gives the user something to look at while we download JS, parse, and hydrate the app

- Initial Load: Server serves initial HTML and that's displayed on the initial load which leads to faster TTFB
- JS Loaded
- Hydration: App is hydrated, React components are initialized and App becomes interactive. [https://www.gatsbyjs.com/docs/conceptual/react-hydration/](https://www.gatsbyjs.com/docs/conceptual/react-hydration/)

Pros:

- This method is really good for SEO because bots get fully rendered HTML and they can easily interpret the content on the page.
- The data is fetched only one time and cached.
- Time To First Byte, First Paint, First Contentful Paint, Largest Contentful Paint benefit from this approach as it is very similar to static serving.

Const:

- The data can get stale until you regenerate the content.
- Time To Interactive can be slow because of hydration process

When to use: The content of the page doesn't update frequently. Data must be known at build time.

- Marketing pages
- Blog posts
- E-commerce product listings
- Help and documentation

In NextJS, you can use `next build` to build the app for production. When that happened, HTML is generated and is reused for every request. Two types of static-site generation

- Without data: for pages that can be generated without fetching external data at build time. It's just simple HTML without talking to any APIs.
- With data: for pages that can only be generated **after fetching** external data at build time. In NextJS, we fetch data in `getStaticProps` function

### Server-side rendering

The HTML output of React components gets generated on every request. This is used when you have a data that needs to be fresh at every request. The server will fetch the data, render the HTML and returns to the user.

Pros:

- This method is really good for SEO because bots get fully rendered HTML and they can easily interpret the content on the page.
- First Paint, First Contentful Paint, Largest Contentful Paint is fast as it is very similar to static serving.
- The data is always fresh

Const:

- Generate pages on the server takes time, that often result in a slower TTFB, unlike SSG
- Time To Interactive can be slow because of hydration process
- UX is not so good because navigation needs page refresh
- Redundant data fetching (if not cache)

When to use: The content of the page updates frequently and it changes on every user request, ex: checkout page, cart page...

### SSR and NextJs

Frameworks like Next and React Router Framework do SSR and CSR. Any page you visit will serve fully formed HTML (SSR). However, when you go from one page to the next, the next page is a client-side navigation without a refresh (CSR like SPAs). The framework takes care of the lazy loading and code-splitting for you in a way that doesn't have the same pitfalls as the home-grown approach to SPAs. This is why I call React frameworks a hybrid: best of SPA concepts and best of SSR rendering.

Overall, I'd say these frameworks take care of your app in ways that you'd normally have to manually do yourself. That's their primary benefit.

**How do they take care of lazy-loading for you?** Lazy loading happens automatically because you follow certain conventions that tell the framework how your site is built. They know what to load when you need it. You can even do pre-fetching which means we start to load the next pages before the user even goes there -- creating a very fast experience.

**How do they do client-side navigations?** The first page visit will get HTML from the server. Then the client loads a small JS file which has some framework code and just enough JS for the components on the current page. It also knows all the next pages we can go to. When the user clicks to go to another page, the framework handles getting more JS from the server while also data-fetching the data for that page in parallel. This all happens without a page refresh, so we're getting the best aspects of SPA and MPA at the same time.