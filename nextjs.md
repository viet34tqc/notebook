# NextJS

<https://www.youtube.com/watch?v=PN1HgvAOmi8>
<https://tsh.io/blog/ssr-vs-ssg-in-nextjs/>
<https://prateeksurana.me/blog/future-of-rendering-in-react/>

## Why NextJS

- Zero-config: no Webpack and Babel
- Server-side rendering: Content is populated from the server which is impossible for CRA => good for SEO
- Routing without react router
- Images optimization with `Image`
- Code splitting and bundling

## How

NextJS renders HTML files with full content in advance that is good for SEO. When user get the content, client-side rendering takes over and makes the website interactive like tradition react app (this process is called hydration)

## Page Routing

If you are using create-react-app and you are in need of building routes for your app, then you will need to install third-party package like **react-router-dom**. With NextJS, that installation isn't necessary anymore because NextJS support routing by default.

For navigating between pages, NextJS provides you a component named `Link`:

```JSX
import Link from 'next/link';
<Link href='about'>About</Link>
```

Needless to say, you can still use HTML `a` tag and it works. However, when you click on the link using `a` tag, your page will be refreshed.
Besides, NextJS will automatically **prefetch** the code for the linked page in the background. By the time you click on the link, the code for the linked page will already loaded in the background, and the page transition will be near-instant.

**Note**:

- If you need to link to an external page outside the Next.js app, just use an <a> tag without Link.
- If you need to add attributes like, for example, className, add it to the a tag, not to the Link tag

```jsx
<Link href="/">
	<a className="foo" target="_blank" rel="noopener noreferrer">
	Hello World
	</a>
</Link>
```

## Use next link with 3rd library

`Link` component just render an `a` tag without any styles. Besides, you can not use `Link` with another 3rd library like Material UI or Chakra, which also have `Link` component. In this case, you need to use `NextLink`

```jsx
import NextLink from 'next/link'
import { Link as MuiLink } from '@mui/material'

// We have to use passHref to pass `href` to MUILink
<NextLink href="/" passHref>
  <MuiLink>Home</MuiLink>
</NextLink>
```


## Adding third-party scripts

Use `Script` from `next/script`:

```jsx
export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      <Script
        src="https://connect.facebook.net/en_US/sdk.js"
        strategy="lazyOnload"
        onLoad={() =>
          console.log(`script loaded correctly, window.FB has been populated`)
        }
      />
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  );
}
```

## Use cases

### Retrieving data from URLs

Use `useRouter` hook to get the params

### Dynamic Routing

<https://nextjs.org/docs/routing/dynamic-routes>
For example, you need to create a page for a post based on its id, then in the **pages** folder, you have to create **post** folder. Next, create a `[id].js` in the **post** folder.

## API routes

When you run build command, each of the API is a .json file that represents a severless server
API routes cannot be used with `next export`

## Hooks

### useRouter

Similar to React Router

```javascript
const router = useRouter(); // In React Router, it's useHistory()
router.push('/');
```

In React Router, we have another hooks for URL params. In NextJS, it's included in `useRouter`

```javascript
const router = useRouter();
const { id } = router.query;
```


## getStaticProps

Allows us to fetch data from external API before component renders and pass that data as a prop to the component on build.
Take a note that you shouldn't fetch data from NextJS's API route.

When a page with `getStaticProps` is pre-rendered at **build time**, in addition to the HTML file, Next.js generates a JSON file holding the results of running `getStaticProps`.

In development, `getStaticPath` runs on every requests
In production, `getStaticPath` runs only at build time

When you navigate to a page that's pre-rendered using `getStaticProps`, Next.js fetches this JSON file (pre-computed at build time) and uses it as the props for the page component. This means that client-side page transitions will not call getStaticProps as only the exported JSON is used.

```javascript
export const getStaticProps = async ({ params }) => {
	const country = await getCountry(params.id);

	return {
		props: {
			country,
		},
	};
};
```

Then we can get the country in the component like this:

```javascript
const Country = ({ country }) => {};
```

## getStaticPath

This function helps NextJS know which dynamic pages to pre-render. For example, if you name your dynamic route as `[id].js`, it returns the array of possible value for the `[id]`

Let's say you have a bunch of single posts. These posts are fetched from an API and Next needs to know their ids in advance. This is done in `getStaticPath` function. It returns the path that contains an array with every route for this dynamic url.

`getStaticPath` is used with `getStaticProps`. Both of them is only run on server-side.

In development, `getStaticPath` runs on every requests
In production, `getStaticPath` runs only at build time

