# D3.js

### Reference

**MUST READ**: <https://www.d3indepth.com/> 
<https://blog.griddynamics.com/using-d3-js-with-react-js-an-8-step-comprehensive-manual/>
<https://www.youtube.com/watch?v=2LhoCfjm8R4>
<https://www.youtube.com/watch?v=2Y7qLPz4Iu4&list=PLzCAqE_rafAc_2QWK8ii16m2ju4qhYAJT>
<https://www.youtube.com/watch?v=Y-ThTzB-Zjk&list=PLDZ4p-ENjbiPo4WH7KdHjh_EMI7Ic8b2B&index=2>

## What is d3.js library

It is a library for manipulating document based on data. It means that you can manipulate your DOM and connect the DOM to the data given.

=> d3.js allows your to automatically render and re-render your HTML & SVG elements on your page based on the data

## Basic API

### `select`

Select an element. You can select by tag, class, id.
```ts
const svgRef = useRef<SVGSVGElement | null>(null);
useEffect(() => {
	select(svgRef.current)
});
```

### `selectAll`

Select all the elements
```js
selectAll('rect')
```

### `append`

Append another element into selected element
```js
select(svgRef.current).append('rect')
```

### `attr`

Add attribute to the element
```js
select(svgRef.current).append('rect').attr('width', 100).attr('height', 100)
```

### `style`

Add inline style to the element
```js
select(svgRef.current).append('rect').attr('width', 100).attr('height', 100).style('fill', 'blue');
```

### `Array.map()` or `selectAll().data(data)`

<https://stackoverflow.com/questions/44354851/array-map-vs-d3-selectall-data-enter>

TL,DR: **DON'T USE `map`**, just go with `selectAll().data(data)`. `map()` only helps when first draw the elements. Then, if you want to do something like transition or update the data, you will need to re-create all the elements and make the changes.

## Data join

A data join creates a corresponding between an array of data and a selection of HTML or SVG elements

- HTML elements are added or removed such that each array element has a corresponding HTML elements
- Each HTML/SVG element is postioned, sized, styled according to the value of its corresponding array element

Let's say we have this data
```js
const data = [
	{ width: 100, height: 200, color: 'blue' },
	{ width: 100, height: 150, color: 'red' },
	{ width: 100, height: 120, color: 'green' },
];
```
and the HTML:
```jsx
<svg ref={svgRef}>
	<rect />
	<rect />
	<rect />
</svg>
```
Then, we want to render the svg based on this data. In this case, we can use `data()` method
```js
const selection = select(svgRef.current);
selection
	.selectAll('rect')
	.data(data)
	.attr('width', (d) => d.width)
	.attr('height', (d) => d.height)
	.attr('fill', (d) => d.color);
```

However, the given data can be changed, not 3 but 300. Therefore, we cannot predefined all the `rect` elements in the `svg` based on that data.
This is exactly the problem that `enter` resolved. It adds the placeholder nodes for each data that has no corresponding DOM element in the selection. The inverse to `enter` is `exit`. `exit` compares the existing DOM elements to the data. If the number of DOM elements is bigger than the current data value, those elements would be selected to be removed

First, let's remove the `rect` defined inside the `svg`. Then, we change our code like this
```js
selection
	.selectAll('rect')
	.data(data)
	.enter()
	.append('rect')
	.attr('width', (d) => d.width)
	.attr('height', (d) => d.height)
	.attr('fill', (d) => d.color);
```

D3 only works with the missing `rect`. If you predefined any `rect` in the `svg`, the code above won't be applied

### `join`

Prior version 5 off D3, using `enter` and `exit` was the only for data join. However, it's still difficult to understand the concept of `enter` and `exit`. Fortunately, from version 5, data join is much easier to learn with `join`. Here is another version of the example above using `join`

```js
selection
	.selectAll('rect')
	.data(data)
	.join('rect') // This is where `rect` elements are added or removed corresponding to the data
	.attr('width', (d) => d.width)
	.attr('height', (d) => d.height)
	.attr('fill', (d) => d.color);
```

## Scale

Let's say we have some data, which is so large or so small, such as financial data. The data could be up to milions and passing those data into the bar is not resonable. That's when `scale` comes in. In another way, it helps to convert our raw data into number | string that can be fit into the chart

`scale` functions return another function which takes raw data as argument. Then, the result of this function is the scaled data

Install these package first: `yarn add d3-scale @types/d3-scale d3-array @types/d3-array`

API:
- `scaleLinear().domain([min, max]).range([min, max])`
`domain([min, max])`: set the scale's domain. If our data value ranges from 0 to 1000, we can set `domain([0, 1000])`.
`range([min, max])`: the data value is scaled/mapped to this range. 

However, the data can be remove or changed, so does the max number. So, we need to use `max` from `d3-array` to recalculate the max number if the data is changed. What it returns is the current maximum data value.
`domain([min, max(data, d => d.number)])`

- `scaleBand().domain(array_of_label).range([min, max]).paddingInner(0.1)`
By default, given a domain of a certain length (an array or an interator) and a continuous range `[min, max]`, it will divide the range **evenly** between the elements of the domain.

`scaleBand().bandwidth` returns the width of each elements. Then we can use `paddingInner` to add spacing between elements

- `scaleTime`
`scaleTime` is similar to `scaleBand` except the `domain` is expressed as an array of dates

### Update scale

Call the `domain(newData)` again: `scale.domain(newData)`

## Axis

Install package: `yarn add d3-axis @types/d3-axis`
Axis is render as a `g` element

API: `axis_position`, with `position` is `left`, `top`, `right`, `bottom`. For example, you can set yAxis is `axisLeft` and xAxis is `axisBottom`

Before create axis, We need to define `yScale` and `xScale` first. The `ticks` are calculated based on the scales.

```js
const yScale = scaleLinear()
	.domain([0, maxValue])
	.range([dimension.chartHeight, 0]);
const xScale = scaleBand()
	.domain(data.map(d => d.name))
	.range([0, dimension.chartWidth])
	.paddingInner(0.05);
const yAxis = axisLeft(yScale)
	.ticks(3)
	.tickFormat(d => `$${d}`);
const xAxis = axisBottom(xScale);
```

## Parsing CSV data

use `d3.csv(Url)`

```js
d3.cvs(url).then(data => {})
```