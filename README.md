# Comprehensive Git Cheatsheet
Full git cheatsheet based on Kevin Skoglund's Git Essential Training on Lynda.com

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
8. View commits you've made: `git log`
     - Return most recent 5 commits: `git log -n 5`
     - Return all commits made since August 9th, 2016: `git log --since=2016-08-09`
     - Return all commits made up until August 9th, 2016: `git log --until=2016-08-09`
     - Return all commits made by a certain author: `git log --author="Name"`
     - Return all commits with "Init" in the commit message (case sensitive): `git log --grep="Init"`


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


## Terminology
HEAD
- pointer to "tip" of current branch in repository
- last state of repository, what was last checked out
- points to parent of next commit
   - where writing commits takes place

   








