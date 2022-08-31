<h1 style="text-align: center;"> Git/GitHub Reference Guide</h1>

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


UNDOING CHANGES AND TIME TRAVELING
To view previous commit:		git checkout <7digit-commit-hash> 
(Puts u in detached head state)
(DETACHED HEAD: Instead of pointing 
to a branch it points to a
commit.Reattach head by switching to 
any branch)

Checkout by referencing commits	git checkout HEAD~2 (where HEAD~2 is 2 commits before HEAD)
Relative to a particular
commit(weird syntax)

Go back to last branch: 		git switch -

Discarding changes to 		git checkout HEAD <filename> or git checkout -- <filename> (...)
Revert to your last
Commit:

The new way of doing		git restore <file-name>
Above (since checkout
"Does many things"
So this is like git
Switch in a way of
Trying to take away
Work from checkout):

Restoring Files to a		git restore --source Head~1 <file-name> (Head~1 can be a hash)
State prior to HEAD:

Unstage files with		git restore --staged <file-name>
Restore:

Reset a branch to a 		git reset <commit-hash>
Specific commit.
Which deletes the commits
But keeps their changes in
The working directory
(So you could possibly
Use them to create another
Branch):
If you want to drop the		git reset --hard <commit-hash>
Changes too:

Revert is similar to		git revert <commit-hash>
Reset but it does things
Different. It
Creates a brand new 
Commit that undoes
The changes from a commit
(BETTER FOR COLLABORATION):


GITHUB BASICS
Get a local copy of an		git clone <url>
Existing repository:
(Most popular repo roster
Is GitHub)

SSH Key Generation:		https://docs.github.com/en/authentication/connecting-to-github-with-ssh

Get Code On GitHub:
	Option 1: (We must specify a remote if we do this)
		-Create a new repo on GitHub
		-Connect your local repo (add a remote)
		-Push up your changes to GitHub
		-Github example:
			git remote add origin https://github.com/ProblemBears/EloquentJS-Solutions.git	(add remote)
			git branch -M main								(rename master branch to main because Github doesn't like using master anymore)
			git push -u origin main								
	Option 2: (Remote is specified by the cloning process)
		-Create a brand new repo on Github
		-Clone it down to your machine
		-Do some work locally
		-Push up your changes to Github

A Remote is really two things: a URL and a Label
To view existing remotes:		git remote or git remote -v (verbose, for more info)
Adding a new remote:		git remote add <name> <url> (a common name is origin but the name not special)
Rename remote:			git remote rename <old> <new>
Delete remote:			git remote remove <name>

To push work to the		git push <remote> <branch>
Remote connection
(Like Github):

To push a local branch		git push <remote> <local-branch>:<remote-branch> (not common)
To a remote branch of
A different name we do:

Push branch to a remote		git push -u <origin> <main>
And connect upstream:
(This links the remote and local branch, so that we just have to use git push and not specify the 
Remote and branch over and over again. Also works to shorten git pull)

Clone a remote repo		git clone <url>
To a local computer:

A Remote Tracking Branch is a reference to the state of the master branch on the remote (can't move it yourself). It's a bookmark to the last know commit on the master branch
To view remote branches:	git branch -r
Checkout remote branch:	git checkout origin/main

When you first clone a repo from the remote, IT WON'T CREATE LOCAL BRANCHES FOR ALL THE REMOTE BRANCHES, JUST THE DEFAULT ONE. ALTHOUGH it does know about the remote branches (see git branch -r)
Create CONNECTED
local branches
For remote branches 		git switch <remote-branch-same-name>
By doing:


FETCHING: Allows us to download changes from a remote repo W/O SCREWING UP our working directory
(It's as if the remote branch becomes it's own branch)
Fetch a specific branch		git fetch <remote> (all changes)
From a remote using:		git fetch <remote> <branch> (specific branch)

PULLING: Actually updates our HEAD branch and what whatever changes are retrieved from remote
	(think of it as - git pull = git fetch + git merge)
Fetch the latest info
From specified branch and		git pull <remote> <branch> (merge conflicts may happen!)
Merge those changes into
Our current branch
(THIS MEANS WHERE MATTERS):

Shorthand above (because		git pull
Git defaults the origin
And the connected branch
Of the current so...)
Simply do:


GITHUB PAGES:
	-You get one user GitHub page for portfolio AND unlimited for each repo
	-can only be in JS/CSS/HTML (so static)
	-SETUP:
		Settings > GitHub Pages > Select a Branch > Should have a index.html

PULL REQUEST:
	-Upload your  feature branch to a remote repo and for that branch Click on Pull Request
	 so that it could possibly be merged in
To handle merge conflicts
Locally we follow these instructions
1) Bring in the changes to test:	git fetch origin
				git switch <feature-branch> (GitHub has the legacy way of doing)
				git merge main
				//FIX CONFLICTS!
   (THIS HANDLES THE MERGE CONFLICTS ON THE FEATURE BRANCH)
2) Merge the changes and update on Github:
				git switch main
				git merge --no-ff <feature-branch> (no fast forward. Commit merge)
				git push origin main
PULL REQUEST CONCLUSION: Github will autodetect the pull request merge. (Delete branch if u want)

CONFIGURING BRANCH PROTECTION RULES:
	Settings > Branches > Branch Protection Rules

FORKING: COPY OF A GITHUB REPO ON OUR GITHUB ACCOUNT
	-Allows us to create a personal copy of other people's repos. We call these copies a
	 "fork" of the original. (LIKE PRS, NOT A GIT FEATURE)
	-Then you could clone, push, pull on your version of the repo on your GitHub
	-We usually have a remote called "upstream" that points to the original GitHub Repo
	 that we forked, not to push to it (WE CAN'T), but to pull the daily changes it might
	 go through as an open source project.
	-Finally, we can do pull request on our GitHub fork to merge to the original
	-TO SETUP SECOND REMOTE FOR ORIGINAL:
		1) Copy URL from the original
		2) Setup Second remote:
			git remote add upstream <url>
		3) Pull from the original with:
			git pull upstream <branch>
