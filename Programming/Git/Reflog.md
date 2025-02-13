## What is Git reflog?

Git reflog (reference log) is a command that tracks the history of changes made to Git references like branches and HEAD over time. Unlike git log which shows commit history, reflog shows how these references have moved, making it invaluable for recovering from mistakes or viewing reference manipulation history.

## Key Components of reflog format:

- **HEAD@{n}:** Shows how many steps back in time from current HEAD
- **Reference movements:** Records all reference changes including commits, merges, resets
- **Timestamp:** When the reference change occurred

## Reference:
- [https://git-scm.com/docs/git-reflog](https://git-scm.com/docs/git-reflog)
- [https://git-scm.com/book/en/v2/Git-Internals-Reflogs](https://git-scm.com/book/en/v2/Git-Internals-Reflogs)
- [https://git-scm.com/docs/gitrevisions#Documentation/gitrevisions.txt-emem](https://git-scm.com/docs/gitrevisions#Documentation/gitrevisions.txt-emem) 
## Similar:
- Git log (shows commit history)
- Git blame (tracks changes by line)
- Git shortlog (summarized version of git log)
## Opposite:
- Forward-only version history
- Simple commit logs without reference tracking
- Basic file modification timestamps
## Theme/Questions:
- How long does reflog keep history?
- Can reflog help recover deleted branches?
- What's the difference between reflog and log?
## What does this lead to?
- Advanced Git disaster recovery
- Better understanding of Git's internal reference system
- More confident Git operations with safety net