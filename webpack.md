# Webpack

<https://viblo.asia/p/webpack-danh-cho-nguoi-moi-bat-dau-bJzKmo4Ol9N>
<https://www.youtube.com/watch?v=TOb1c39m64A>
<https://fullstack.edu.vn/blog/phan-1-tao-du-an-reactjs-voi-webpack-va-babel.html>

Webpack treats every file as the modules. Based on the connection between files (`import` and `export`), it builds a dependency graphs and use it creates a single (or multiple) bundler file

## Packages

- `webpack-dev-server`: create a live reload dev server

```js
// Basic config
// run webpack server
devServer: {
	static: './dist', 
},
```
From v4.0, hot module replacement is enable by default.

- `@babel/core`: convert modern JS to older version
- `babel-loader`: a loader of webpack, make webpack work with babel
- `@babel/preset-env`: In Babel, a preset is a set of plugins used to support particular language features. @babel/preset-env takes any target JS versions you've specified and transpile the code to that target.

```json
{
  "targets": "> 0.25%, not dead"
}

{
  "targets": {
    "chrome": "58",
    "ie": "11"
  }
}
```

When no targets are specified: Babel will assume you are targeting the oldest browsers possible. For example, @babel/preset-env will transform all ES2015-ES2020 code to be ES5 compatible.
```json
{
  "presets": ["@babel/preset-env"]
}
```
Babel recommends setting targets to reduce the output code size.

- `@babel/preset-react`: convert jsx to js.
```json
{
    "presets": [
        "@babel/preset-env",
        "@babel/preset-react"
    ]
}
```

## Init

- npm init -y
- yarn add -D webpack webpack-cli

## Configure

- touch webpack.config.js
- Basic configure:

```javascript
// Require a nodejs module named 'path'
// 'path' handle the file name, file link
// Nodejs has lots of default modules, such as: 'http', 'https', 'path'...
const path = require('path');

module.exports = {
	entry: './src/index.js', // input file
	output: {
		filename: 'main.js',
		path: path.resolve(__dirname, 'dist'), // output dir
	},
};
```

Must have:
`entry`: can be multiple entry
`output`: only one output
`loader`: By default, Webpack can only understand JS file. `loader` helps webpack understand other files.
  - test: file type
  - use: loader name, can be array
`plugins`: other things that loader can't do like optimize bundle, manage assets
`mode`: 3 modes `development`, `production` and `none`. Default is `production`


## Run

Run `npx webpack` to start bundle the files in src folder. There would be a file named main.js in the dist folder

## Watch mode

`npx webpack --watch`

Watch the changes and still need to reload manually

## devServer

Auto reload

## Hot module replacement

<https://www.youtube.com/watch?v=yR25JoybTxo>
Make the change without reload

## Developtment

To check source map of the errors
`npx webpack --watch --mode development`

## Code splitting

- Why: to remove duplicated import. If 2 files import the same module, that module will be extract from 2 files

```javascript
optimization: {
	splitChunks: {
		chunks: 'all',
	},
},
```

## Alias

Must ensure Webpack to understand
```javascript
resolve: {
	alias: {
		'@components': path.resolve(__dirname, '.src/components')
	}
},
```

## Loader

- Why: Preprocess files as you import or load them
- Structure:

```js
output: {...},
module: {
	rules: [
		{
			'test': /\.(js|jsx)$/i, // file extensions
			use: {
				loader: 'babel-loader', // loader name
				options: {
					presets: ['@babel/preset-env'],
				},
			},
		},
		{
      test: /\.css$/i,
      use: ['style-loader', 'css-loader', 'sass-loader'],
  	},
	]
}
```

Some popular loaders:
- sass, sass-loader: import scss file in js files
- postcss-loader: add prefix to css using browserlist
- css-loader and style-loader: import css files in js files

## Plugin

- HtmlWebpackPlugin: generate an production HTML5 file (in `build` folder) for you that includes all your webpack bundles in the body using script tags
```js
...
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  ...
  // Chứa các plugins sẽ cài đặt trong tương lai
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html"
    })
  ]
};
```
