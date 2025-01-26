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