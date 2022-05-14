# Git-Notes
A repository for my notes on Git Version Control and Git Related Things

### Getting Started

* In your selected folder type `git init` to initialize folder and contents for git version control

### Understanding Git Environments

-  __`git log`__
   - Lists commit hash, author, email, date, HEAD

- __Git Environments__:
   - Working
   - Staging
   - Commit

- __File States__:
   - Tracked: 
      - Files that existed in the previous snapshot
      - __States__:
         - Unmodified
            - Files have not changed since last commit
         - Modified
            - Files have changed since last commit
         - Staged
            - File(s) moved into staging area
   - Untracked: 
      - Anything else (i.e. new file added since last commit)

- __`git status`__

- __`git add { . | -A | -All }`__

- __`git restore { . | <FILENAME> }`__

- __`git commit -m "<COMMIT MESSAGE>"`__

