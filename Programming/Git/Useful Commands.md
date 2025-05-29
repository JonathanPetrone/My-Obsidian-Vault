## Git reflog
The [`git reflog` command](https://git-scm.com/docs/git-reflog) (pronounced ref-log, not re-flog) is kinda like `git log` but stands for "reference log" and specifically logs the changes to a "reference" that have happened over time.


## Delete branch
- `git branch -d <branch>`: This **deletes** the branch **only if it has been fully merged** into its upstream branch or the current branch. If the branch has unmerged changes, Git will prevent the deletion and show a warning.
- `git branch -D <branch>`: This **force deletes** the branch **regardless of whether it has been merged or not**. This is useful if you want to remove a branch without caring about its merge status.

## Git merge commitish
git merge commitish

A 'commitish': e.g. a commit, branch, tag, or reflog entry

### Git reset
git-reset - Reset current HEAD to the specified state

### Git revert
Where [`git reset`](https://git-scm.com/docs/git-reset) is a sledgehammer, [`git revert`](https://git-scm.com/docs/git-revert) is a scalpel.

A revert is effectively an _anti_ commit. It does not _remove_ the commit (like `reset`), but instead creates a new commit that does the exact opposite of the commit being reverted. It undoes the change but keeps a full history of the change and its undoing.

#### Using Revert
To revert a commit, you need to know the commit hash of the commit you want to revert. You can find this hash using `git log`.

## Diff

The [`git diff` command](https://git-scm.com/docs/git-diff) shows you the differences between... stuff. Differences between commits, the working tree, etc.

I frequently use it to look at the changes between the current state of my code and the last commit. For example:

```bash
# show the changes between the working tree and the last commit
git diff
```

```bash
# show the differences between the previous commit and the current state, including the last commit and uncommitted changes
git diff HEAD~1
```

```bash
# show the change between two commits
git diff COMMIT_HASH_1 COMMIT_HASH_2
```