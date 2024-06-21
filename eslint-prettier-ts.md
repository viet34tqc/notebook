# Setup ESlint + Prettier + Typescript React

- <https://cathalmacdonnacha.com/setting-up-eslint-prettier-in-vitejs>
- <https://z1.digital/blog/eslint-guide-how-to-use-it-with-confidence>
- <https://prateeksurana.me/blog/difference-between-eslint-extends-and-plugins/>
- <https://stackoverflow.com/questions/53189200/whats-the-difference-between-plugins-and-extends-in-eslint>

Making a new project ready for development can be a little overwhelming, especially if you use React, Typescript, ESLint, and Prettier, but today I am going to help you set up your React project correctly in a simple way.

Usually, the problem is that every time you have to set up ESLint and Prettier you go to the web search for an article, and this article is probably not updated. Usually, they use a lot of plugins and non-default configurations.

We are going to use the basic configuration of ESLint and Prettier to have fewer headaches.

## What is ESLint?

ESLint is a JavaScript linting open source project and is used to find problems and syntax issues in your code, it will help us find broken logic that would be found only in run time.

## ESLint concept

### `plugins`

Plugins are published as npm modules with names in the format of *eslint-plugin-<plugin-name>*. It provides a set of rules that you can individually apply depending on your need. 

```json
{
  "plugins": ["plugin-name"] // The "eslint-plugin" suffix can be ommited
}
```

Adding a plugin does not mean that all the rules for the plugins will be applied automatically, you still need to individually add the rules you want in the `rules` section in your config file. 

But configuring the rules one by one would be tedious, it would be nicer if you could just use predefined set of rules by the plugin owner. This recommended set of rules is config file (or `preset`)

### `extends`

The config file I mentioned above can be shareable, which means it can be published to npm with names in the format *eslint-config-<config-name>*

To use shareable configuration file, you need to install it from npm then load it in your `extends` section after adding the plugin in the `plugins` section. For example, plugin like `prettier`, has seperate config file named `eslint-config-prettier` and you need to install before using it. By extending from a plugin config, we can get recommended rules without adding them manually.

In short, `plugins` section provides rules but we have to add them manually, `extends` section provides predefined rules of a plugin, so we don't have to add the rules by ourselves.

On the other hand, some plugins provide config file already, then you can add that config to `extends` without installing it from npm. For example, `eslint-plugin-react` includes predefined configs such as `recommended`, `all`, `jsx-runtime`.

To use an imported config from a plugin, we follow the syntax: `plugin:[name-plugin]/[name-config]`

```json
{
  // Use the config 'recommended' from eslint-plugin-react
  "extends": ["plugin:react/recommended"]
}
```

`plugins` is not needed in your own config, if it is already defined in a configuration, that you extend from by extends.

Example:

<https://github.com/jsx-eslint/eslint-plugin-react/blob/a944aa519246d1f4c7a494ebd40e5b2c23601b77/index.js#L20>[`eslint-plugin-react`] already contains plugins: `[ 'react' ]`, hence this entry is not needed anymore in own config and plugin rules can be used directly. However, you need to check the source code of the plugin carefully. So, to be on the safe side, just add that plugin to `plugins` section

Besides, config from shareable configuration, plugin configuration, `extends` can also accept:

- "eslint:recommended"
- "eslint:all"
- Another configuration file, like "./my/path/.eslintrc.js"

### `override`


Tell ESLint what to do when it comes across a specific file type. This is used when you want to have a seperate configuration for a file type. Below is just a demo, practically, we can define the typescript configuration without `overrides`

```js
overrides: [
  {
    files: ['*.ts', '*.tsx'],
    extends: [
      'plugin:@typescript-eslint/eslint-recommended',
      'plugin:@typescript-eslint/recommended',
      'plugin:@typescript-eslint/recommended-requiring-type-checking',
    ],
    parser: '@typescript-eslint/parser',
    parserOptions: {
      project: true,
    },
    plugins: ['typescript-eslint'],
    rules: {
    },
  },
],
```

