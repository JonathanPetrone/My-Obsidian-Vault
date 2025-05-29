## Git Stash

The [`git stash` command](https://git-scm.com/docs/git-stash) records the current state of your working directory _and the index_ (staging area). It's kinda like your computer's copy/paste clipboard. It records those changes in a safe place and reverts the working directory to match the `HEAD` commit (the last commit on your current branch).

The `git stash` command stores your changes in a [stack (LIFO) data structure](https://www.youtube.com/watch?v=SD45xbKReT4). That means that when you retrieve your changes from the stash, you'll always get the most recent changes first.

To stash your current changes and revert the working directory to match `HEAD`:

```
git stash
```

To list your stashes:

```bash
git stash list
```


# Pop

Stash has [a few options](https://git-scm.com/docs/git-stash), but the ones that you will use most are:

```
git stash
git stash pop
git stash list
```

The `pop` command will (by default) apply your _most recent_ stash entry to your working directory and remove it from the stash list. It effectively undoes the `git stash` command. It gets you back to where you were.


## Apply Without Removing from Stash

This will apply the most recent stash changes, just like `pop`, but it will keep the stash in the stash list.

```sh
git stash apply
```
## Remove a Stash Without Applying

This will remove the most recent stash from the stash list without applying it to your working directory.

```sh
git stash drop
```
## Reference a Specific Stash

Most stash commands allow you to reference a specific stash by its index.

```sh
# Apply the third (0, 1, 2) most recent stash
git stash apply stash@{2}

# Remove the third most recent stash
git stash drop stash@{2}
```
