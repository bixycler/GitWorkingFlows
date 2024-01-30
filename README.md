Git Workflows
===

Typical workflows
---

- [Centralized Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows) with `rebase`: 
  This is the old workflow from Subversion (SVN) based on a single central branch named `main` or `master` or `trunk`, with a **linear history**. The linearity is achieved with `git rebase`: the downstream (branch or local repo) is rebased to the upstream's `HEAD` before being fast-forward-merged in to the upstream.
- [Branching workflows](https://www.abtasty.com/blog/git-branching-strategies/) for features, commons, release and deployment:
  Git is superior at branching and merging. Hence, most of the Git workflows are based on `git branch` and `git merge`.    
  ![Git Flow by Vincent Driessen from 2010](https://nvie.com/img/git-model@2x.png)
  + [Feature Branch Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow):
    For each feature, functionality, or task, a "feature branch" is forked from the main branch. The dev in charge will commit changes to his/her feature branch, then request the main branch to pull & merge these commits via [pull requests](https://docs.github.com/en/pull-requests) (PR), which is also called ["merge requests"](https://docs.gitlab.com/ee/user/project/merge_requests/) (MR).  
    The branching from one main branch to many feature branhces can be repeated to create multilayered branching, e.g. `master` -> `project` -> `feature`, or `master` -> [`integration`](https://remarkablemark.medium.com/git-integration-branch-workflow-77fa0fd32883) -> `ticket`, etc.
  + [Common Branch Workflow](#CommonBranchWorkflow):
    This extends the Feature Branch Workflow with one or more common branches holding common commits to be shared with other feature branches.
  + [Forking Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow):
    This extends the branching from branches to repositories. Instead of forking feature branches from the main branch, here each dev will fork a downstream repo from the upstream repo. The dev will commit changes to downstream repo, then request the upstream repo to pull & merge these commits via PR/MR. This workflow enpowers the *open-source development*.
  + [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/): 
    This extends the Feature Branch Workflow with *release* and *hotfix* branches for scheduled release management.
  + [GitLab Flow](https://docs.gitlab.co.jp/ee/topics/gitlab_flow.html#production-branch-with-gitlab-flow): 
    This adds branches for *deployment on different environments*, e.g. staging, pre-production, production. 
- [Trunk-based workflows](https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development) for CI/CD:
  The modern frameworks for continuous integration & continuous deployment (CI/CD) enable devs to get back to the simple Centralized Workflow. There may also be feature branches, like in [GitHub Flow](https://docs.github.com/en/get-started/using-github/github-flow), but they are **short-lived** instead of long-lived feature branches in the Feature Branch Workflow.


<a id="CommonBranchWorkflow"></a>

Common Branch Workflow
---

Although most of the feature branches (in Feature Branch Workflow) are independent of each other, sometime there are common features must be shared between other features. In this case, instead of pushing all shared commits to the main branch, we should have a common branch standing between the main branch and feature branches.

Workflow:
- The common branch is treated as a feature branch, i.e. forked from the main branch then eventually merged back to the main branch. Other feature branches are forked from the main branch, not from the common branch, because some features may not need some commits in the feature branch.
- A dev contribute to this common branch by `cherry-pick`ing commits from his/her feature branch.
- Then other devs will be informed to merge these common commits into their feature branches. 
- In developing phase, the common branch is treated as a feature branch to hold works in progress which usually have backward changes and rollbacks. 
- Before being merged into the main branch, just like a feature branch, the common branch can be cleaned up, beautified with squashing, rebased, etc. However, it must be merged before other feature branches.

Note: There is a question about this issue of [shared feature branch](https://stackoverflow.com/questions/3817967/correct-git-workflow-for-shared-feature-branch) on Stack Overflow, but there has not been any satisfactory answer.

