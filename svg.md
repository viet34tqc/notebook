# SVG

## Basic shape

- `rect`: draw a rectangle, need `width`, `height`, `x` (start point), `y` (end point)
- `circle`: draw a circle
- `g`: group shape
- `line`: draw a line
- `path`: draw multiple line
  - `d`: draw
  - `M`: start position
  - `L`: L<next_horizontal_point> <next_verticle_point>

```html
<svg width="960" height="500">
  <g transform="scale(1.7)">
    <circle cx="50" cy="50" r="40"></circle>
    <rect x="150" y="25" width="50" height="50"></rect>

    <circle cx="50" cy="150" r="40" fill="red"></circle>
    <rect x="150" y="125" width="50" height="50" fill="#83cc2a"></rect>

    <g transform="translate(0, 200)" fill="#adf6ff" stroke="black">
      <circle cx="50" cy="50" r="40" stroke-width="5"></circle>
      <rect x="150" y="25" width="50" height="50"></rect>
    </g>

    <g class="lines" transform="translate(50, 0)">
      <line x1="200" y1="20" x2="300" y2="280"></line>
      <path fill="none" d="M300 280 L350 200 L400 250 L450 230"></path>
    </g>
  </g>
</svg>
```

## `defs` and `use`

Define elements without directly rendering it and that element will be serve as template for future use. `defs` is really useful for reusable pattern

Let's say we have to draw a group of houses. The houses can have different colors and we don't want to repeat the code to draw a single house.

```html
<svg width="280px" height="120px" viewBox="0 0 280 80" xmlns="http://www.w3.org/2000/svg">
    <defs>
        <g id="house" style="stroke: black;">
            <rect x="0" y="41" width="60" height="60" />
            <polyline points="0 41, 30 0, 60 41" />
            <polyline points="30 101, 30 71, 44 71, 44 101" />
        </g>
    </defs>
</svg>
```

The houses are not rendered yet. We will use `use` to display the the element defined in `defs`

```html
<svg width="280px" height="120px" viewBox="0 0 280 80" xmlns="http://www.w3.org/2000/svg">
    <defs>
       .....
    </defs>
    <!-- make use of the defined groups -->
    <use xlink:href="#house" x="0" y="0" style="fill: #cfc;" />
    <use xlink:href="#house" x="70" y="0" style="fill: yellow;" />
</svg>
```

Now we have 2 houses with different colors.

## `textPath`

<https://alligator.io/svg/textpath/>