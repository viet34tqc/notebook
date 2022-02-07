# Display: grid

## fr

Fr is fraction unit and 1fr is for 1 part of the available space

## `grid-auto-flow: dense`

This will attempt to fill all the holes in the layout. If the smaller item comes after the big item, there would be spaces before the big item. Using `grid-auto-flow: dense` makes the smaller items precede the big item

<https://css-tricks.com/expandable-sections-within-a-css-grid/>

## `auto-fill` and `auto-fit`

<https://css-tricks.com/auto-sizing-columns-css-grid-auto-fill-vs-auto-fit/>

Syntax:

`grid-template-columns: repeat( auto-fit, minmax(250px, 1fr) );`
`grid-template-columns: repeat( auto-fill, minmax(250px, 1fr) );`

The difference between `auto-fill` and `auto-fit` is only noticeble when the row is wide enough to fit more columns in it.

If you are using `auto-fit`, the content will stretch to fill the entire row width
Whereas with `auto-fill`, the browser will allow empty columns to occupy space in the row. These empty columns will be allocated a fraction even if they don't have any content

In case you have only one item in the row, `auto-fit` will expend it to fill the container width and it's unexpected.