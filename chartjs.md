# ChartJS

<https://www.youtube.com/channel/UCojXvfr41NqDxaPb9amu8-A>

## Concept

### `labels`

This is array of XAxes values
```js
const data = {
	labels: totalCases.map(c => c.date),
};
```

### `datasets`

This is YAxes values
```js
const data = {
	datasets: [
		{
			data: totalCases.map(c => c.case), // This is YAxes values definition
		},
	],
};
```

### `options`

### `ticks`

### `legends`

These are the label above the chartt

## Tips

### How to format yAxes value

<https://www.youtube.com/watch?v=czt31ENbr2I>
```js
options = {
	y: {
		ticks: {
			// This callback will override the default format
			callback: function (label, index, labels) {
				return label / 1000 + 'k';
			},
		},
	},
}
```

### Hiding vertical and horizontal grid line

```	js	
scales: {
	x: {
		grid: {
			display: false,
		},
	},	
	y: {
		grid: {
			display: false,
		},
	},		
},		
```
