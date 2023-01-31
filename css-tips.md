# CSS Tips

## Table border radius and box shadow

Create a div wrapper and apply border radius and box shadow for it

## DON'T USE SHORTHAND CSS FOR UTILITY CLASS

Because it might prevent overriding CSS

## Reorder the element using display grid

Use `grid-template-area` instead of `grid-template-columns`

## Conditional CSS

### Using `:has`

<https://12daysofweb.dev/2022/css-has-selector/>
```css
.other-field {
  display: none;
}

form:has(option[value="other"]:checked) .other-field {
  display: block;
}

/*Change grid columns based on number of items*/
.wrapper {
  --item-size: 200px;
  display: grid;
  grid-template-columns: repeat(
    auto-fill,
    minmax(var(--item-size), 1fr)
  );
  gap: 1rem;
}

.wrapper:has(.item:nth-last-child(n + 5)) {
  --item-size: 120px;
}
```

### Force wrap to a new line using `flex-basis`

```css
/* If it’s less than 190px, it will wrap into a new line.*/
.card__title {
  flex-grow: 1;
  flex-basis: 190px;
}
```

### Header, body, footer layout using `min-content`

```css
/*https://i.imgur.com/KCSwUm7.png*/
.c-todo {
  display: grid;
  grid-template-rows: min-content auto min-content;
  height: 100vh;
}
```


## Using CSS for mobile screen

```html
<!-- Loading and parsing mobile.css is not render-blocking on large screens -->
<link
  rel="stylesheet"
  href="mobile.css"
  media="screen and (max-width: 480px)" />
```

## `:only-child` and `:only-of-type`

<https://www.youtube.com/watch?v=yyPteFyZsCE>

## Disable double click to zoom in iOS.

```css
button {
    touch-action: manipulation;
}
```

## Background image opacity

Apply opacity to absolute psuedo element, parent element use isolation: isolate
<https://www.youtube.com/watch?v=lRPguPbovro>

## Replace `position: absolute` with `display: grid`

Set `grid-area: 1/-1` the same for all the child element
<https://ishadeed.com/article/less-absolute-positioning-modern-css/>
<https://www.youtube.com/watch?v=sKFW3wek21Q>

## Animate from `display: none`

<https://www.youtube.com/watch?v=4prVdA7_6u0>

## Inconsistantly sized logo

```css
.logo {
  width: 15%;
  aspect-ratio: 3/2;
  object-fit: contain;
  /* For logos that have different background color with container background color  */
  mix-blend-mode: color-burn; 
}
```

## `z-index` without `position`

We can use z-index on a grid or flex item. No need to add `position: relative` at all.

## `display: contents` to change the HTML order

Basically, when the parent element has `display: content`, it is removed from the html, but the children elements stay still
<https://ishadeed.com/article/less-absolute-positioning-modern-css/>

## Fix overflow problem when using `display: grid`

<https://ishadeed.com/article/min-content-size-css-grid/>

If the child element has a slider or something that has exceeds the width of that child, we need to set its width as `minmax(0, 1fr)` instead of `1fr`

## `scroll-snap`

<https://codepen.io/5t3ph/pen/yLzQeGr/652097fa9ce1150aeb5400637ab91b63>