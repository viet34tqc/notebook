# Display: grid

<https://www.coltsteele.com/tutorials/mastering-css-grid>
<https://learncssgrid.com/>
<https://www.coltsteele.com/tutorials/mastering-css-grid>

## fr

Fr is fraction unit and 1fr is for 1 part of the remaining available space. So to calculate 1fr, we have to get the remaining space first.

- If there is no static unit, like `1fr 2fr 1fr`: the remaining space is full width
- If there is unit other than `fr`, like `200px 1f 1fr` or `minmax(200px, 400px) 1fr 1fr`: the remaining space = full width - static value. It means:
  + Any length units that are not fractions will be allocated first.
  + The fractions will apply to the remaining space.

## `minmax(min, max)`

<https://ishadeed.com/article/css-grid-minmax/>
<https://www.hongkiat.com/blog/css-grid-layout-minmax/>

The returned value lies between `min` and `max` value
- static value `minmax(100px, 200px)`: by default, if there is space, this returns `200px`, otherwise `100px` is the value (not less than 100px)
- dynamic value `minmax(100px, 1fr) 1fr 1fr`: 1fr is calculated first, the static value is `100px`. If 1fr value is bigger than 100px, then it returns 1fr

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

- `auto-fit` will fit as many columns as it can into the space.
- `auto-fill` will fill the row with as many columns as it can, even if there's no content in those columns. It won't expand or grow them to fit the space.

In case you have only one item in the row, `auto-fit` will expend it to fill the container width and it's unexpected.

## `place-content` and `place-items`

<https://www.youtube.com/watch?v=vNwoDkn7AIc>

When we use `display: grid`, the container is divided into cells which contain grid items. By default, grid items take up full width of its cell.

- `place-content`: 
  - shorthand for `align-content` and `justify-content`
  - align the whole grid items together
- `place-items`:
  - shorthand for `align-items` and `justify-items`
  - align the items within its cell