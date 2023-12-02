# Monorepo

## Reference

- <https://monorepo.tools/>

## Definition

Monorepo allows us to include multiple projects into a single repository. The opposite of monorepo is multirepo, which is what most of us do when starting a project

Going from multi to monorepo is a matter of moving all your projects into a single repository.

## Benefits

- Simple dependancy management: all dependancies are sharing and hosted in the same repo
- Consistency: all the packages will be enforced to use the same code standards
- Visibility: everyone are informed of any updates in all packages => better collaboration
- Single source of truth: one version of every dependency means there are not versioning conflicts and no dependency hell

## When to use

When a project has a core package surrounded with various add-ons; this is for example the case for Babel.

When you need to publish multiple packages and want to avoid your contributors having to open PRs on many separate repositories whenever they want to make a change. This is for example the case for Jest.

## Drawbacks

- Hard to maintain when the codebase grows

## How

We have several options to set up monorepo, such as using libraries like `Nx`, `Lerna`, and so on. Alternatively, we can take advantage of `workspace`, which is a built-in feature of npm, pnpm and yarn.

Tools like `Nx` is very similar to `workspace` from  `yarn` or `npm`, but it provides better DX, faster build time and many other features