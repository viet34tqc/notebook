# Sass

## Why use sass over css

## Use `@use` and `@foward` instead of `@import`

<https://www.youtube.com/watch?v=CR-a8upNjJ0&list=PL4-IK0AVhVjNiZpLY-3ILQFyRF0Bs914J&index=3&ab_channel=KevinPowell>

The problem with `@import` is that the any variables defines in sass files is globals. When you use it, you wouldn't know where it's defined. Other than that, the variable name between files can be conflicted

`@use` works the same as `@import`, where it includes css class into main file but variables. If you want to use variabe from other file you need to use `@use` first then access it with the file name

```scss
// If you aren't using `as` you need to use `variables` to access 
// Or you can use `@use './variables' as *` to use $font-size without namespace
@use './variables' as var
.button {
	font-size: var.$font-size
}

@use './variables' as *
.button {
	font-size: $font-size
}
```

If you have multiple files that include variable defination, using `@use` could be cumbersome as you wil have to write `@use` for each file. So we can use `@foward` to collect all the files into single name. `@foward` is like an index file to re-export other components in React.

```scss
// /var/_index.scss
@use './fonts'
@use './colors'

// Usage
@use ./var;
.button {
	font-size: var.$font-size;
}
```

## For loop

```scss
@for $i from 1 through 12 {
  .margin-#{$i} {
    margin-top: $i * $base-size;
  }
}
```

## Array, object and iteration

```scss
$base-size: 1rem;

$sizes: (
  'size-1': $base-size * 0.25,
  'size-2': $base-size * 0.5,
  'size-3': $base-size * 0.75,
  'size-4': $base-size,
);

$sides: inline, inline-start, inline-end, block, block-start;

@each $key, $value in $sizes {
  $number: string.slice($key, 6);

  .padding-#{$number} {
    padding: $value;
  }
}
```

## Mixins

It's like function

Use `@mixin` to declare and `@include` to execute.

```scss
@use 'sass:map';

$breakpoints: (
  sm: 40em,
  md: 60em,
);

@mixin mq($key) {
  $size: map.get($breakpoints, $key);
  @media (min-width: $size) {
    @content;
  }
}

// Usage
.button {
	@include mq(sm) {
		font-size: 1em;
	}
}
```

## Folder structure

<https://www.youtube.com/watch?v=gP8yFWCTr7Q&list=PL4-IK0AVhVjNiZpLY-3ILQFyRF0Bs914J&index=4&ab_channel=KevinPowell>

Because we are going to use `@use`, each file includes a kind of variables
/sass
	/variables
		/colors
		/fonts
		/mixins
		/index: @foward other file
	/base
		/reset
		/typo
		/index
	/components
	/layouts
		/home
