# Setup

Reference: 

- <https://www.youtube.com/watch?v=H91aqUHn8sE>
- <https://khalilstemmler.com/blogs/typescript/node-starter-project/>
- <https://khalilstemmler.com/blogs/tooling/prettier/>

## `tsconfig.json`

```json
{
	"compilerOptions": {
		"target": "ES2021",
		"module": "commonjs",
		"lib": ["ES2021"],
		"allowJs": true,
		"outDir": "dist",
		"rootDir": "src",
		"strict": true,
		"esModuleInterop": true
	}
}

```

`package.json`:

```json
{
	"scripts": {
		"start": "nodemon",
		"build": "rimraf ./dist && tsc",
		"start-prod": "npm run build && node dist/index.js",
		"lint": "eslint . --ext .ts",
		"prettier": "prettier --config .prettierrc src/**/*.ts --write",
		"pulldb": "npx prisma db pull"
	},
	"devDependencies": {
		"@types/node": "^18.11.19",
		"@typescript-eslint/eslint-plugin": "^5.51.0",
		"@typescript-eslint/parser": "^5.51.0",
		"eslint-config-prettier": "^8.6.0",
		"eslint-plugin-prettier": "^4.2.1",
		"eslint": "^8.33.0",
		"nodemon": "^2.0.20",
		"rimraf": "^4.1.2",
		"ts-node": "^10.9.1",
		"typescript": "^4.9.5"
	}
}

```

## Setup eslint amd prettier

Prettier package is needed for husky and prettier the whole project.

`eslintrc`

```json
{
	"root": true,
	"parser": "@typescript-eslint/parser",
	"plugins": ["@typescript-eslint", "prettier"],
	"extends": [
		"eslint:recommended",
		"plugin:@typescript-eslint/eslint-recommended",
		"plugin:@typescript-eslint/recommended",
		"prettier"
	],
	"rules": {
	    "prettier/prettier": 2 // Means error
	}
}
```

`.eslintignore`

```json
node_modules
dist
```

`prettierrc`

```json
{
	"semi": true,
	"trailingComma": "none",
	"singleQuote": true,
	"printWidth": 80
}
```

## Husky

`yarn install -D husky`

First, you need to *prepare* (install) husky before using it.

There are two cases when installing husky

## `.git` and `package.json` are on the same folder

Add this to `package.json`

```json
"scripts": {
		...
    "prepare": "cd ../ && husky install ./frontend/.husky"
},
"husky": {
    "hooks": {
      "pre-commit": "npm run prettier && npm run lint"
    }
}
```

Run `yarn prepare`

## `package.json` is in nested folder

<https://scottsauber.com/2021/06/01/using-husky-git-hooks-and-lint-staged-with-nested-folders/>

Add this to `package.json`

```json
"scripts": {
		...
    "prepare": "cd ../ && husky install ./frontend/.husky"
}
```

Run `yarn prepare`

Then create a pre-commit file with no file extension under the .husky folder with the following contents

```
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

cd ./backend && npm run prettier && npm run lint
```

`backend` is the folder installing husky.