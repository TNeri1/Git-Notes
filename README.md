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
         - __`git commit --amend`__
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
   - **HOW TO USE:**
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

- **GIT FLOW:**
   - Feature/Fix Branch
   - Make Changes
   - Merge to Master
   - Delete Old Branch

### MERGE CONFLICTS:
: Conflicts happen when you're merging two branches, but you or someone has made changes to items in the same file
- When you merge branches or merge a branch to main you have to choose which of the changes you would like to keep.

### STASH AND CLEAN:
- **Stashing Code:**
   : Stashing is a way of putting away code temporarily so that you can work on something else.
      - i.e. Boss wants you to make important change but you were already working on new changes on the current branch you're in.
         - You want to restore everything but want to not lose any of the changes you have made
   - `git stash`
      : Takes whatever the changes were and temporarily put them in a storage facility
      : A quick way of undoing the work that you've done so that you can put into stashes and come back to
   - `git stash list`
      : Allows you to take a look at what has been stored
   - `git stash apply`
      : Allows you to apply a stash set of changes
   - `git stash pop`
      : Removes the git stash from the list

- **GIT CLEAN:**
   : Lets you remove all untracked files and directories from your branch super quickly
   : Nice way of removing files that you don't need anymore
   : A lot of the times when you do a git restore, because it doesn't do anything to untracked files, it will leave them there. So sometimes you need a perfectly clean folder.
      : Be careful with it, because it can sometimes remove things you don't want it to remove, **that's why it is a good thing to do the `-n` option so you know what it's going to remove**
   - `git clean -n`
      - dry run 
      - If you don't want to destroy things
   - `git clean -d`
      - directories
      - subdirectories
   - `git clean -f`
      - force
      - When you actually want to do a cleaning that is not a dry run
   - **You can also combine flags:**
      - `git clean -dn`
      - `git clean -df`

### GITHUB:

- **REMOTES:**
   - `git remote add NAME URL`
   - `git remote remove NAME`
   - `git rename OLDNAME NEWNAME`
   - `git remote -v`

- **GIT PUSH:**
   - `git push REMOTE BRANCH`
   - `# git push --set-upstream-to origin main`
   - `git push -u origin main # --set-upstream`
   - `# git push --all`
   - `# git branch --set-upstream-to <origin/remote-branch>`

- **MANAGING PROJECTS:**
   - Contributors
      - Can invite others to work along side you
   - Issues
      - Create notes about your proejct that can be shared and ccommented by others, usually for things you need to fix
   - Labels
      - A way to organize issues, that help to identify the purpose of the issue
   - Milestones
      - Are for grouping issues into goals that need to be achieved
   - Projects
      - A way to take a look at the progress of your work and see how well you're accomplishing your goals

# GIT INTERMEDIATE TECHNIQUES

### BRANCH MANAGEMENT:

- **FORCE PUSH TO A REMOTE:**
   : **WHY**: 
   If you find yourself in a state where you fetch additional commits from collaborators from the remote server and find that they were wrong or messed things up in a big way. So now we're going to force push instead (Ignoring the differences in the remote version and replace the remote version with my local version)
   - Local version is better than the remote version
   - Remote version went wrong and needs repair
   - Versions have diverged and merging is undesirable
   - __Use with **EXTREME CAUTION**:__
      - Easy way to anger your whole development team
      - Disruptive for others using the remote branch
      - Commits disappear
      - Subsequent commits are orphaned
   - __HOW TO:__
      - `git push -f`
      - `git push --force`
   - __AFTERMATH:__
      - Have other collaborators do the following command:
         - `git reset --hard <branch>`
            - EXAMPLE: `git reset --hard origin/master`

- **IDENTIFY MERGED BRANCHES:**
   - List branches that have been merged into a branch
   - Useful for knowing what features have been incorporated
   - Useful for cleanup after merging many features
   - __HOW TO:__
      - `git branch --merged`
         - Git will give a list of branches in local repository which have been merged into the current branch fully
      - `git branch --no-merged`
         - This will return the branches that have not yet been merged into the current branch (Or to put another way, they contain commits which have not yet been added to the current branch)
      - `git branch -r --merged`
         - Lists remote branches that are either merged or unmerged with the current branch
         - __HOW IT WORKS:__
            - Use current branch by default
            - "Branches whose tips are reachable from the specified commit (HEAD if not specified)"
            - Branch tip is in the history of the specified commit
            - Can specify other branch names or commits:
               - `git branch --merged HEAD`
                  - EXAMPLE: `git branch --merged july_release`
                     - Starts at the tip of july_release and work backwards down the history to see which other branches' tips show up in the history
                  - EXAMPLE: `git branch --merged origin/july_release`
                     - Same as above except for remote repositories
                  - EXAMPLE: `git branch --merged b325a7c49`
                     - Same as above two except just specifying a particular commit.
                     - We can say, if we start at this commit and work backwards in the history, how many of the other branch tips are we going to find?