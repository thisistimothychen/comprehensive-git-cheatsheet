# Comprehensive Git Cheatsheet
Full git cheatsheet based on Kevin Skoglund's Git Essential Training on Lynda.com



### **Table of Contents**

1. [Installing Git](#installing-git)
2. [Getting Started](#getting-started)
3. [Making Changes to Files](#making-changes-to-files)
4. [Undoing Changes](#undoing-changes)
5. [Ignoring Files](#ignoring-files)
6. [Navigating the Commit Tree](#navigating-the-commit-tree)
7. [Comparing commits](#comparing-commits)
8. [Branching](#branching)
9. [Merging](#merging)
10. [Stashing Changes](#stashing-changes)
11. [Remotes](#remotes)
12. [Terminology](#terminology)
13. [Great Resources](#great-resources)

---

## Installing Git
[Instructions on setting up Git](https://help.github.com/articles/set-up-git/)

## Getting Started
1. Create project folder (this will be your working directory)
2. Navigate into the root of your working directory
3. Initialize Git in your working directory: `git init`
4. Make changes in your working directory
5. Add all changes that have been made in the working directory (stage your changes): `git add .`
6. Add only certain files with changes to the staging index: `git add filename`
7. Commit your staged changes with a message: `git commit -m 'message here'`
8. OR Add all unstaged files and commit them together: `git commit -am 'message here'`
9. View commits you've made: `git log`
     - Return most recent 5 commits: `git log -n 5`
     - Return all commits in a concise format: `git log --oneline`
     - Return all commits made since August 9th, 2016: `git log --since=2016-08-09`
     - Return all commits made up until August 9th, 2016: `git log --until=2016-08-09`
     - Return all commits made by a certain author: `git log --author="Name"`
     - Return all commits with "Init" in the commit message (case sensitive): `git log --grep="Init"`
     - Returns a nice view of the branches and history of [merges](#merging): `git log --graph --oneline --all --decorate`



## Making Changes to Files
#### Show current status in local working directory
    git status
    
#### View changes
Compare what's in the repository (the version that HEAD is pointing at) versus what's in your working directory (lines preceded with minus sign is from the old version; lines preceded with plus sign is from the new version):

    git diff

View only the staged changes (instead of all of your working directory):

    git diff --staged
    
#### Delete a file
Keep the file accessible through trash: delete the file in your working directory and then:

    git rm file_to_delete

Completely remove the file from your working directory and machine (deleted file is not in trash) and stage this change:

    git rm file_to_delete
    
#### Rename or move a file

    git mv original_filename new_filename


## Undoing Changes
#### Undoing local working directory changes (revert to repository version)
Make your working directory's `filename` look like the one in your repository (the `--` makes sure to stay on the current branch):

    git checkout -- filename
    
#### Unstaging files
Keep the changes in your local working directory but unstage it (e.g. if you're adding a bunch of files to a commit but you accidentally add a file that shouldn't be in that commit)

    git reset HEAD filename

#### Amending commits
Add a file to the most recent commit (where the HEAD points to)

    git add filename
    git commit --amend -m 'Commit message'
    
Change the most recent commit's message

    git commit --amend -m 'New commit message'

#### Retrieving old versions
Put a version of `filename` which was in a commit before HEAD (`SHA-ha$hc0d3` is the commit's SHA hash which looks like a string such as `c4b913ef2da10a5d72c1ab515ada1fb9c3569e3`) into your staging area

    git checkout SHA-ha$hc0d3 -- filename
    git commit -m 'Revert to filename at commit c4b913ef2da10a5d72c1ab515ada1fb9c3569e3'

#### Reverting a commit (flip the changes that were made)
Works well for simple changes.

    git revert c4b913ef2da10a5d72c1ab515ada1fb9c3569e3

#### **[CAUTION]** Using `reset` to undo commits
(`reset` moves the HEAD pointer)

(Safest) Soft reset moves the HEAD pointer of the repository to the specified commit. Will not change the staging index or the working directory. Your staging index and working directory will contain the files in their later revised state.

    git reset --soft c4b913ef2da10a5d72c1ab515ada1fb9c3569e3

(Default) Mixed reset moves the HEAD pointer of the repository to the specified commit, and also changes the staging index to match the repository. Will not change working directory. Your working directory still retains the changes that you've made.

    git reset --mixed c4b913ef2da10a5d72c1ab515ada1fb9c3569e3

(Most destructive) Hard reset moves the HEAD pointer of the repository to the specified commit, AND it will make your staging index **and your working directory** match that as well. _Any changes that came after the specified commit are completely gone._

    git reset --hard c4b913ef2da10a5d72c1ab515ada1fb9c3569e3


#### Removing untracked files
Removes untracked files from your working directory.

Test run of what files will be removed:

    git clean -n

Forces the clean to run (does not remove files in staging index, however):

    git clean -f


## Ignoring Files
#### Using .gitignore files
Create a .gitignore file by saving a new notepad file as .gitignore (and file type as All Files) in your working directory or use the nano program in UNIX:

    nano .gitignore

Set rules:

    # Pound signs denote a comment -- these are skipped.
    *.php               # ignores all files with .php extension (note: the wildcard (*) does not apply to directories
    !index.php          # do not ignore index.php (even though the line above this says to ignore it!)
    assets/videos/      # ignore all files in a directory with a trailing slash

Commit the .gitignore file:

    git add .gitignore
    git commit -m 'Add .gitignore file'


#### Understanding what to ignore
- compiled source code: you want to store uncompiled code only, and then compile the code after you pull down the repository (since compiled code depends on the computer specs)
- packages and compressed files (files that you're usually not using in the project itself)
- logs and databases (files that change often)
- operating system generated files (these files won't related to your project)
- user uploaded assets (images, PDFs, videos) usually stored during development/testing

More resources:

[GitHub's help article on .gitignore](https://help.github.com/articles/ignoring-files)

[GitHub’s collection of .gitignore file templates](https://github.com/github/gitignore)


#### Ignoring tracked files
(Use for files you want to stop tracking after you've committed to your repository.) Remove `filename` from the staging index (not from the repository or working directory) so that it is no longer tracked (even if it has been committed previously):

    git rm --cached filename

#### Tracking empty directories
Create a `.gitkeep` file in the directory


## Navigating the Commit Tree
You can reference commits in Git using the following tree-ish methods:
- full SHA-1 hash
- short SHA-1 hash (at least 4 characters for small projects, and 8-10 characters to be unambiguous)
- HEAD pointer
- branch reference (this would refer to the tip of the branch)
- parent commit (`HEAD^`, `acf87504^`, `master^`, `HEAD~1`, `HEAD~`)
- grandparent commit (`HEAD^^`, `acf87504^^`, `master^^`, `HEAD~2`)
- great-grandparent commit (`HEAD^^^`, `acf87504^^^`, `master^^^`, `HEAD~3`)

#### Exploring tree listings
    git ls-tree <tree-ish>

#### Getting more from the commit log
- Show only one line for each commit: `git log --oneline`
- Show only 5 commits: `git log -5`
- Show the commits since August 9th, 2016: `git log --since=2016-08-09`
- Show the commits until August 9th, 2016: `git log --until=2016-08-09` or `git log --before=2016-08-09`
- Other time formats: `git log --since="2 weeks ago"`, `git log --since=2.weeks`, `git log --until=3.days`
- Show all by a certain author: `git log --author="Name"`
- Show all commits with "temp" in the commit message: `git log --grep="temp"`
- Show all commits between `2907d12` and `acf8750`: `git log 2907d12..acf8750`
- Show all commits that have affected `filename`: `git log filename`
- Show the actual changes: `git log -p`
- More can be found by running `git log --help`


## Comparing commits
Show all changes since commit `2907d12`:

    git diff 2907d12

Show all changes between `2907d12` and `acf8750`:

    git diff 2907d12..acf8750

Show changes in `filename` between `2907d12` and `acf8750`:

    git diff 2907d12..acf8750 filename

Show snapshot of changes between `2907d12` and `acf8750`:

    git diff --stat --summary 2907d12..acf8750
    
Other options:
- `-b` or `--ignore-space-change` (insignificant changes)
- `-w` or `--ignore-all-space`



## Branching
- try new ideas
- isoltae features or sections of work

#### Viewing and creating branches
View all branches in local repository:

    git branch

Create new branch:

    git branch branch_name

#### Switching branches

    git checkout branch_name

#### Creating and switching branches (at same time)

    git checkout -b branch_name


#### Comparing branches

    git diff first_branch..second_branch

Put differences on one line:

    git diff --color-words first_branch..second_branch

Flip the "old branch" and "new branch":

    git diff --color-words second_branch..first_branch

Show all branches that are completely included in current branch:

    git branch --merged

#### Renaming branches

    git branch --move old_branch_name new_branch_name

#### Deleting branches
(You cannot be on `branch_to_delete` to run the following command.) Delete a branch that has been fully merged:

    git branch -d branch_to_delete

**[CAUTION]** Delete a branch that has NOT been fully merged:

    git branch -D branch_to_delete


## Merging
Simple case of merging:
1. Checkout into the branch that you want to _receive_ the changes
2. `git merge branch_to_get_changes_from`

Merge without using fast-forward method (when you want to just use the `branch_to_get_changes_from`'s commits):

    git merge --no-ff branch_to_get_changes_from

Merge only if the fast-forward method can be used (creates a merge commit message instead of just using the `branch_to_get_changes_from`'s commits):

    git merge --ff-only branch_to_get_changes_from

#### Dealing with merge conflicts:
Abort a merge (check that you're in the merging state):

    git merge --about

Resolve the conflicts manually. Look at the differences and pick which one you want to keep and delete the one you want to remove. Then save your file, run `git add .` and then `git commit` or `git commit -m 'Commit message'`

    <<<<<<< HEAD
    <p>Hello, World!</p>
    =======
    <p>Goodbye, World!</p>
    >>>>>>> branch_to_get_changes_from

Use a merge tool (use the following command to see the tools):

    git mergetool

#### Strategies to reduce merge conflicts
- keep lines short so you can pinpoint your conflicts
- keep commits small and focused
- beware stray edits to whitespace (spaces, tabs, line returns)
- merge often (break up and resolve conflicts as you go)
- track changes to master (keep your branches and master in sync)


## Stashing Changes
The stash is not part of the repository, staging index, or working directory -- it's a special fourth area in Git. Similar to commits but don't have SHA's associated with them. Use when you have changes but you're not ready to commit them yet (e.g. when you need to switch branches but have uncommitted changes).

    git stash save 'stash message that no one will see but should still be descriptive'

View stashed changes (all stashes are available on any branch):

    git stash list

Show more information about the stash named `stash@{0}`:

    git stash show stash@{0}

Show `stash@{0}` as a patch (a section of code you can apply to different things to modify change them) (i.e. "show me the changes"):

    git stash show -p stash@{0}

(Commonly used) Retrieve a stashed change into current branch and remove from stash (there may be merge conflicts):

    git stash pop
    
(Not as commonly used) Retrieve a stashed change but leave a copy in stash (there may be merge conflicts):

    git stash apply

Delete a stashed change named `stash@{0}`:

    git stash drop stash@{0}

**[CAUTION]** Delete all stashes:

    git stash clear


## Remotes
Allows for collaboration with others. You can take your changes and put them on a remote server so that other people can see them, make changes of their own, and upload those changes back to the remote server. You will also be able to pull others' changes back into your repository.

#### Adding a remote repository
1. Set up GitHub and create a repository
2. `git remote` shows all the remotes
3. `git remote add <alias> <url>` to add a remote -- use the repository you created in step 1 to find `<alias>` and `<url>` (it's on the initial setup page)
4. `git remote rm <alias>` to remove the `<alias>` remote
5. `git push -u <alias> <branch>` to push `<branch>` to the `<alias>` remote (you may be prompted to input your GitHub username and password)

`git branch -r` to see remote branches

`git branch -a` to show both remote and local branches

#### Cloning a remote repository to your local machine
1. Navigate to your desired directory
2. Get the path to the repository you want to clone
3. `git clone https://github.com/coolkid/coolproject.git` to clone the `coolproject` repository from GitHub user `coolkid`

#### Pushing changes to a remote repository
    git push <alias> <branch>
    
(If your remote is being tracked, you can just use `git push` instead.)

#### Fetching changes from a remote repository
Update `<alias>/master` with what's on the remote repository: run `git fetch <alias>` or `git fetch`

##### Guidelines:
- fetch before you work
- fetch before you push
- fetch often


#### Merging in fetched changes
Merge local version of `<alias>/master` with current branch

    git merge <alias>/master

Do `git fetch` and `git merge` in one single command:

    git pull

#### Checking out remote branches

    git branch branch_we_are_creating  <alias>/where_we_are_checking_out_from

#### Pushing to an updated remote branch
GitHub will not let you push changes to a branch if there needs to be a merge. You must `fetch` changes from the remote repository, `merge` the changes in your local repository, and then `push` back to the remote repository.

#### Deleting a remote branch
`git push <alias> :remote_branch_to_delete` or `git push <alias> --delete remote_branch_to_delete`



---

## Terminology
HEAD:
- pointer to "tip" of current branch in repository
- last state of repository, what was last checked out
- points to parent of next commit
   - where writing commits takes place

Tree-ish: something that references part of the tree


---


## Great Resources
[Here are all the Git commands I used last week, and what they do. by Sam Corcos](https://medium.freecodecamp.com/git-cheat-sheet-and-best-practices-c6ce5321f52#.blar3050f)

[Try Git!](http://try.github.io/)
