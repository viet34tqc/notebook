# Basic IntersectionObserver

## Step 1: Create an observer

`new IntersectionObserver(callback(entries: HTMLDOMElement[], observer: Object), observerOptions)`

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