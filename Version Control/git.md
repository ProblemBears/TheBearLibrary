<h1 style="text-align: center;">Git/GitHub Reference Guide</h1>

## Table of Contents
1. [Git Config](#git-config)
2. [Git Basics](#git-basics)
3. [Commits in Detail](#commits-in-detail)
4. [Branches](#branches)
5. [Merging Branches](#merging-branches)
6. [Git Diff](#git-diff)
7. [Git Stash](#git-stash)
8. [Undoing changes and time traveling](#undoing-changes-and-time-travelling)
9. [Github Basics](#github-basics)
10. [Github Pages](#github-pages)
11. [Pull Request](#pull-request)
12. [Configuring branch protection rules](#configuring-branch-protection-rules)
13. [Forking](#forking-copy-of-a-github-repo-on-our-github-account)
## Change Text Editor
1. https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Setup-and-Config (copy/paste for your editor)
2. If you get error when try to commit DO: CTRL + SHIFT + P -> Type: "Code" -> Click "Shell Command: Install 'code command in path"

## Git Config
|                               |                                                   |
|           ---                 |                      ---                          |
| **Check your username:** 		|   `git config user.name`                          |
| **Change your username:** 	|	`git config —global user.name “Insert Name”`    |
| **Check your email:** 		|   `git config user.email`                         |
| **Change your username:** 	|	`git config —global user.email “email@email”`   |

## Git Basics
|                               |                                                   |
|           ---                 |                      ---                          |
| Status of git Repo:           |		`git status`                                |
| Initialize git Repo:          |		`git init`                                  |
| Add to Staging Area:          |		`git add file1 file2`                       |
| Add all changes:              |		`git add .`                                 |
| Commit Staging Area:          |		`git commit -m “my message”`                |
| Log of Commits:               |		`git log`                                   |

## Commits in Detail
|                               |                                   |
|           ---                 |               ---                 |
| Amend last commit message:	|	`git commit -—amend`            |
| Amend last commit files:      |   `git commit --amend --no-edit`  |
| Commit and add all staged:    |	`git commit -a -m "message"`    |

## Branches
| | |
| --- | --- |
| List all branches:            |	`git branch` (-v for mor info)                                      |
| Create new branch:            |	`git branch branch-name`                                            | 
| Switch to a branch:           |	`git switch branch-name`                                            |
| Switch branch (legacy):       |	`git checkout branch-name`                                          |
| Create and switch to branch:  |	`git switch -c branch-name`                                         |
| Delete a Branch:              |	`git branch -d branch-name` (be off the branch to be deleted)       |
| Rename a Branch:              |	`git branch -m new-branch-name` (be on the branch to be renamed)    |

## Merging Branches
- **Fast-Forward Merge** - Make **receiver branch** "catch up" to another branch\
To do this merge follow, these basic steps:
    1. Switch to or checkout the branch you want to merge the changes into (the receiving branch)
    2. Use the git merge command to merge the changes from a specific branch into the current branch\
    ```
    git merge branch-name
    ```

- **Merge Commit** - When head of receiver branch isn't on the path to the branch to be merged. We generate a Merge in a commit (This also shows that it is possible for a commit to have two parents), **this is automatically done unless there is a conflict** which we would have to resolve manually.

## Git Diff
|||
|---|---|
| List all changes in our Working directory that Are NOT staged for next commit: | `git diff` |
| List all changes in the working directory since the last commit: | `git diff HEAD` |
| List changes between staged and last commit: | `git diff --staged` or `--cached` |
| Diff Specific Files: |	`git diff HEAD [filename]` or `git diff --staged [filename]` (more files by seprating with spaces) |
| Comparing Branches: |	`git diff branch1..branch2` (order matters) (apparently we don't have to use .. we can just space) |
| Comparing Commits: |	`git diff commit1..commit2` (do git log --oneline to get the commit hash)|


## Git Stash
|||
| --- | --- |
| Take all uncommited changes (both staged and unstaged) and stash them, reverting changes in your working copy: | `git stash` |
| Remove most recently stashed changes in your stash and re-apply them to working copy: | `git stash pop` |
| Apply stash without removing from the stash(might cause MERGE CONFLICT) | `git stash apply` |
| Show Items in Stash:              | 	`git stash list`            |
| Reference Specific stash Item:    |	`git stash apply stash@{2}` |
| Delete Specific stash Item:       |	`git stash drop stash@{2}`  |
| Completely empty stash:           |	`git stash clear`           |


## Undoing changes and time travelling

- **Detached Head**: Instead of pointing 
to a branch the head points to a commit.
	- Reattach the head by switching to any branch

|||
| --- | --- |
| To view previous commit: (Puts u in detached head state) | `git checkout <7digit-commit-hash>` |
| Checkout by referencing commits |	`git checkout HEAD~2` (where HEAD~2 is 2 commits before HEAD) |
| Go back to last branch: | 		`git switch -` |
| Discarding changes to Revert to your last Commit: | `git checkout HEAD <filename>` or `git checkout -- <filename>`(...) |
| The new way of doing Above (since checkout "Does many things" So this is like git Switch in a way of Trying to take away Work from checkout): | `git restore <file-name>`|
| Restoring Files to a State prior to HEAD: | `git restore --source Head~1 <file-name>` (Head~1 can be a hash) |
| Unstage files with Restore: |		`git restore --staged <file-name>` |
| Reset a branch to a Specific commit. Which deletes the commits But keeps their changes in The working directory (So you could possibly Use them to create another Branch): | `git reset <commit-hash>` |
| If you want to drop the Changes too: |		`git reset --hard <commit-hash>` |
| Revert is similar to Reset but it does things Different. It Creates a brand new Commit that undoes The changes from a commit (BETTER FOR COLLABORATION): | `git revert <commit-hash>` |

## Github Basics

- [SSH Key Generation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- **Get Code On GitHub: (2 Options)**
	1. (We must specify a remote if we do this)
		- Create a new repo on GitHub
		- Connect your local repo (add a remote)
		- Push up your changes to GitHub
		- Github example:
			```
			git remote add origin <repo-url>
			git branch -M main
			git push -u origin main		
			```						
	2. (Remote is specified by the cloning process)
		- Create a brand new repo on Github
		- Clone it down to your machine
		- Do some work locally
		- Push up your changes to Github
- A Remote is really two things: **a URL** and **a Label**

|||
|---|---|
| Get a local copy of an Existing repository: (Most popular repo hoster Is GitHub) | `git clone <url>` |
| To view existing remotes: |		`git remote` or `git remote -v` (verbose, for more info) |
| Adding a new remote: |		`git remote add <name> <url>` (a common name is origin but the name is not special) |
| Rename remote:	|		`git remote rename <old> <new>` |
| Delete remote:	|		`git remote remove <name>` |
| To push work to the Remote connection (Like Github): |  `git push <remote> <branch>` |
| To push a local branch To a remote branch of A different name we do: | `git push <remote> <local-branch>:<remote-branch>` (not common) |
| Push branch to a remote And connect upstream: (This links the remote and local branch, so that we just have to use git push and not specify the Remote and branch over and over again. Also works to shorten git pull) | `git push -u <origin> <main>` |
| Clone a remote repo to a local computer: |	git clone <url> |
| To view remote branches: |	`git branch -r` |
| Checkout remote branch: |	`git checkout origin/main` |
| Create CONNECTED local branches For remote branches By doing: | `git switch <remote-branch-same-name>` |


- **Fetching -** Allows us to download changes from a remote repo W/O MESSING UP our working directory (It's as if the remote branch becomes it's own branch)

|||
|---|---|
| Fetch a remote |		`git fetch <remote>` |
| Fetch a specific branch: |		`git fetch <remote> <branch>` |

- **Pulling -** Actually updates our HEAD branch and what whatever changes are retrieved from remote (think of it as - `git pull = git fetch + git merge`)

|||
|---|---|
| Fetch the latest info From specified branch and Merge those changes into Our current branch (THIS MEANS WHERE MATTERS): | `git pull <remote> <branch>` (merge conflicts may happen!) |
| Shorthand above (because Git defaults the origin And the connected branch Of the current so...) Simply do: | `git pull` |

## Github Pages
- You get one user GitHub page for portfolio AND unlimited for each repo
- can only be in JS/CSS/HTML (so static)
- **Setup:** <br />&nbsp;&nbsp; **Settings > GitHub Pages > Select a Branch >** Pick a directory with an index.html

## Pull Request
- Upload your feature branch to a remote repo and for that branch Click on Pull Request so that it could possibly be merged in
- To handle merge conflicts locally we follow these instructions
	1. Bring in the changes to test:
		```	
		git fetch origin
		git switch <feature-branch>
		git merge main
		FIX CONFLICTS!
		(THIS HANDLES THE MERGE CONFLICTS ON THE FEATURE BRANCH)
		```
	2. Merge the changes and update on Github:
		```
		git switch main
		git merge --no-ff <feature-branch> (no fast forward aka Commit merge)
		git push origin main
		```

## Configuring branch protection rules:
**Settings > Branches > Branch Protection Rules**

## Forking: Copy of a Github repo on our Github account
- Allows us to create a personal copy of other people's repos. We call these copies a
	 "fork" of the original.
- Then you could clone, push, pull on your version of the repo on your GitHub
- We usually have a remote called "upstream" that points to the original GitHub Repo
	 that we forked, not to push to it (WE CAN'T), but to pull the daily changes it might
	 go through as an open source project.
- Finally, we can do pull request on our GitHub fork to merge to the original
- To setup second remote for original:
	1. Copy URL from the original
	2. Setup Second remote:
		`git remote add upstream <url>`
	3. Pull from the original with:
		`git pull upstream <branch>`
