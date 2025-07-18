## What is Git?
Git is a distributed SCM (Source Code Management). It's a way to handle and working different versions of source code. 

With Git, every time you commit, or save the state of your project, Git basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot. This lets us use these snapshot almost as save files in a video game, letting us jump from different points in history.

We can replace the current source code when we feel like it or “merge” it with what new code we have built, to create a new baseline for our work. This means that as long as there is more than a copy of the code, we are immune to some of the risks that comes with a centralized storage point. 

## The three stages of files within git
- **Untracked:** the file exists, but is not part of git's version control
- **Staged:** the file has been added to git's version control but changes have not been committed
- **Committed:** the change has been committed

## Reference:
- https://archaeogeek.github.io/gettingstartedwithgit/git/stages.html 
- https://git-scm.com/about/branching-and-merging 
- https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F 
## Similar:
- SVN (Subversion) - Another version control system, but centralized rather than distributed
- Mercurial - A distributed version control system with similar concepts to Git
- Perforce - Enterprise version control system used in large organizations
## Opposite: 
- Manual file backup (copying files with dates in filename)
- Single-user systems (no collaboration features)
- FTP-based file management (no version tracking)
## Theme/Questions:
- How does branching work in Git?
- What are best practices for commit messages?
- How to handle merge conflicts effectively?
## What does this lead to?
- Collaborative software development through platforms like GitHub and GitLab
- Continuous Integration/Continuous Deployment (CI/CD) pipelines
- Open source project management and contribution workflows