# Monorepo

## Reference

- <https://monorepo.tools/>

## Definition

Monorepo allows us to include multiple projects into a single repository. The opposite of monorepo is multirepo, which is what most of us do when starting a project

Going from multi to monorepo is a matter of moving all your projects into a single repository.

## Benefits

- Code sharing: all dependancies are sharing and hosted in the same repo. For example: admin dashboard and client app using the same component library packages, shared types between server and client (**Most important**)
- Single source of truth: one version of every dependency means there are not versioning conflicts and no dependency hell 
- Consistency: all the packages will be enforced to use the same code standards
- Visibility: everyone are informed of any updates in all packages => better collaboration

## Drawbacks

- Hard to maintain when the codebase grows

## When to use

- When a project has a core package surrounded with various add-ons; this is for example the case for Babel.

## When not to use

- When you need to limit the access control based on the specific roles. For example, in the monorepo, there is a private package and only some members of the team can get access to it

## How

We have several options to set up monorepo, such as using libraries like `Nx`, `Lerna`, and so on. Alternatively, we can take advantage of `workspace`, which is a built-in feature of npm, pnpm and yarn.

Tools like `Nx` is very similar to `workspace` from  `yarn` or `npm`, but it provides better DX, faster build time and many other features

## Notes

- A provider needs to tell the consumers where are the entry point declaration so the consumers can notice it. This is a must. There are 2 ways to do it in `package.json` file
	- declare "types" and "module"
	```json
	"types": "dist/index.d.ts",
	"module": "dist/index.js",
	```
	- declare `exports`
	```json
	"exports": {
	  ".": {
	    "types": "./dist/index.d.ts",
	    "import": "./dist/index.js"
	  }
	}
	```
- The best way to develop the monorepo packages is build the consumer and import the built output. 
	- Faster type checking in consumers (no need to recompile models every time)
	- Simulates real-world package consumption
	- Catches build issues early
	- Works with all tools (bundlers, test runners, etc.) without special config
	- Cleaner separation of concerns
	Don't use `path` to import provider package from consumers because it's slower
- `tsc` and `tsx`
  - tsc (TypeScript Compiler): Used for packages that served as consumers, needs to output compiled files for other packages to import. For examples, shared package
	- Compiles TypeScript to JavaScript files
	- Doesn't execute code, just transpiles it
	- Used for building your project (outputs .js and .d.ts files)
	- In watch mode (--watch), it recompiles when files change but doesn't run anything
  - tsx: Used for packages that runs immediately. For example, server package.
	- Runs TypeScript files directly without a separate build step
	- Transpiles on-the-fly in memory and executes immediately
	- Like ts-node but faster and better ESM support

## References

Declare "composite: true" in tsconfig.json first:

```json
{
	"compilerOptions": {
		"composite": true
	}
}
```

reference to the **tsconfig file** of the consumer modules

```json
"references": [{ "path": "../models" }]
```

When you do that ts system understands that you are referencing to '../models/tsconfig.json'. If you use another tsconfig file, you should use that file instead

```json
"references": [{ "path": "../models/tsconfig.build.json" }]
```

## Lerna

- `p install -D lerna`
- `pnpm dlx lerna init` => create a lerna.json
- `pnpm lerna run build`
- `p lerna run build,lint,test,check --stream`: Run multiple tasks with detailed output
- `p lerna run build,lint,test,check --scope={name_of_module}`: Run multiple tasks for a particular module
- `p lerna run build,lint,test,check --since={name_of_ref_like_main_branch}`: Run tasks on packages that have been changed since the point you branch off from main

## Nx

- `pnpm dlx nx init`