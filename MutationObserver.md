# MutationObserver

<https://www.macarthur.me/posts/use-mutation-observer-to-handle-nodes-that-dont-exist-yet>

Its purpose is to do something when the DOM tree changes, so we can use mutation observer to handle nodes that dont exist yet

```js
const domObserver = new MutationObserver((_mutationList, observer) => {
	const button = document.getElementById('button');

	if (button) {
    	button.addEventListener('click', () => console.log('clicked!'));

		// No need to observe anymore. Clean up!
		observer.disconnect();
 	}
});

domObserver.observe(document.body, { childList: true, subtree: true });
```
