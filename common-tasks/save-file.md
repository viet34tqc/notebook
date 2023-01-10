# Save file as

Save file as using `window.showSaveFilePicker`

<https://gist.github.com/viet34tqc/57bcb7d5de26cbbda45dcb9525abf531>

```js
// Check for browser support
if (window.showSaveFilePicker === undefined) {
	const msg = [
	  "Your browser does not allow Samuwrite to save files.",
	  "Please switch to another browser or use the Mac app of Samuwrite.",
	].join(" ");
	throw Error(msg);
}

let handle: FileSystemFileHandle;
try {
	handle = await window.showSaveFilePicker({
	  excludeAcceptAllOption: false,
	  suggestedName: "Untitled",
	  types: [{ description: "Markdown", accept: { "text/plain": [".md"] } }],
	});
} catch (error: unknown) {
	// User clicks "Cancel" on the file picker
	if (error instanceof DOMException && error.code === error.ABORT_ERR)
	  return null;
	throw error;
}

// Write content
const writable = await handle.createWritable();
await writable.write(content);
await writable.close();
```
