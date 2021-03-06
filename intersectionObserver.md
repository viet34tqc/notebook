# Basic IntersectionObserver

## Step 1: Create an observer

`new IntersectionObserver(callback(entries: HTMLDOMElement[], observer: Object), observerOptions)`
```javascript
const appearOptions = {
	threshold: 0,
	rootMargin: '0px 0px -250px 0px',
};

const appearOnScroll = new IntersectionObserver(function (entries, observer) {
	entries.forEach((entry) => {
		if (!entry.isIntersecting) {
			return;
		} else {
			entry.target.classList.add('appear');
			observer.unobserve(entry.target);
		}
	});
}, appearOptions);
```

## Step 2: Tageting the element to be observered

```javascript
const sliders = document.querySelectorAll('.slide-in');

sliders.forEach((slider) => {
	appearOnScroll.observe(slider);
});
```