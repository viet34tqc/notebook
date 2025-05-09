# Git Basic

## Reference

- <https://www.freecodecamp.org/news/gitting-things-done-book>
- <https://www.robinwieruch.de/git-team-workflow/>
- <https://www.git-tower.com/learn/git/ebook/en/desktop-gui/advanced-topics/rebase/>
- <https://www.atlassian.com/git/tutorials/merging-vs-rebasing>
- <https://dev.to/lydiahallie/cs-visualized-useful-git-commands-37p1>

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
#Pull the latest commit from master and merge into feature branch
git pull origin master:master
```

equal to

```bash
git checkout feature
git fetch origin master:master
git merge master
```

## Rebase

<https://www.freecodecamp.org/news/how-to-use-git-rebase>

Rebasing is changing the base of your branch from one commit to another making it appear as if you'd created your branch from a different commit. The commits of current branch will be moved on top of the commits from the branch you want to integrate into.

The primary reason for rebasing is to maintain a linear project history, without such automatic merge commit. For example, consider a situation where the main branch has progressed since you started working on a feature branch. You want to get the latest updates to the main branch in your feature branch, but you want to keep your branch's history clean so it appears as if you've been working off the latest main branch.

Here is the flow. Let's say, you want to integrate the changes from `branch B` into `branch A`.

- First, `branch A` will temporarily save all the latest commit on `branch A` that happened after the branching out point. It's like git stash
- Next, it applies all commits from `branch B` that we want to integrate. At this point, `branch A` and B look exactly the same
- Final, the new commits on `branch A` are reapplied - but on another position, on top of the integrated commits from `branch B` (they are re-based)

In result, you will have a straight line history for your project. However, rebase has a pitfall: **it rewrites the history**.
As you can see from the example above, the newest commits on `branch A` has the new parent commit. **It's like branch A is branching from newest commit from branch B**, not from the old commit of branch B anymore => Branch A's history has changed. So, another developer working on the latest commit on `branch A` might got trouble

**REBASE BEST PRACTICE**

- Do not use rebase on shared branch on remote repository (ex: *master*). You should use rebase only for cleaning up your local work (like your *feature* branch) then merge it into shared team branch
- Regularly fetch and rebase your local branches to stay up-to-date with the main branch. Conflicts become messy! Rebase early and often.

Git workflow

```bash
git checkout master
git pull

git checkout feature
git rebase master
git push --force-with-lease
```

<https://www.youtube.com/watch?v=vQgcl8VouLU>

### How to write commit

`git commit -am "WIP: something".`

WIP: Work In Progress - used to describe a project or task that is currently being worked on, but it not yet completed.

### Interactive rebase

Before we rebase `master` on to `dev`, we might want to modify the commit on `dev` branch first, like `drop` (remove unneeded commit) or `squash` (combine commits)

## Conflict when merge and rebase

<https://www.freecodecamp.org/news/resolve-merge-conflicts-in-git-a-practical-guide/>

When Git cannot perform an auto-merge because changes are in the same region, it indicates the conflicting regions with special characters. The character sequences are like this:

```
<<<<<<<
=======
>>>>>>>
```

There is a difference between rebase and merge when conflict appears

- Merge conflict

Everything between `<<<<<<<` and `=======` are your local changes. These changes are not in the remote repository yet. All the lines between `=======` and `>>>>>>>` are the changes from the remote repository or another branch

- Rebase conflict is opposite to rebase: Code in `<<<<<<<` and `=======` comes from other branch. The current changes on your local branch is pushed down
