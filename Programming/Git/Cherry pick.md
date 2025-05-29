## How to Cherry Pick

1. First, you need a clean working tree (no uncommitted changes).
2. Identify the commit you want to cherry-pick, typically by `git log`ing the branch it's on.
3. Run:

```
git cherry-pick <commit-hash>
```
