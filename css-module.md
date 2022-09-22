# CSS module

`import styles from './abc.module.scss`;

`styles` is an object that collects all the classes in `abc.module.scss` as its property. For example, in `abc.module.scss`, we have class header and footer like this

```css
.header{}
.footer{}
```

Then, when we `console.log(styles)`, we will have
{
	header: header_${suffix},
	footer: footer_${suffix},
}

## Tips with React

### Using with clsx (or classnames) package

- When the props is boolean, you have to declare the class in an object. If they are text, you can leave it outside an object

```js

const classes = clsx(styles.wrapper, styles[size], {
    [styles.disabled]: disabled,
});
```

### Pass className to children component

We often style the custom class in parent component, so we only need to concatenate it
```js
const classes = clsx(styles.wrapper, className, {
 //...
});
```

### Cascade

If both parent and children class is hashed (using CSS module), we write the CSS like normally

```scss
.menuList {
    width: 224px;

    .menuItem {
        margin-left: 0;
    }
}
```

If the children class comes from another library, we need to use `:global`

```scss
.menuList {
    width: 224px;

    :global(.fa) {
        margin-left: 0;
    }
}
```


