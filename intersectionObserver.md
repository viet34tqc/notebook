# Basic IntersectionObserver

## Step 1: Create an observer

`new IntersectionObserver(callback(entries: IntersectionObserverEntry[], observer: Object), observerOptions)`

- `IntersectionObserverEntry`: is the object contains information between the target and its root element
- `observer`: is the reference to the observer itself
- observerOptions: contains observer configuration
  - `root`: specifies the element that serves as the viewport for checking the visibility of the target element. By default, this is set to the browser window.
  - `rootMargin`: allows you to specify a margin around the root element, effectively expanding or shrinking its dimensions. This is useful when you want to detect when an element enters or exits a certain area around the viewport.
 - `threshold` sets a single number or an array of numbers between 0 and 1 that determine when the observer triggers a callback. For example, if we set `threshold: 0.5`, then our callback function will be called whenever at least half of the target element is visible in the viewport.

```javascript
const appearOptions = {
	threshold: 0,
	rootMargin: '0px 0px -250px 0px',
};

const observer = new IntersectionObserver(function (entries, observer) {
	entries.forEach((entry) => {
		if (!entry.isIntersecting) {
			return;
		} else {
			entry.target.classList.add('appear');
			observer.unobserve(entry.target);
		}
	});
}, appearOptions);

// If you just want to toggle A when B intersect
// https://www.youtube.com/watch?v=V-CBdlfCPic&ab_channel=KevinPowell
const navObserver = new IntersectionObserver(() => {
	primaryHeader.classList.toggle('sticking');
})
navObserver.observe(scrollWatcher)
```

## Step 2: Tageting the element to be observered

Observer has a method `observe`. We use this method to observe the element we want

```javascript
const sliders = document.querySelectorAll('.slide-in');

sliders.forEach((slider) => {
	observer.observe(slider);
});
```