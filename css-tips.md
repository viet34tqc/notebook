# CSS Tips

## Table border radius and box shadow

Create a div wrapper and apply border radius and box shadow for it

## DON'T USE SHORTHAND CSS FOR UTILITY CLASS

Because it might prevent overriding CSS

## `:only-child` and `:only-of-type`

<https://www.youtube.com/watch?v=yyPteFyZsCE>

## Replace `position: absolute` with `display: grid`

Set `grid-area: 1/-1` the same for all the child element
<https://ishadeed.com/article/less-absolute-positioning-modern-css/>
<https://www.youtube.com/watch?v=sKFW3wek21Q>

## Animate from `display: none`

<https://www.youtube.com/watch?v=4prVdA7_6u0>

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