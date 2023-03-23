# heroicons.dev

<https://heroicons.dev/>

## Context

This project has lots of state. So, it puts `state` in an context and `setState` in another context

## Cache function

using `Map()`

```ts
export type CacheEntry = readonly [
	Manifest: typeof manifestV1 | typeof manifestV2,
	Icon:     LazyExoticComponent<any>,
]

function createCache() {
	const cache = new Map<IconsetValue, CacheEntry>()
	function has(iconset: IconsetValue) {
		return cache.has(iconset)
	}
	function get(iconset: IconsetValue) {
		if (cache.has(iconset)) {
			return cache.get(iconset)!
		} else {
			let manifest: typeof manifestV1 | typeof manifestV2
			let Icon: LazyExoticComponent<any>
			switch (iconset) {
				case "v1-20-solid":
					manifest = manifestV1
					Icon = lazy(() => import("./data/iconsets/v1-20-solid")) // Icon here will be created by createElement
					break
					//...
			}
			cache.set(iconset, [manifest, Icon] as const)
			return [manifest, Icon] as const
		}
	}
	return { has, get }
}

export const cache = createCache()
```

## `Skeleton` component and ProgressBar component

## Download function

```ts
// https://stackoverflow.com/a/18197341
export function download(filename: string, content: string) {
	// Create <a download={filename} href={encoded content}>
	const anchorElement = document.createElement("a")
	anchorElement.setAttribute("download", filename)
	anchorElement.setAttribute("href", "data:text/plain;charset=utf-8," + encodeURIComponent(content))
	anchorElement.style.display = "none"
	// Append and click
	document.body.appendChild(anchorElement)
	anchorElement.click()
	// Remove
	anchorElement.remove()
}
```

## Copy function

`navigator.clipboard.writeText(text)`
