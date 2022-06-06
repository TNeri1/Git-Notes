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

### DIFFERENCES

- `git diff`:
   - Shows the differences between files
   - When moving files (i.e. deleting a file and adding another file) `git diff` will show everything
      - Use Visual Studio Code's **Source Control Editor**

- `git log --oneline`:
   - Better and simpler way to do `git log`
   - `git diff [commit hash from git log --oneline]`
      - Shows the differences between two commits
         - Not as useful, would suggest using Git Lens from Visual Studio Marketplace 

### CHANGING HISTORY

- **Amending**:
   - You can often end up committing something that's not quite right
      - You can create a new commit **BUT** that ends up creating a messy history
      - **TRY THIS INSTEAD TO ADD THINGS TO THE LAST COMMIT:**
         - __`git commit -amend`__
            - This will launch the default editor and allow you to edit the file with the amended history
         - __`git commit -am 'New commit message'`__
            - If you don't want to do the above explanation
         - __`git commit -amend --no-edit`__
            - If you just want to leave the message the same as the last commit

         - __`git reset [hash from git log --oneline]`:__
            - Let's you go back to a previous commit (like a rewind feature)
               - It will undo some of the changes (unstage the changes)
               - Moved back the head to another commit
                  - Didn't do anything to the files (Did not delete, just rewinded the commit)
               - You can then do a `git status` -> `git add .` -> `git commit -m "Commit Message"` -> `git log --oneline`

         - __`git reset --hard [hash from git log --oneline]`:__
            - **A LITTLE DANGEROUS**
            - Will delete any commits before the given hash id **AND** change all the files
               - Sometimes you want to do this to a specific commit to rewind time to a specific commit and continue from there

### CHANGING HISTORY USING REBASING:

- Designed to take the commits of one branch and apply them to another
   - **HOW TO USE REBASE**:
      - `git rebase <branch>/<commit>`
      - `git rebase --interactive <branch>/<commit>`
      - `git rebase -i HEAD~#`
         - **Really useful and best way to use rebase!!!**
         - Use the HEAD pointer to modify the current branch's directory
            - Rebase by moving back to a certain number of commits
               - *USEFUL IF YOU HAVE AN EXTREMELY LONG COMMIT HISTORY AND YOU ONLY WANT TO BACK A FEW STEPS*
      - `git rebase -i --root`
         - Another great way of editing branches in a text editor that displays all the branches
      
      - **When in text editor, simply cut the line of commit and paste in another place among the commits**
         - You can use some commands instead of `pick` (i.e. You will see `pick [hash id]`) such as `s` (`s [hash id]`) for squashing a commit into the previous one **OR** use `f` (`f [hash id]`) for fixing up (like squash) but not using the commit message of the commit being "squashed"

### BRANCHES:

- **ONE OF THE MOST POWERFUL FEATURES OF GIT**
   : Let's you create different versions of your code so you can play around with adding new features without worrying about messing up what's stable
   - **HOW TO USE**:
      - **LOOKING AT BRANCHES**
         - `git branch`
            - `* main`
      - **COPYING A BRANCH**
         - `git switch -c NAME`
         - **(ALT)** `git checkout -b NAME`
      - **MERGING**
         : Merges changes from one branch into the current branch (For example: __Meaning switch back to main branch and then execute this command with branch you want merged into main__)
         - `git merge <branch>`
      - **Deleting a Branch**
         : When you're done with a feature or a fix, it's a good idea to delete the branch you no longer need.
         - `git branch --delete NAME`
         - `git branch -d NAME`
            - You can use this as long as the branches are free of conflicts
         - `git branch -D NAME`
            - This forces git to ignore things and just delete the branch

- **GIT FLOW**:
   - Feature/Fix Branch
   - Make Changes
   - Merge to Master
   - Delete Old Branch