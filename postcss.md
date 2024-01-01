# PostCSS

<https://www.youtube.com/watch?v=Kn2SKUOaoT4>

It's like the webpack with multiple plugins for different purpose with CSS, ex: import css from other file like sass, minify css, convert modern css to what css browser can understand...

You can use PostCSS with simple static project, or vite project

## Installation and Setup

- Main package: postcss
- Plugin packages: 
	+ cssnano: minify css
	+ postcss-import: import other css file like sass
	+ postcss-preset-env: convert modern css

First, you need to create a `postcss.config.js` file

With normal static project:

```js
const cssnano = required('cssnano');
module.export = {
	plugins: [
		cssnano({
		  preset: 'default',
		}),
	]
}
```

With Vite: Vite use ES Module and doesn't allow CommonJS module, which use `require` and `module.export`

```js
import cssnano from 'cssnano';

export default {
  plugins: [
    cssnano({
      preset: 'default',
    }),
  ],
};
```

## Config example

```js
import cssnano from 'cssnano';
import postcssImport from 'postcss-import';
import postcssNesting from 'postcss-nesting';
import postcssPresetEnv from 'postcss-preset-env';

export default {
  plugins: [
    postcssImport(),
    // If we set the `postcssPresetEnv` stage to level 3, it supposes that we are using the lastest browser and leave as it is as the @layer behaves. Some older version like safari 16 cannot understand this. So we need to use postcssNesting
    // If we set the stage to level 2 postcssPresetEnv, we don't need postcssNesting because postcssPresetEnv will convert it older css that all major browsers can understand
    postcssNesting(),
    cssnano({
      preset: 'default',
    }),
    postcssPresetEnv({
    	// If we are not using features in stage2+ (like layers) we can remove this option (just use postcssPresetEnv() with empty options object)
      // At the time of writting this code, stage 2 doesn't preserve @layer and it will convert to normall css by adding specificity :not(#\#)
      // stage 3 preserve @layer as it is
      // @layer is supported in from safari 15 and above, so we are safe to use.
      // https://preset-env.cssdb.org/features/#stage-2
      stage: 3,
    }),
  ],
};

```

## Using `postcss-import` with `@layer`

<https://stackoverflow.com/questions/67342849/import-some-attribute-not-work-in-layer-tailwindcss>

```css
@import 'baz.css' layer(baz-layer);
```

With tailwind: 

```css
@import 'tailwindcss/base' layer(Base);
@import './base/typography.css' layer(Base);

@import 'tailwindcss/components' layer(Components);

@import 'tailwindcss/utilities' layer(Utilities);

@layer Base {
  #root {
    @apply some-styles;
  }
}
```
