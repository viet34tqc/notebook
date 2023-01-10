# Upload file using JS

## Using `Window.showOpenFilePicker`

This method is not available on Firefox, still need polyfill

<https://gist.github.com/viet34tqc/e56ec0d9157cbdcdf1685a779400fb2d>

```ts
if (window.showOpenFilePicker === undefined) {
	const msg = [
	  "Your browser does not allow Samuwrite to open local files.",
	  "Please use another browser or the Mac app of Samuwrite.",
	].join(" ");
	throw Error(msg);
}

let handle: FileSystemFileHandle;

try {
	const accept = { "text/plain": [".md", ".mdx", ".txt"] };
	const result = await window.showOpenFilePicker({
	  excludeAcceptAllOption: false,
	  multiple: false,
	  types: [{ description: "Markdown & Text", accept }],
	});
	handle = result[0];
} catch (error: unknown) {
	// User clicks "Cancel" on the file picker
	if (error instanceof DOMException && error.code === error.ABORT_ERR) return null;
	throw error;
}

const file = await handle.getFile();
```
