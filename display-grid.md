# Display: grid

<https://learncssgrid.com/>

## fr

Fr is fraction unit and 1fr is for 1 part of the remaining available space

## `grid-auto-rows` and `grid-auto-columns`

Default value size for rows and columns.

## `justify-items` and `justify-content`

Display grid divides container into rows and columns. If the columns don't take the whole width of container, `justify-content` will work like `justify-content` in display flex

Meanwhile, when the item inside each column doesn't take the whole width of that column, `justfiy-items` will work like that column has `display: flex` and `justify-content`

In conclusion, `justify-content` align grid items, `justfiy-items` align content of grid items. The same goes for `align-items` and `align-content`

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

## `place-content` and `place-items`

<https://www.youtube.com/watch?v=vNwoDkn7AIc>

- `place-content`: 
  - shorthand for `align-content` and `justify-content`
  - behaves like `display: flex`
- `place-items`:
  - shorthand for `align-items` and `justify-items`
  - align the content in that grid