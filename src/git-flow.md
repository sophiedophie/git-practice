# Git-flow
Reference: [Introducing git flow](https://datasift.github.io/gitflow/IntroducingGitFlow.html)

## What is git flow?
- It's a branch managing model from Vincent Driessen. Article: [A successful git branching model](https://nvie.com/posts/a-successful-git-branching-model/)
- Manage codes as each purpose
- Main branch: Master for production-ready state, Develop for reflecting latest development result
- Support branch: Feature for new features, Hotfix for sudden fix, Release for managing release branched out from develop and merging to master.

## What is each branch for?
- Feature branch should be branched off from develop, and merge back into develop. After development is done, prepare release branch which is off from develop.
- Release branch is from develop, and will be merged into develop and master. It can contain several commits after branched from develop.
- Hotfix branch is for fixing bugs on production, so branched off from master and will be merged to both develop and master.

## Why git flow structure(merits)?
- Feature branch:
  1. Parallel development: isolate new development from finished work
  2. Collaboration: each feature branch is a sandbox so easy to start new feature
- Release branch:
  - Able to make staging area for all features which is ready but not released yet
  - Can work more to fix bugs after deploy somewhere
- Hotfix branch:
  - Branch out from a tagged release: hotfix contains only emergency fix

## How can we start playing with it?
- Git-flow is already integrated with cli. You can install it. [Github repo](https://github.com/nvie/gitflow)

## How to install and play

```bash
  # install git-flow on macOS
  $ brew install git-flow

  # Initialize git flow
  $ git flow init -d

  # Create feature branch
  $ git flow feature start ${branch_name}

  # Finish the branch after commits: it merge changes to develop and remove the branch
  $ git flow feature finish ${branch_name}

  # Push/pull a feature branch to remote repo
  $ git flow feature publish ${branch_name}
  $ git flow feature pull ${remote} ${branch_name}
```

## Rollback procedure under git-flow
- Recently in my team we had issues so reverted release to previous version. But we didn't have proper manual for rollback.

### One option I can think:
let's say previous version is 2.4.0
```bash
  $ git checkout master

  $ git checkout -b release/2.4.1 master
  $ git revert HEAD

  # ... and create PR to master

  # After release, tag with new version
  $ git tag -a v2.4.1
```
- This keeps the previous merge commit and creates new commit to revert

#### Pros
- Can keep the commit logs
- Prevent from mistake might be happening while reverting on emergency situation since developer should get approvals again from peer.

#### Cons
- When people are in a hurry to revert to the previous version, it would take time to create new release branch and get approvals again.

#### Not sure
- Is it okay to use release branch?
- In this case, we keeps the version and even bump up the version to release even it's same with previous: maybe master is not same with previous (new revert commit added) so need to be new patch version.