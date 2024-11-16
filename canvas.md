# Canva

First, create a context

```js
const canvas = document.querySelector('#myCanvas') as HTMLCanvasElement;
const context = canvas.getContext('2d');
```

Start to draw: `context.beginPath()`
Stop draw: `context.endPath()`

## Add outline

```js
context.lineWidth
context.strokeStyle = 'yellow'
context.stroke()
```

## Add fill color

```js
context.fillStyle = 'yellow'
context.fill();
```

## Arc

```js
arc(centerX, centerY, radius, startAngle, endAngle, isAntiClockwise);
```

`isAntiClockwise`: means which direction we are going to draw this circle


```js
context.beginPath();
context.arc(200, 200, 93, 0, Math.PI * 2, true);
context.fillStyle = '#FF6A6A';
context.fill();

context.lineWidth = 20;
context.strokeStyle = 'yellow';
context.stroke();
```

## Border radius

Using `lineJoin`

```js
context.beginPath();
context.moveTo(100, 100);
context.lineTo(100, 300);
context.lineTo(300, 300);
context.closePath();
 
// the outline
context.lineWidth = 10;
context.strokeStyle = '#666666';
context.lineJoin = "round"; // Draw rectangle with rounded corner
context.stroke();
```

