## What is Git Forking vs Cloning?
A fork is a copy of a repository on a platform like GitHub that allows you to freely experiment with changes without affecting the original project. Unlike cloning, which is a local copy, forking creates a remote copy under your account. This is commonly used for contributing to open source projects where you don't have direct write access to the original repository.

Use Forking (there is no fork command in Git) from Github when you want to eventually contribute to a repo.

**Example steps:**
1. Fork main repo
2. Clone down forked repo to own environment
3. Make changes to the main branch (or other) in the forked repo
4. Push changes to the main branch (or other)
5. Open PR with the changes from the for onto the original repo

## Key Differences:

- **Fork:** Server-side copy of repository (GitHub feature)
- **Clone:** Local copy of repository (Git command)
- **Permissions:** Fork preserves connection to original repo for PRs, clone is independent

## Reference:
- [https://docs.github.com/en/get-started/quickstart/fork-a-repo](https://docs.github.com/en/get-started/quickstart/fork-a-repo)
- [https://git-scm.com/docs/git-clone](https://git-scm.com/docs/git-clone)
- [https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks) 
## Similar:
- Branch creation (for internal team collaboration)
- Repository mirroring
- Git remote tracking
## Opposite:
- Direct repository access
- Local-only development
- Single-user workflows
## Theme/Questions:
- When should you fork vs clone?
- How to keep a fork synchronized with original repo?
- What are best practices for fork-based contributions?
## What does this lead to?
- Open source contribution workflows
- Distributed team collaboration
- Code review and quality control processes