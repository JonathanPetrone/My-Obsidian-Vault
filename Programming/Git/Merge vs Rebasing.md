Both **merging** and **rebasing** integrate changes from one branch into another, but they do so in different ways.

### Key Differences

| Feature                 | Merge (`git merge`)                                                     | Rebase (`git rebase`)                                                       |
| ----------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| **Commit History**      | Preserves branch structure with a merge commit                          | Creates a linear, rewritten history                                         |
| **New Commit Created?** | Yes (merge commit)                                                      | No (replays commits)                                                        |
| **Use Case**            | When preserving full history is important (e.g., in team collaboration) | When you want a clean, linear commit history (e.g., before pushing to main) |
| **Conflict Resolution** | Happens once during merge                                               | Happens for each commit during rebase                                       |

### Repeat Resolution Setup

A common complaint about rebase is that there are times when you may have to manually resolve the same conflicts over and over again. This is especially prevalent when you have a long-running feature branch, or even more likely, multiple feature branches that are being rebased onto `main`.

### RERERE to the Rescue

> The [git rerere](https://git-scm.com/book/en/v2/Git-Tools-Rerere) functionality is a bit of a hidden feature. The name stands for “reuse recorded resolution” and, as the name implies, it allows you to ask Git to remember how you’ve resolved a hunk conflict so that the next time it sees the same conflict, Git can resolve it for you automatically.

In other words, if enabled, `rerere` will remember how you resolved a conflict (applies to rebasing but also merging) and will automatically apply that resolution the next time it sees the same conflict. Pretty neat, right?