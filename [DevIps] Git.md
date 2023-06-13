## Introduction to GIT

- Content Tracker
- Distributed Version Control System
- Remote Repository
- Local Repository
  - Working Area
  - Staging Area
  - Committed Files

## Installing GIT

- Check OS version

  ```
  cat /etc/*release*
  ```

- Install Git

  ```
  sudo apt update
  sudo apt install git -y
  ```

- Identify the version of Git

  ```
  git --version
  git verson 2.17.1
  ```

## Initialize GIT Repository

- Initialize Git

  ```
  git init
  ```

- Check Git status

  ```
  $git status
  On branch master
  No commits yet
  
  Untracked files:
    (use "git add <file>..." to include in what will be committed)
          .bashrc
          .vimrc
          learning-app-ecommerce/
  
  nothing added to commit but untracked files present (use "git add" to track)
  ```

- Add new file to Staging Area

  ```
  git add story1.txt
  ```

-  Commit files

  ```
  git commit -m "Added first story"
  ```

- Ignore files

  ```
  echo notes.txt >> .gitignore
  ```

- Check log

  ```
  git log --oneline
  ```

  

## GIT Branches

- Create branch

  ```
  git branch xxx
  ```

- Switch branch

  ```
  git checkout -b xxx
  ```

- Check branch

  ```
  git branch
  ```

- Delete branch

  ```
  git branch -d xxx
  ```

- Merge branch

  ```
  git merge
  ```

## Working as a team with Remote Repositories

- Add remote repository

  ```
  git remote add origin https://..../[name].git
  ```

- List all the remote repositories

  ```
  git remote -v
  ```

## Cloning, Pushing, Pulling & Fetching

- Pushing

  ```
  git push origin master https
  ```

- Cloning

  ```
  git clone git@github.com:account/remote-repo.git
  ```

  



## Pull Requests

## Merge conflicts

## Forks

## Rebasing & Interactive Rebasing

## Cherry Picking

## Resetting and Reverting

## Stashing

## Reflog

## Understanding GIT