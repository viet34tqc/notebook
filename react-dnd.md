# React DND

A drag-n-drop library

## Concept

### Item

When you drag or drop something, it's not a component or an item, it's called an item. 
Item is an object describing what is dragged, like: `{ cardId: 42 }`

### Type

Each item has a type. This is an unique string describing the item, like: `card`, `list`...
The drag source and drop target must have the same type.

### Monitors

The monitors let you update the component props in response to the drag and drop state changes. This is a method of `spec` object in `useDrag`hook.

## Hooks

### `useDrag`

Params:

- spec: A specification object or a function that creates a specification object.
- deps: A dependency array used for memoization. This behaves like the built-in `useMemo` React hook. The default value is an empty array for function spec, and an array containing the spec for an object spec.

Return Value:

- Collected Props: An object containing collected properties from the `collect` function. If no `collect` function is defined, an empty object is returned.
- DragSource Ref: A connector function for the drag source. This must be attached to the draggable portion of the DOM.
- DragPreview Ref: A connector function for the drag preview. This may be attached to the preview portion of the DOM.

```	jsx
import { useDrag } from 'react-dnd'

function Item({itemID}) {
  const [{ isDragging }, dragSource, dragPreview] = useDrag(() => ({
	// "type" is required. It is used by the "accept" specification of drop targets.
    type: 'BOX',
    // `item` is required.
    item: {itemID}
  	// Collect method passes the up-to-date value of `isDragging` props to this `Box` component directly.
  	// Methods of monitor: https://react-dnd.github.io/react-dnd/docs/api/drag-source-monitor
    collect: (monitor) => ({
      isDragging: monitor.isDragging()
    })
  }))

  return (
    {/* This is optional. The dragPreview will be attached to the dragSource by default */}
    <div ref={dragPreview} style={{ opacity: isDragging ? 0.5 : 1}}>
        {/* The drag ref marks this node as being the "pick-up" node */}
        <div role="Handle" ref={dragSource} />
    </div>
  )
}
```

### `useDrop`

Params:

- spec: A specification object or a function that creates a specification object.
- deps: A dependency array used for memoization. This behaves like the built-in `useMemo` React hook. The default value is an empty array for function spec, and an array containing the spec for an object spec.

Return Value:

- Collected Props: An object containing collected properties from the `collect` function. If no `collect` function is defined, an empty object is returned.
- DropTarget Ref: A connector function for the drop target. This must be attached to the drop-target portion of the DOM.

```	jsx
function Bucket() {
  const [{ canDrop, isOver }, drop] = useDrop(() => ({
    // The type (or types) to accept - strings or symbols
    accept: 'BOX',
    // Props to collect
    collect: (monitor) => ({
      isOver: monitor.isOver(),
      canDrop: monitor.canDrop()
    })
  }))

  return (
    <div
      ref={drop}
      role={'Dustbin'}
      style={{ backgroundColor: isOver ? 'red' : 'white' }}
    >
      {canDrop ? 'Release to drop' : 'Drag a box here'}
    </div>
  )
}
```


