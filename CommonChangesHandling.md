Dealing with common changes
---

Let's consider two branches `A` and `B` having common changes, and we need to merge them together. 
- "Common" here means **the same contents (changes)** no matter if the commits have the same hash or different hashes (due `cherry-pick`, `rebase` or manually apply patches).
- `git merge` (3-way merge) happily deals with these common changes. However, which one will be blamed is complicated: 
  + Usually the branch having more changes on a file are prefered to be blamed on that file. [(¿ Exact algorithm ?)](https://stackoverflow.com/questions/64661089/git-merge-of-identical-changes-make-git-blame-target-the-merged-commit)
  + `git blame --first-parent` can be used to blame the merge commit itself instead of its ancestors.
  + ➡️ The blaming complication will be resolved if we clean the history.

<a id="cleanHistory" name="cleanHistory"/>

### History cleaning

When merging branches, the branching history reflects the actual development history but contains clutters that hinder human's comprehension: **redundant commits** shared between different branches and **ambiguity** in `git blame`.
- **Redundancy & ambiguity** can be avoided by **linearizing** the history.
- When merging `A` onto `B` (`B'` = `B` + `A`), the linearization of `B'` is achieved by expelling the common changes `b + c` (between `A` and `B`) from `A` (`dA` = `A` - `B`) before merging (`B'` = `B` + `dA`):  
  ![GitMerge-commons](img/GitMerge-commons.png)  
  `git merge-base B A` = `b`  
  `A` = `b` + `c` + `dA`  
  `B` = `b` + `c` + `dB`  
  `B'`= `b` + `c` + `dB` + `dA` = `B` + `dA`  
  The `A`-exclusive changes `dA` can be obtained using various techniques: `A > git rebase B` or `B > git merge --squash A` or manually select changes in `A` (e.g. using `git cherry-pick`).
- `git rebase` works well when there's no common changes after common base, i.e. when `c` = `∅`. But when `c` ≠ `∅`, it will report conflicts on all common changes.  
  ➡️ `git rebase` is unusable for Common Branch Workflow.
- `git merge --squash` works well even when there're common changes after common base (`c` ≠ `∅`). However, all the developing history after the common base will be squashed down into one commit.  
  ➡️ `git merge --squash` is usable for Common Branch Workflow.
