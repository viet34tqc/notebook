# NextJS

## Why NextJS

- Zero-config: no Webpack and Babel
- Server-side rendering: Content is populated from the server which is impossible for CRA => good for SEO
- Routing without react router
- Images optimization with `Image`
- Code splitting and bundling

## Page Routing

If you are using create-react-app and you are in need of building routes for your app, then you will need to install third-party package like **react-router-dom**. With NextJS, that installation isn't necessary anymore because NextJS support routing by default.

For navigating between pages, NextJS provides you a component named `Link`:

```JSX
import Link from 'next/link';
<Link href='about'>About</Link>
```

Needless to say, you can still use HTML `a` tag and it works. However, when you click on the link using `a` tag, your page will be refreshed.

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

## Data Fetching

### getStaticProps

Allows us to fetch data from external API before component renders, and we will pass that data as a prop to the component.
Take a note that you shouldn't fetch data from NextJS's API route.

When a page with `getStaticProps` is pre-rendered at build time, in addition to the page HTML file, Next.js generates a JSON file holding the result of running `getStaticProps`.

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

Let's say you have a bunch of single posts. These posts is fetched from an API and you need to create routes for them at build time (`next build`). This is done in `getStaticPath` function

```js
export const getStaticPaths = async () => {
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

## Client-side rendering & Server-side rendering & Static-site generation & Pre-rendering

### Client-side rendering (Plain React.js app)

Initial Load: App is not rendered
JS Loaded
Hydration: App is hydrated, React components are initialized and App becomes interactive
If you are trying to visit a react application that has a really large bundle on a slow connection, you might see a flash of white on the page

Pros:

- Great for UX

Cons:

- Not good for SEO because Google bot cannot crawl any data at first load because it can only sees a div

### Static-site generation (Pre-rendering using NextJS)

The HTML is generated at build-time and is reused for each request. This is the default rendering method of NextJS.

Initial Load: Server serves initial HTML and that's displayed on the initial load which leads to faster TTFB
JS Loaded
Hydration: App is hydrated, React components are initialized and App becomes interactive

Pros:

- This method is really good for SEO because bots get fully rendered HTML and they can easily interpret the content on the page.
- The data is fetched only one time and cached.
- Faster TTFB
  Const:
- The data can get stale until you regenerate the content.

In NextJS, you can use `next build` to build the app for production. When that happened, HTML is generated and is reused for every request. Two types of static-site generation

- Without data: for pages that can be generated without fetching external data at build time. It's just simple HTML without talking to any APIs.
- With data: for pages that can only be generated **after fetching** external data at build time. In NextJS, we fetch data in `getStaticProps` function

### Server-side rendering

The HTML gets generated on every request. This is used when you have a data that needs to be fresh at every request. The server will fetch the data, render the HTML and returns to the user.

Pros:

- This method is really good for SEO because bots get fully rendered HTML and they can easily interpret the content on the page.
- The data is always fresh
  Const:
- Slower.
- Redundant data fetching

### Incremental Static Regeneration

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
