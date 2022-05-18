# Git-Notes
A repository for my notes on Git Version Control and Git Related Things

### GETTING STARTED

* In your selected folder type `git init` to initialize folder and contents for git version control

### UNDERSTANDING GIT ENVIRONMENTS

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

### IGNORING FILES

- __*Why Ignore Files?*__:
   - Sensitive Info
      - Passwords
      - API Keys
      - Authentication Tokens
   - Personal Notes
   - System Files

- __Using .gitignore__:
   - Create .gitignore file in root directory of project structure
   - Add any file or pattern to .gitignore file

```python
.DS_Store
.vscode/
authentication.js
node_modules
notes/
**/*-todo.md
```

- __GLOBAL IGNORE FILE__:
   - `git config --global core.excludesfile [file]`
      - If you have a lot of projects you can add this as a config variable pointing to a file on your hard drive
      - Can add anything to harddrive and this file will pick it up automatically

- __CLEARING THE CACHE__:
   - `git rm -r --cached .`
      - If you add something to .gitignore file after you done a lot of commits, sometimes the way git caches a lot of information locally will get dirty:
         - Use the above code (with period for the current folder) to delete all the files that are cached recursively
         - You may have to do another add or commit

### DELETING AND RENAMING

- **DELETING WITH** `git rm [file]`
   - This removes a file and automatically stages the changes ready for committing
      - If you want to restore, use the following:
         - `git restore -S [file] (or . )`
         - Then `git restore .` again to put back file

- __Renaming file__:
   - If you rename a file and hit `git status` you will see the original file deleted and a new file with the new name made.
      - If you want to __restore__ the file with the new name, this gets a little complicated:
         - Hit `git restore .` and you will have both the original and renamed file.
            - Just simply delete the renamed file

   - __`git mv [file] [renamed file]`__:
      - A git command for renaming a file (**MUCH BETTER**)
         - This renames the file and automatically stages the change ready for committing
            - If you want to **restore** then use the same command above and rename the renamed file to the original name