## Setup ESlint

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
    "browser": true, // Enables browser global variables (like window, document, etc.)
    "es2021": true // Enables ES2021 globals and syntax.
  },
  "extends": [
    "eslint:recommended", // Uses the recommended rules from ESLint
    "plugin:react/recommended", // Uses the recommended rules for React.
    "plugin:@typescript-eslint/recommended", // Allow ESLint understand TS files. It contains a parser and a bunch of recommended rules for working with Typescript
    "prettier" // Turns off all rules that are unnecessary or might conflict with Prettier, that could be formatting-related ESLint rules. 
  ],
  // Specified react version.
  "settings": {
    "react": {
      "version": "detect"
    }
  },
  "parser": "@typescript-eslint/parser", // This specifies the parser to use for linting TypeScript code. @typescript-eslint/parser parses TypeScript code so ESLint can understand it.
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true // Enables linting of JSX syntax.
    },
    "ecmaVersion": "latest", // Specifies the version of ECMAScript syntax you want to use
    "sourceType": "module" // Specifies the type of modules ("module" for ES6 modules).
  },

  // This section specifies additional plugins to use.
  "plugins": ["react-hooks", "@typescript-eslint"], // No longer need to specify react here because "plugin:react/recommended defines it already

  // This section specifies custom rules and their settings.
  "rules": {
    "react/react-in-jsx-scope": "off", // No longer need to import React from 'react'
    "@typescript-eslint/no-unused-vars": ["warn"],
    "@typescript-eslint/no-explicit-any": "off", // Temporarily disabled variable that has any type
    "react-hooks/exhaustive-deps": "warn" // This rule is disabled by default
  }
}
```

Touch `.eslintignore`

```json
node_modules
dist
```

## Prettier

How does Prettier compare to ESLint/TSLint/stylelint, etc.?

Linters have two categories of rules:

**Formatting rules**: eg: max-len, no-mixed-spaces-and-tabs, keyword-spacing, comma-style…

Prettier alleviates the need for this whole category of rules! Prettier is going to reprint the entire program from scratch in a consistent way, so it’s not possible for the programmer to make a mistake there anymore

**Code-quality rules**: eg no-unused-vars, no-extra-bind, no-implicit-globals, prefer-promise-reject-errors…

Those rules are handled by ESLint. Prettier does nothing to help with those kind of rules. They are also the most important ones provided by linters as they are likely to catch real bugs with your code!

*In other words, use Prettier for formatting and linters for catching bugs!*

To install Prettier:

`yarn add -D prettier eslint-config-prettier`

- `eslint-config-prettier`: Turns off all rules that are unnecessary or might conflict with Prettier, that could be formatting-related ESLint rules. 

Some other tutorials tell you to use `eslint-plugin-prettier`, but according to <https://stackoverflow.com/questions/44690308/whats-the-difference-between-prettier-eslint-eslint-plugin-prettier-and-eslint/44690309#44690309>[this] and <https://prettier.io/docs/en/integrating-with-linters.html>[this], that prettier plugin takes care the linting as well by adding prettier rules to eslint, meanwhile we should let prettier do the formatting only.

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

After installing you have to create the prettierc file:

`touch .prettierrc`

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

## Setup husky

<https://typicode.github.io/husky/get-started.html>

Allow us to do some tasks like testing, lint, format before commit

- First, Install husky: `pnpm add --save-dev husky`
- Run `pnpm exec husky init` or `npx husky init`. It creates a `pre-commit` file in `.husky/` folder 
- Update `pre-commit` file with your scripts

**Project Not in Git Root Directory**

For example, you have a folder `backend` and `frontend` in the root folder and `frontend` has a husky setup

In `package.json` in frontend: `"prepare": "cd .. && husky frontend/.husky"`
In your hook script, change the directory back to the relevant subdirectory:
```
# frontend/.husky/pre-commit
cd frontend
npm test
```

## Setup `lint-staged` with `husky`

Do something with the staged files only. Combining `lint-staged` with `husky` allows us to implement some task for *staged files* right *before commit*

- Install `lint-staged`: `pnpm i lint-staged`
- Create `lint-staged.config.mjs` or `lint-staged.config.js`, depending on if your project supports ESM or not

```js
const config = {
  '*.{js,jsx,ts,tsx}': ['pnpm lint', 'pnpm format', 'git add .'], // If your `pnpm lint` command also fixed some bug, we should staged all the fixed file by using `git add .` 
}
export default config
```

`NextJS` using `next lint` that requires a little bit different config

```js
import path from 'path'

const buildEslintCommand = (filenames) =>
  `next lint --fix --file ${filenames
    .map((f) => path.relative(process.cwd(), f))
    .join(' --file ')}`

const config = {
  '*.{js,jsx,ts,tsx}': [buildEslintCommand, 'pnpm format', 'git add .'],
}
export default config
```

In `pre-commit` file in `.husky` folder, we add this comand `pnpm lint-staged`

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