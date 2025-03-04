# GIT

This file will contain all the main git commits and ideas for a basic development
experience

## Merge

The idea of merging two branches is to find `they're last matching ancestor`
and in a form of a `new commit` set both branches as the same one, having both
changes.
We can `fast forward a merge` when the last commit on the main branch be the
common ancestor of both branches. `No merge commit is made.

## Rebase

Rebase is ment to do kind of the job of merge but without creating a merge
commit to it. It will do kind of a `fast forward` merge that the new commits
on the working branch will be placed `upfront` of the target branch.
It just `moves` the merge base from the source commit of the branch creation to the last
commit on the target branch.

### > [!WARNING]

> You should never rebase a public branch (like main) onto anything else. Other
> developers have it checked out, and if you change its history, you'll cause a lot of problems for them.

## Conflicts

When we merge or rebase two branches that have changes to the same lines in two
different commits and they aren't in a `child-parent` relation, we will have a
conflict.

## Reset

We can do a reset in two ways, `hard` and `soft`.

### soft

When doing a `soft` commit we will come back to a certain point in history but we
will `keep in staged` all the changes made in difference from where you were to
where you are now.

```terminal
git reset --soft COMMITHASH or HEAD~1 or 2 to take the walk back from the
current commit
```

### hard

When doing a hard reset we will reset to a certain commit and throw everything
in the `trash`, even our `current in working tree changes`.

```terminal
git reset --hard COMMITHASH or HEAD~1 or 2 to take the walk back from the
current commit
```

## reflog

If we mess up reflog tracks the branch history if we mess up in any point.

```terminal
git reflog
```

## Commands

### Change branches

```terminal
git switch or git switc -c new_branch
```

### Restore a file changes

```terminal
git restore file
```

### Change last commit message

```terminal
git commit --amend ou git reset --soft HEAD~1 para voltar um commit
```
