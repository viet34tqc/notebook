# Webpack

## Init

- npm init -y
- yarn add -D webpack webpack-cli
- create

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

## Run
Run `npx webpack` to start bundle the files in src folder. There would be a file named main.js in the dist folder

## Watch mode
`npx webpack --watch`

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
```javascript
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
- sass-loader: compile sass to css
- css-loader and style-loader: import css files in js files