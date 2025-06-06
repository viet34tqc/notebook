# CSS Tips

## Table border radius and box shadow

Create a div wrapper and apply border radius and box shadow for it

## DON'T USE SHORTHAND CSS FOR UTILITY CLASS

Shorthand CSS is like `margin: 0px 1px`
Because it might prevent overriding CSS

## Reorder the element using display grid

Use `grid-template-area` instead of `grid-template-columns`

## Conditional CSS

### Using `:has`

- <https://ishadeed.com/article/css-has-guide/>
- <https://12daysofweb.dev/2022/css-has-selector/>
- <https://www.smashingmagazine.com/2023/01/level-up-css-skills-has-selector/>

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

/* Fix layout when sidebar can be hidden */
.grid {
  display: grid;
}
.grid:has(.sidebar) {
  grid-template-columns: 1fr 3fr ;
}
/*Or*/
.grid > .content:only-child {
  grid-column: 1 / -1;
}

/* select prior siblings */
li:has(+ li:hover) {}
h2:has( + h3) /*every h2 that has an adjacent sibling which is an h3*/
```

## Sub grid

Use `subgrid` when you want the size of the grid items to be the same between grid container and nested grid container: <https://12daysofweb.dev/2022/css-subgrid/#you-can-line-up-items-that-are-in-different-subgrids>

Example: you have a layout of card, the title's length of the cards are not the same => the title are not aligned together and so do other components in the cards. Now you use `subgrid`, like so:

```css
.grid {
  display: grid;
}

.card {
  grid-row: auto / span 2;
  display: grid;
  grid-template-rows: subgrid;
  gap: 1px;
}
```

The container doesn't define the row size explicitly. This size is defined by the `grid-row` attribute in the grid item. So, each item will have 2 rows (auto min 1, from row 1, spans 2 rows). Then, we use `grid-template-rows: subgrid` => The row size in the parent container will apply into card, now the title in all the cards will have the same row size.

When using this technique, we need to make sure the number in `grid-row: auto / span ${number};` must equal or greater than the number of component in the card. In the example above, we only have title and content in each card.

## Height with transition

<https://www.youtube.com/watch?v=B_n4YONte5A>

- Use `flex-direction: column` for wrapper and `flex-basis: 0` for item
- Use `grid-template-rows: 0`

## Force wrap to a new line using `flex-basis`

<https://ishadeed.com/article/conditional-css/>

```css
/* If it’s less than 190px, it will wrap into a new line.*/
/* Here flex-shrink is default to 1 */
/* flex-basis without flex-shrink is like min-width: 190px */
.card__title {
  flex-grow: 1;
  flex-basis: 190px;
}
```

## Remove button outline

<https://www.youtube.com/shorts/4B_4WLpbyp8>

`outline-color: transparent`

## Header, body, footer layout using `min-content`

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

- <https://www.youtube.com/watch?v=lRPguPbovro>
- <https://www.youtube.com/watch?v=o1HzOJfgugE>

By default, when you apply `z-index: -1` to the overlay which is a psudo element, the element lies behind every other elements in the DOM, even the relative parent element.

Applying `isolation: isolate` to relative parent element (or a positive `z-index`) will create a stacking context and prevent absolute children with negatives `z-index` from lying behind other elements, just behind other child elements in the stacking context.

## Replace `position: absolute` with `display: grid`

Set `grid-area: 1/-1` the same for all the child element

- <https://ishadeed.com/article/less-absolute-positioning-modern-css/>
- <https://www.youtube.com/watch?v=sKFW3wek21Q>

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

We can use `z-index` on a grid or flex item. No need to add `position: relative` at all.

## `display: contents` to change the HTML order

Basically, when the parent element has `display: content`, it is removed from the html, but the children elements stay still
<https://ishadeed.com/article/less-absolute-positioning-modern-css/>

## Fix overflow problem when using `display: grid`

<https://ishadeed.com/article/min-content-size-css-grid/>

If the child element has a slider or something that has exceeds the width of that child, we need to set its width as `minmax(0, 1fr)` instead of `1fr`

## Sidebar layout

```css
.with-sidebar {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}
.sidebar {
  /* ↓ The width when the sidebar _is_ a sidebar */
  flex-basis: 20rem;
  flex-grow: 1;
}

.not-sidebar {
  /* ↓ Grow from nothing */
  flex-basis: 0;
  flex-grow: 999;
  /* ↓ Wrap when the elements are of equal width */
  min-inline-size: 50%;
}
```

## Form control and input width problem

By default, the input doesn't take the whole width of its parents. Then, we can apply `display: grid` into parent class

```css
.form-control {
  display: grid;
}
```

## `scroll-snap`

<https://codepen.io/5t3ph/pen/yLzQeGr/652097fa9ce1150aeb5400637ab91b63>

## Alternative to input number

- <https://css-tricks.com/finger-friendly-numerical-inputs-with-inputmode/>
- <https://codepen.io/kevinpowell/pen/dyjwWEY>

The problem with `input type="number"`:

- It has arrows that is inappropriate when we want a credit-card number input
- We can type text when using Firefox

`<input inputmode="numeric" pattern="[0-9]*" type="text" name="creditcard">`

```js
numericsInput.forEach((e) => {
  validateInput(e)
})

function validateInput(el) {
  el.addEventListener("beforeinput", function (e) {
    let beforeValue = el.value;
    e.target.addEventListener(
      "input",
      function () {
        if (el.validity.patternMismatch) {
          el.value = beforeValue;
        }
      },
      { once: true }
    );
  });
}
```

## Breakout element using css grid and grid columns name

This method is too good to be true.

<https://www.youtube.com/watch?v=c13gpBrnGEw>

## Child element breaks out parent element

<https://archive.hankchizljaw.com/wrote/creating-a-full-bleed-css-utility/>

```css
.full-bleed-image {
  width: 100vw;
  margin-left: 50%;
  transform: translateX(-50%);
}
```

