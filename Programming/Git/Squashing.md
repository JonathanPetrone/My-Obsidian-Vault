## What Is Squashing?

It's exactly what it sounds like. We take all the changes from a series of commits and squash them into a single commit.

![git squashing](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/EeSnkk5.png)

Perhaps confusingly, squashing is done with the `git rebase` command! Here are the steps to squash the last `n` commits:

1. Start an interactive rebase with the command `git rebase -i HEAD~n`, where `n` is the number of commits you want to squash.
2. Git will open your default editor with a list of commits. Change the word `pick` to `squash` for all but the first commit.
3. Save and close the editor.

When you squash commits, for example:

```
A - B - C - D
```

into a single commit:

```
ABCD
```

you're removing history from the project. Sure, all the _changes_ are still there, but the _individual markers of each change_ are gone. Meaning we've erased _all_ the commits we've made in the last few lessons, we can't go back to individual checkpoints anymore.

