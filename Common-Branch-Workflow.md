Common Branch Workflow
===

Although most of the feature branches (in Feature Branch Workflow) are independent of each other, sometime there are common features must be shared between other features. In this case, instead of pushing all shared commits to the main branch, we should have a common branch standing between the main branch and feature branches.

Workflow:
- The common branch is treated as a feature branch, i.e. forked from the main branch then eventually merged back to the main branch. Other feature branches are forked from the main branch, not from the common branch, because some features may not need some commits in the feature branch.
- A dev contribute to this common branch by `cherry-pick`ing commits from his/her feature branch.
- Then other devs will be informed to merge these common commits into their feature branches. 
- In developing phase, the common branch is treated as a feature branch to hold works in progress which usually have backward changes and rollbacks. 
- Before being merged into the main branch, just like a feature branch, the common branch can be cleaned up, beautified with squashing, rebased, etc. However, it must be merged before other feature branches.

