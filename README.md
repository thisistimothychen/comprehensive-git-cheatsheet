# Comprehensive Git Cheatsheet
Full git cheatsheet based on Kevin Skoglund's Git Essential Training on Lynda.com

## Installing Git
[Instructions on setting up Git](https://help.github.com/articles/set-up-git/)

## Getting Started
1. Create project folder
2. Navigate into the root of your project folder
3. Initialize Git in your project: `git init`
4. Make changes to your project
5. Add all changes that have been made in the project (stage your changes): `git add .`
6. Add only certain files with changes to the staging index: `git add filename.fileextention`
7. Commit your staged changes with a message: `git commit -m 'message here'`
8. View commits you've made: `git log`
     - Return most recent 5 commits: `git log -n 5`
     - Return all commits made since August 9th, 2016: `git log --since=2016-08-09`
     - Return all commits made up until August 9th, 2016: `git log --until=2016-08-09`
     - Return all commits made by a certain author: `git log --author="Name"`
     - Return all commits with "Init" in the commit message (case sensitive): `git log --grep="Init"`

