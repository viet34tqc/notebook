# Setup ESlint + Prettier + Typescript React

<https://cathalmacdonnacha.com/setting-up-eslint-prettier-in-vitejs>
<https://z1.digital/blog/eslint-guide-how-to-use-it-with-confidence>

Making a new project ready for development can be a little overwhelming, especially if you use React, Typescript, ESLint, and Prettier, but today I am going to help you set up your React project correctly in a simple way.

Usually, the problem is that every time you have to set up ESLint and Prettier you go to the web search for an article, and this article is probably not updated. Usually, they use a lot of plugins and non-default configurations.

We are going to use the basic configuration of ESLint and Prettier to have fewer headaches.

## What is ESLint?

ESLint is a JavaScript linting open source project and is used to find problems and syntax issues in your code, it will help us find broken logic that would be found only in run time.

## ESLint concept

<https://prateeksurana.me/blog/difference-between-eslint-extends-and-plugins/>
<https://stackoverflow.com/questions/53189200/whats-the-difference-between-plugins-and-extends-in-eslint>

### `plugins`

Plugins are published as npm modules with names in the format of *eslint-plugin-<plugin-name>*. It provides a set of rules that you can individually apply depending on your need. 

```json
{
  "plugins": ["my-awesome-plugin"] // The "eslint-plugin" suffix can be ommited
}
```

Adding a plugin does not mean that all the rules for the plugins will be applied automatically, you still need to individually apply each and every rule you would want to use with that plugin, with the `rules` object in your config file. 

But configuring each and every rule you want to use would be gruesome, wouldn't it be nice if you could just use some of the recommended set of rules by the plugin owner. This recommended set of rules is config file (or `preset`)

### `plugins` config file and `extends`

Config file can be shareable, which means it can be published to npm with names in the format *eslint-config-<config-name>*

To use shareable configuration file, you need to install it from npm then load it in your `extends` section after adding the plugin in the `plugins` section. For example, plugin like `prettier`, has seperate config file named `eslint-config-prettier` and you need to install before using it. By extending from a plugin config, we can get recommended rules without adding them manually.

In short, `plugins` provides rules but we have to add them manually, shareable config provides configurable rules in advanced for a plugin, so we don't have to add the rules by ourselves.

On the other hand, some plugins provide config file already, then you can add that config to `extends` without installing it from npm, like so: 

```json
{
  "extends": ["plugin:react/recommended"]
}
```

When you extend a configuration from a plugin, you may not need to declare that plugin in `plugins` if it is already defined in a configuration

Example: `eslint-plugin-react` already contains `plugins: [ 'react' ]`, hence this entry is not needed

## Setup

To install ESLint:

`yarn add -D eslint`

After installing the ESLint we can initialize the config file:

`npx eslint --init`

Or just install these packages:

`yarn add -D eslint-plugin-react@latest @typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest  eslint-plugin-react-hooks`

**This is optional**
`yarn add -D eslint-plugin-import eslint-import-resolver-typescript`

Create a `.eslintrc` file and paste this code

```json
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier"
  ],
  // Specified react version.
  "settings": {
    "react": {
      "version": "detect"
    }
  },
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": "latest",
    "sourceType": "module"
  },
  "plugins": ["react-hooks", "@typescript-eslint"], // No longer need to specify react here because "plugin:react/recommended defines it already
  "rules": {
    "react/react-in-jsx-scope": "off", // No longer need to import React from 'react'
    "@typescript-eslint/no-explicit-any": "off", // Temporarily disabled variable that has any type
    "react-hooks/exhaustive-deps": "warn" // This rule is disabled by default
  }
}
```

## Prettier

How does Prettier compare to ESLint/TSLint/stylelint, etc.?

Linters have two categories of rules:

**Formatting rules**: eg: max-len, no-mixed-spaces-and-tabs, keyword-spacing, comma-style…

Prettier alleviates the need for this whole category of rules! Prettier is going to reprint the entire program from scratch in a consistent way, so it’s not possible for the programmer to make a mistake there anymore

**Code-quality rules**: eg no-unused-vars, no-extra-bind, no-implicit-globals, prefer-promise-reject-errors…

Prettier does nothing to help with those kind of rules. They are also the most important ones provided by linters as they are likely to catch real bugs with your code!

*In other words, use Prettier for formatting and linters for catching bugs!*

To install Prettier:

`yarn add -D prettier eslint-config-prettier`

- `eslint-config-prettier`: Turns off all rules that are unnecessary or might conflict with Prettier, that could be  formatting-related ESLint rules. 

Some other tutorials tell you to use `eslint-plugin-prettier`, but according to <https://stackoverflow.com/questions/44690308/whats-the-difference-between-prettier-eslint-eslint-plugin-prettier-and-eslint/44690309#44690309>[this] and <https://prettier.io/docs/en/integrating-with-linters.html>[this], that prettier plugin takes care the linting as well by adding prettier rules to eslint, meanwhile we should let prettier do the formatting only.

After installing you have to create the prettierc file:

`touch .prettierrc`

At this point you have a blank .prettierrc file and a default eslintrc file, so the next step is to configure the eslintrc file.

Open your eslintrc file.

If you are going to use or you intend to use Jest in your project add the next lines to your 'env':

```json
{
    "env": {
        "browser": true,
        "es2021": true,
	"jest": true // Add this line!
    },
	...
}
```

To use prettier alongside with eslint you need to change the extends object (Implemented as above)

```json
{
	...
	"extends": [
	  "eslint:recommended",
	  "plugin:react/recommended",
	  "plugin:@typescript-eslint/recommended",
	  "prettier" // Add this line!
	],
	...
}
```

The main part of ESLint is the rules objects. Here you can put any rule you think is good for your project and team.

My basic configuration is this:

```json
{
	...
	"rules": {
        "react/react-in-jsx-scope": "off",
        "camelcase": "error",
        "spaced-comment": "error",
        "quotes": ["error", "single"],
        "no-duplicate-imports": "error"
  },
	...
}
```

If you want to learn more about ESLint rules you can check out rules page.

**This is optional**
The last thing to set up in ESLint is the eslint-import-resolver-typescript. Just add the settings key in the eslint configuration file.

```json
{
	...
	"settings": {
    "import/resolver": {
      "typescript": {}
    }
  }
}
```

Lastly, we are going to set up the `.prettierrc` file created at the begging of the article.

My basic configuration for `.prettierrc` file is:

```json
{
  "semi": false,
  "tabWidth": 2,
  "printWidth": 100,
  "singleQuote": true,
  "trailingComma": "all",
  "jsxSingleQuote": true,
  "bracketSpacing": true
}
```

But if you want to learn more check out the options page.

Finally, we have to add the scripts in the package.json:

```json
{
	"scripts": {
	  "lint": "eslint src/**/*.{js,jsx,ts,tsx,json}",
      "lint:fix": "eslint --fix src/**/*.{js,jsx,ts,tsx,json}",
      "format": "prettier --write src/**/*.{js,jsx,ts,tsx,css,md,json} --config ./.prettierrc"
    },
}
```

Basically:

- lint: will search for problems, but will not fix
- lint fix: will search and try to fix the problems.
- format: will call prettier to fix the code style.

At this point, you probably have a fully working ReactJS project with linting. If you encounter some error or problem don't hesitate in commenting below.

## Using eslint with Vite

First, you need to let Vite know about eslint. Vite will display error in the terminal if there is eslint error

`yarn install -D vite-plugin-eslint --save-dev`

Next, we need to integrate eslint with vite configuration

```js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import eslint from 'vite-plugin-eslint';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react(), eslint()],
});
```