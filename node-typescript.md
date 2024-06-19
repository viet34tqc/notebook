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
