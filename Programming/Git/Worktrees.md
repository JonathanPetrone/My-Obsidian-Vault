## What Is a Worktree?

A worktree (or "working tree" or "working directory") is just the directory on your filesystem where the code you're tracking with Git lives. Usually, it's just the root of your Git repo (where the `.git` directory is). It contains:

- Tracked files (files that Git knows about)
- Untracked files (files that Git doesn't know about)
- Modified files (files that Git knows about that have been changed since the last commit)

## The Worktree Command

Git has the [`git worktree` command](https://git-scm.com/docs/git-worktree) that allows us to work with worktrees. The first subcommand we'll worry about is simple:

```
git worktree list
```

It lists all the worktrees you created.

## Linked Worktrees

We've talked about:

1. Stash (temporary storage for changes)
2. Branches (parallel lines of development)
3. Clone (copying an entire repo)

Worktrees accomplish a similar goal (allow you to work on different changes without losing work), but are particularly useful when:

1. You want to switch back and forth between the two change sets without having to run a bunch of `git` commands (not branches or stash)
2. You want to keep a light footprint on your machine that's still connected to the main repo (not clone)

## The Main Worktree

- Contains the `.git` directory with the entire state of the repo
- Heavy (lots of data in there!). To get a new main working tree requires a `git clone` or `git init`

## A Linked Worktree

- Contains a `.git` _file_ with a _path_ to the main working tree
- Light (essentially no data in there!), about as light as a branch
- Can be complicated to work with when it comes to env files and secrets

## Create a Linked Worktree

To [create a new worktree](https://git-scm.com/docs/git-worktree#Documentation/git-worktree.txt-addltpathgtltcommit-ishgt) at a given path:

```
git worktree add <path> [<branch>]
```

_Note: `<branch>` is optional, it will use the last part of the path as the branch name._

### Delete Worktrees

You may never need to stash again! Okay, stash is still useful for tiny stuff, but worktrees are so much better for long-lived changes.

However, at some point you will need to clean up your worktrees. The simplest way is the [`remove` subcommand](https://git-scm.com/docs/git-worktree#Documentation/git-worktree.txt-remove):

```
git worktree remove WORKTREE_NAME
```

An alternative is to delete the directory manually, then [`prune`](https://git-scm.com/docs/git-worktree#Documentation/git-worktree.txt-prune) all the worktrees (removing the references to deleted directories):

```
git worktree prune
```