```js
export async function getStaticProps({ params }) {
  // Fetch necessary data for the blog post using params.id
}

export const getStaticPaths = async () => {
	// Return a list of possible value for id
	// The paths is an array of objects. Each object **must have** `params` property.
	// The value of `params` property must be an object with `id` key (if you named your dynamic route as [id].js)
	const res = await fetch('https://restcountries.eu/rest/v2/all');
	const countries = await res.json();

	const paths = countries.map((country) => ({
		params: { id: country.alpha3Code },
	}));

	return {
		paths,
		fallback: false,
	};
};
```

`fallback` property is used when the path is not existed yet (is not returned by `getStaticPaths`). It accepts 3 values: 

- `false`: it returns 404 page
- `true`: 
  - The path that has not been generated at build time will not return 404. Instead, NextJS will serve a *fallback version* of the page
  - In the background, the path is statically generated, **loading state** is shown while generating this path 
  - Page is rendered with required props after generating 
  - New path will be cached in CDN (later requests will result in cached page) - crawler Bots may index fallback page (not good for SEO)
- `blocking`:
  - The path is switch to server-side rendering. Future requests will serve the static file from the cache.
  - the path is statically generated **without loading state** (no fallback page) - new path will be cached in CDN (later requests will result in cached page)


## Client-side rendering & Server-side rendering & Static-site generation & Pre-rendering

### Client-side rendering (Plain React.js app)

HTML is rendered on client side by using JS

- Initial Load: App is not rendered
- JS Loaded
- Hydration: App is hydrated, React components are initialized and App becomes interactive (Hydration is the process of getting your static HTML from server and turning it into dynamic DOM that React can modify)

If you are trying to visit a react application that has a really large bundle on a slow connection, you might see a flash of white on the page

Pros:

- Good for UX => used a lot in web application
- Lower server usage

Cons:

- Not good for SEO because Google bot cannot crawl any data at first load because it can only sees a div
- High Initial load time because of JS bundle, then we need to run that JS, call API to get data...

### Static-site generation (Pre-rendering using NextJS)

The HTML is generated at build time and is reused for each request. This is the default rendering method of NextJS.

- Initial Load: Server serves initial HTML and that's displayed on the initial load which leads to faster TTFB
- JS Loaded
- Hydration: App is hydrated, React components are initialized and App becomes interactive (convert from static HTML to interactive HTML)

Pros:

- This method is really good for SEO because bots get fully rendered HTML and they can easily interpret the content on the page.
- The data is fetched only one time and cached.
- Faster TTFB

Const:

- The data can get stale until you regenerate the content.

In NextJS, you can use `next build` to build the app for production. When that happened, HTML is generated and is reused for every request. Two types of static-site generation

- Without data: for pages that can be generated without fetching external data at build time. It's just simple HTML without talking to any APIs.
- With data: for pages that can only be generated **after fetching** external data at build time. In NextJS, we fetch data in `getStaticProps` function

When to use:

The content of the page doesn't update frequently

- Marketing pages
- Blog posts
- E-commerce product listings
- Help and documentation

### Server-side rendering

The HTML output of React components gets generated on every request. This is used when you have a data that needs to be fresh at every request. The server will fetch the data, render the HTML and returns to the user.

Pros:

- This method is really good for SEO because bots get fully rendered HTML and they can easily interpret the content on the page.
- The data is always fresh

Const:
- UX is not so good because navigation needs page refresh
- Redundant data fetching (if not cache)

When to use:

The content of the page updates frequently and it changes on every user request, ex: checkout page, cart page...

- Marketing pages
- Blog posts
- E-commerce product listings
- Help and documentation

### Incremental Static Regeneration

<https://vercel.com/docs/concepts/next.js/incremental-static-regeneration>

In static-site generation with Data, the data fetching is run at build time. However, you can also make it happen at <i>request time</i> **without having to re-build the entire site**. This is done by using `revalidate` property.

```javascript
export const getStaticProps = async ( { params } ) => {
  const country = await getCountry( params.id );

  return {
    props: {
      country,
    },
    **revalidate: 60** // seconds
  };
};
```

So, if the users visit your site at the first 60 seconds, he will see the static HTML that was generated at build time. After 60s, the next person who visits the site, he will see the stale version of it. This is based on the `Cache-Control` `stale-while-revalidate` HTTP headers.

Then, in the background, NextJS triggers a regeneration of the page by calling `getStaticProps` again. Once the page has been successfully generated, it invalidtes the cache and updates it with the newest version of the page. Now, the next person who comes to the site will see the most updated content
If the background regeneration fails, the old page will stay unaltered

## Which rendering method to use

<img src="https://i.imgur.com/HOdyKup.png" />
<img src="https://i.imgur.com/WnE3o7T.png" />