## What is Git merge with commitish?

The `git merge` command with commitish allows you to merge changes from any commit reference point, not just branches. A commitish is any identifier that points to a specific commit in Git's history, including branches, tags, commit hashes, and special references like HEAD@{1}.

## The types of commitish:

- **Branch names:** Direct references like 'main' or 'feature-branch'
- **Commit hashes:** Full or shortened SHA-1 hashes
- **Tags:** Named reference points marking specific versions
- **HEAD references:** Special pointers like HEAD@{1} or HEAD~3

## Reference:

- [https://git-scm.com/docs/git-merge](https://git-scm.com/docs/git-merge)
- [https://git-scm.com/docs/gitrevisions](https://git-scm.com/docs/gitrevisions)
- [https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection](https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection)

## Similar:
- Git cherry-pick (selecting specific commits)
- Git rebase (reorganizing commit history)
- Git reset (moving HEAD to specific commits)
## Opposite:
- Basic branch merging without commit references
- Linear version control systems
- Simple file versioning without commit history
## Theme/Questions:
- How do Git references work internally?
- What's the difference between various commitish types?
- How to recover deleted commits using commitish?
## What does this lead to?
- Advanced Git workflow management
- Better disaster recovery capabilities
- More flexible branch management strategies