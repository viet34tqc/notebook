# Git Basic

## Reference

<https://www.git-tower.com/learn/git/ebook/en/desktop-gui/advanced-topics/rebase/>
<https://www.atlassian.com/git/tutorials/merging-vs-rebasing>
<https://dev.to/lydiahallie/cs-visualized-useful-git-commands-37p1>

## Merge:

There are two cases of merge:
- fast-foward
This is the simpler one. One of the two branches, let's say it is `branch A`, doesn't have any new commits since the branching happened.
In this case, performing the integration is dead simple: Git adds all the commits of the other branch (`branch B`) on top of the `branch A`. This is 'fast-foward' merge. Both branch has exact the same history.

- no fast-foward
Normally, however, both branches move foward individually.
To make an integration, Git will have to create a new commit that contains all the changes between them - the merge commit. Instead of being created by a developer, this commit is created automatically by Git.

Command:
```bash
git checkout feature
// Pull the latest commit from master and merge into feature branch
git pull origin master:master
```
equal to
```bash
git checkout feature
git fetch origin master:master
git merge master
```

## Rebase

If you prefer the project's histoy as a straight line, without such automatic merge commit, you can think of using rebase.
Let's say, you want to integrate the changes from `branch B` into `branch A`.
- First, `branch A` will temporarily save all the latest commit on `branch A` that happened after the branching out point. It's like git stash
- Next, it applies all commits from `branch B` that we want to integrate. At this point, `branch A` and B look exactly the same
- Final, the new commits on `branch A` are reapplied - but on another position, on top of the integrated commits from `branch B` (they are re-based)
In result, you will have a straight line history for your project. However, rebase has a pitfall: **it rewrites the history**.
As you can see from the example above, the newest commits on `branch A` has the new parent commit. Their history has changed. So, another developer working on the latest commit on `branch A` might got trouble

Therefore, you should use rebase only for cleaning up your local work - but never to rebase commits that have already been **published**.

Command:
```bash
git checkout feature
git pull --rebase master
```

Git workflow
Rebase `feature` onto the tip of `master` then merge `feature` into `master`

### Interactive rebase

Before we rebase `master` on to `dev`, we might want to modify the commit on `dev` branch first, like `drop` (remove unneeded commit) or `squash` (combine commits)
