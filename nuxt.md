# Nuxt

## Routing

Internally, Nuxt still uses `vue-router` for routing. But instead of using a configuration file manually, Nuxt create it automatically by the name of file and folder in the `pages` folder (like NextJS).

To navigate between pages, Nuxt use `NuxtLink`

```js
<ul>
  <li><NuxtLink to="/about">About</NuxtLink></li>
  <li><NuxtLink to="/posts/1">Post 1</NuxtLink></li>
  <li><NuxtLink to="/posts/2">Post 2</NuxtLink></li>
</ul>
```

## Auto Import

Nuxt3 supports auto import. Here is the list of what to be auto imported

- Vue APIs like `ref`, `computed`
- Nuxt APIs like 'useFetch'
- Composibles in composible folder
- Components in components folder

All the  API (like components created in nuxt are automatically import. We don't need to import them manually.

## SEO

There are 3 ways to add metadata and other link to the Head tag of the page:

### Global configuration

Define global configuration in `nuxt.config.js`:

```js
app: {
	head: {
		title: 'Nuxt 3 Demo',
		meta: [{ name: 'description', content: 'Nuxt 3 Demo' }],
		link: [
			{
				rel: 'stylesheet',
				href: 'https://fonts.googleapis.com/icon?family=Material+Icons',
			},
		],
	},
},
```

### `useHead`

In case you need to render dynamic meta, you might want to seperate that logic from the template by using `useHead` hook.

```html
<script setup>
const route = useRoute()
useHead({
  meta: [{ name: 'og:title', content: `App Name - ${route.meta.title}` }]
})
</script>
```
Because this is dynamic, it's recommended to place this in an layout file or dynamic route page.


### Built-in components

Nuxt supports SEO meta tag and Head tag for each page via built-in components like: `<Head>`, `<Meta>`, `<Title>`. 

```html
<Head>
	<Title>Nuxt 3 Demo | {{ product.title }}</Title>
	<Meta name="description" :content="product.description" />
</Head>
```

## Layout files

To avoid duplicate common components, like menu or head component for SEO, we use layout files.

### Multiple layout

- If our website has multiple layouts, we can define the layout files and then declare it when create view component. 
- There will be a default layout which is declared in a file named `layouts/default.vue`. The content is loaded in `<slot />` component. The pages using default layout don't need to declare layout in page meta.
- For other pages using a different layout, we have to declare layout property in page meta

```html
<script>
definePageMeta({
  layout: "custom", // `custom` is the name of the layout file: `layouts/custom.vue`
});
</script>
```

### Single layout

- If you only have one layout, `app.vue` file is recommended.
- Then, you will need to use `<NuxtLayout>` and `NuxtPage` component. `NuxtPage` is like `<slot />` component.

```html
<NuxtLayout>
	<NuxtPage />
</NuxtLayout>
```

## Rendering

Nuxt support both client side rendering and client-side combined with server-side rendering

## Fetching data

We will use `useFetch` composible for fetching data. By default, the data is fetched on the server side and is cached.

```js
const { data: products } = await useFetch('https://fakestoreapi.com/products')
```

`useFetch` is cached by default, so there might be a problem when get dynamic data based on an id

```js
const { id } = useRoute().params
const uri = 'https://fakestoreapi.com/products/' + id
const { data: products } = await useFetch(uri) 
```

The data is cached and it's the same when we visit another page. Therefore, `useFetch` provide a key object to invalidate the cache

```js
const { data: products } = await useFetch(uri, {key: id}) // This is like key in React. When we provide a new key to a component, that component is rebuild.
```

`useFetch` also handle the case when the returned data is null, it will render nothing.

## .env

In Nuxt, we cannot access to environment variables because .env variable is read on server-side. Instead, we use `runtimeConfig` in `nuxt.config.js` file

```js
export default defineNuxtConfig({
	runtimeConfig: {
		privateEnv: 'value',
		public: {
			productsBaseUrl: process.env.NUXT_PUBLIC_API_BASE,
		},
	},
});
```
Variables that need to be accessible on the server are added directly inside runtimeConfig. Variables that need to be accessible on both the client and the server are defined in runtimeConfig.public.

To access the runtimeConfig, use `useRuntimeConfig` composible

```js
const runtimeConfig = useRuntimeConfig();
//  fetch the products
const { data: products } = await useFetch(runtimeConfig.public.productsBaseUrl);
```
