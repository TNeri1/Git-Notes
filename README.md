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

- **DELETE LOCAL AND REMOTE BRANCHES:**
   : In order to delete a branch you must first make sure that you're on a different branch
      - You can't remove a branch you're currently on (**It's like removing the ground from beneath your feet**)
   - **HOW TO (LOCAL):**
      - ```sh
         # Delete branch
         # (Must be on a different branch)
         git branch -d new_feature
        ```
         - Code above deletes `new_feature` branch **PROVIDED** that the `new_feature` branch has been fully merged into the current branch.
            - **IF NOT**, Git's going to object, and say there are commits that are over there in this branch you're about to delete that are not yet in this branch, **you might be about to make a big mistake**
               - If you really want to delete that branch use the following:
                  - ```sh
                     # Delete not yet merged branch
                     git branch -D new_feature
                    ```
      - **HOW TO (REMOTE):**
         - ```sh
            # Delete remote branch

            git push origin :new_feature
            # git push origin <local>:<remote>
            """
            Above code is saying push nothing up to the remote branch
            """

            # Delete remote branch v1.7.0+
            git push --delete origin new_feature

            # Delete remote branch, v2.8.0+
            git push -d origin new_feature
           ```

- **PRUNE STALE BRANCHES**
   - Delete all stale remote-tracking branches
   - **Remote-tracking branches, NOT remote branches**
   - Stale Branch:
      - __a remote-tracking branch that no longer tracks anything because the actual branch in the remote repository has been deleted__
   - **REMOTE BRANCHES:**
      1. Branch on the remote repository (bugfix)
      2. **Local snapshot of the remote branch (origin/bugfix)** __<-- This is the remote-tracking branch__ 
         - It's what happens when we call git fetch
            - Fetch the changes from the remote repository and sync up our tracking branch so that the same changes are there
            - Allows us to work offline, off of the origin
               - Even though we can't fetch or push to the remote, we still can access the contents of what was up there
      3. Local branch, tracking the remote branch (bugfix)
   - *Deleting local branches (3) and remote branches (1) but what about number (2)?:*
      - Deleting a remote prune the remote-tracking branch automatically at the same time. (1) and (2) get deleted together
      - **However, it is necessary when collaborators delete branches**
         - EXAMPLE: If a collaborator deletes a remote branch, then your tracking branch is going to be out of sync
      - Fetch does **NOT** automatically prune (You still have the tracking branch). It's up to you to remove it.
         - **THIS IS THE NEED FOR PRUNING:**
            - ```sh
               # Delete stale remote-tracking branches
               git remote prune origin

               # Run below code after above code to see what it would do before you actually let it do it
               git remote prune origin --dry-run

               # Shortcut: prune, then fetch
               git fetch --prune
               git fetch -p

               # Configuration to Always prune before fetch
               # This is not the default because it destructive option.
               git config --global fetch.prune true

               # Prune all unreachable objects
               # DO NOT USE!!!
               git prune

               # Part of garbage collection
               git gc
              ```
               - **`git prune` and `git remote prune` are two totally different things**
                  - `git prune` prunes all unreachable objects from git, **THERE IS NO NEED TO USE IT**, because it's part of the garbage collection done by git using the following:
                     - `git gc`
                        - Basically just says clean up my git repository and throw all the stuff that's not being used.
                        - All those temporary objects not being used anymore can be trashed.

### TAGGING:

- **CREATE TAGS:**
   - Tags allow marking points in history as important
   - A named reference to a commit
   - Most often used to mark releases (v1.0, v1.1, v2.0)
   - Can mark key features or changes (ecommerce, redesign)
   - Can mark points for team discussion (bug, issue 136)
   - **TWO TYPES OF TAGS YOU CAN CREATE:**
      - ```sh
         # ADD LIGHTWEIGHT TAG
         git tag issue136 655da716e7

         # ADD ANNOTATED TAG (MOST COMMON)
         git tag -a v1.1 -m "Version 1.0" dd5c49428a0
        ```
         - Use the second one (Annotated Tag):
            - It gives you the ability to provide a message to go with it
               - That message could be a simple one line message
               - Or it can be a multi line message
            - **`-a` option means that it should be an annotated tag**
            - **`-m` option means to provide the message that shoudl be used for the annotation**
            - Last two parts are the name of the tag followed by the sha of the commit we want to reference

- **LIST TAGS:**
   - ```sh
      # List tags
      git tag
      git tag --list
      git tag -l

      # List tags beginning with "v2"
      git tag -l "v2*"

      # List tags with annotations
      git tag -l -n
      git tag -ln

      # Work with tags (like SHAs)
      git show v1.1

      # See all the code changes between version 1.0 and version 1.1
      git diff v1.0..v1.1

     ```

- **DELETE TAGS:**
   - ```sh
      # Delete a tag
      git tag --delete v1.1
      git tag -d v1.1
     ```
      - **BE CAREFUL, ONCE YOU DELETE A TAG, IT'S PERMANENTLY GONE**

- **PUSH TAGS TO A REMOTE:**
   - Like branches, tags are local unless shared to a remote
   - **`git push` does NOT transfer tags**
      - This is good because sometimes you can have a bunch of tags that you don't want to share with other people, and can just be for your convenience
   - Tags must be **EXPLICITLY** transferred
   - **`git fetch` automatically retrieves shared tags**
   - **HOW IT WORKS:**
      - ```sh
         # Push a tag to a remote server
         git push origin v1.1

         # Push all tags to a remote server
         git push origin --tags

         # Fetch commits and tags
         git fetch

         # Fetch only tags (with necessary commits)
         git fetch --tags

         # Delete remote tags like remote branches
         git push origin :v1.1
         git push --delete origin v1.1
         git push -d origin v1.1
        ```

- **CHECKING OUT TAGS:**
   - Tags are **NOT** branches
      - Tags simply identify a particular commit in your history
   - Tags can be checked out, just like any commit
   - The right way to check out a tag is to create a branch from it
      - ```sh
         # Right way
         git checkout -b new_branch v1.1
         
         # Also correct BUT has implications
         git checkout v1.1
        ```
         - Checking out a commit puts the local repository in a "detached HEAD state"
         - Like being on an unnamed branch
         - New commits will not belong to any branch
         - Detached commits will be garbage collected (~2 weeks)
      - **3 WAYS TO PRESERVE YOUR WORK:**
         - ```sh
            # Tag the commit (HEAD detached)
            git tag temp

            # (EVEN BETTER) Create a branch (HEAD detached)
            # Downside is the HEAD is still in detached state
            git branch temp_branch

            # (BEST) Create a branch and reattach HEAD
            git checkout -b temp_branch
           ```

### INTERACTIVE STAGING

- **INTERACTIVE MODE:**
   - Stage changes interactively
   - Allows staging portions of changed files
   - Helps to omake smaller, focused commits
   - Feature of many Git GUI tools
   - **HOW TO:**
      - ```sh
         git add --interactive

         git add -i
        ```
   - **Staging:**
      - update == add
      - ![](pictures/git_int_ch3-1.png)
   - **Unstaging:**
      - revert == unstaging/checkout
      - ![](pictures/git_int_ch3-1_unstaging.png)
   - **Untracked Files:**
      - a == add untracked files
      - ![](pictures/git_int_ch3-1_adduntracked.png)
   - **Diff:**
      - d == diff
      - ![](pictures/git_int_ch3-1_diff.png)
   - **Help and Quit:**
      - ![](pictures/git_int_ch3-1_help_quit.png)
   - **Remember that this is INTERACTIVE STAGING. You still have to make commits. This just helps you with the staging.**

- **PATCH MODE:**
   - Allows staging portions of a changed file
      - If there are portions to be changed, it assumes that we have multiple changes inside the same file
      - Git is going to decide how to break up those changes into portions called:
         - "Hunk":
            - An area where two files differ
         - Hunks can be staged, skipped, or split into smaller hunks
   - **Staging Portion**
      - ![](pictures/git_int_ch3-2_portion.png)

   - **(SIDE NOTE: `git diff --cached`)**
      - Let's you see the difference in the files that are in your staging area versus what's in HEAD
      - ![](pictures/git_int_ch3-2_diff_cached.png)
   - **PATCH MODE NOTE JUST IN INTERACTIVE MODE:**
      - ```sh
         git add --patch
         git add -p

         git stash -p
         git reset -p
         git checkout -p
         git commit -p
        ```

- **SPLIT A HUNK:**
   - Hunks can contain multiple changes
   - Tell Git to try to split a hunk further
   - Requires one or more unchanges lines between changes
   - **HOW TO:**
      - ![](pictures/git_int_ch3-3_split_hunk.png)

- **EDIT A HUNK:**
   : **WHY**
      - The ability to add portions of a file to the staging area is a powerful feature, that can greatly improve your workflow, which can allow to make better and more **ATOMIC** commits.
   - Can edit a hunk manually
   - Most useful when a hunk cannot be split automatically
   - Diff-style line prefizes: +, -, #, space
   - **HOW TO:**
      - e == edit manually
         - This will open up a text file
         - Change the prefixes and resort the lines accordingly
      - ![](pictures/git_int_ch3-4_edit_hunk.png)
      - ![](pictures/git_int_ch3-4_see_diffs.png)

### SHARE SELECT CHANGES

- **CHERRY-PICKING COMMITS:**
   : **WHY**
      - Imagine you have a branch, master, that has six commits on it, and at some point you started a new feature branch (third commit in master) that comes off one of those commits. But you like some code in the fifth commit in master and want to bring it in your new branch without merging because you don't want the other commits. 
      - You just want to grab one little bit of code that's in that one commit and bring it in to your feature branch
         - **Cherry-picking solves this: Allows you to reach in and grab one commit and apply its change sets to your new feature branch**
            - The results is a new commit with those same changes
            - They'll have identical changes between them even though they have different SHAs that reference them
   - Apply the changes from one or more existing commits
   - Each existing commit is recorded as a new commit on the current branch
   - Conceptually similar to copy-paste
   - New commits have different SHAs
   - **HOW TO:**
      - ```sh
         # With one SHA
         git cherry-pick d4e8411d09

         # With a range of SHAs
         git cherry-pick d4e8411d09..57d290ec44
        ```
         - You can cherry-pick commits from any branch that you have visible to you and even from things that are on the remote
         - **CANNNOT CHERRY-PICK A MERGE COMMIT**
            - *A merge commit is merging together two other sets of commits*
            - *If we try to cherry-pick a merge commit then git __wouldn't__ know which one of those parents that it was joining should be brought into the current branch*
         - Use `--edit` or `-e` to edit the commit message
            - *By default it will use the same commit message as a convenience, but you have the opportunity to edit it, __but you have the opportunity to edit it if you want something different__*
         - **Can result in conflicts which must be resolved**

- **RESOLVE CHERRY-PICKING CONFLICTS:**
   - ![](pictures/git_int_ch4-2_resolve_cherrypick_conflicts.png)

- **CREATE DIFF PATCHES:**
   - Share changes via files
      - Instead of transferring them via a "get remote repository"
   - **Useful when changes are not ready for a public branch**
   - **Useful when collaborators do not share a remote**
   - Often used during a discussion, review, approval process
      - *The idea is just to package up changes into files and then those files can either be emailed or put into a thumb drive or something to be shared with someone else*
   - ```sh
      # Direct output to a file
      echo "Hello" > temp.txt

      git diff [from-commit] [to-commit] > output.diff
     ```
   - ![](pictures/git_int_ch4-4_diff_p1.png)
   - ![](pictures/git_int_ch4-4_diff_p2.png)

- **APPLY DIFF PATCHES:**
   - **WHY:**
      - (Scenario) There is branch that does not have your commits. This might be someone else's branch on the other side of town. We're going to take that file, which we've exported. We've got our diff patch and we're going to put it in on a thumb drive and take it over to their office and they're going to plug it in, or maybe we're going to email it to them and they're going to take that file and they want to apply it now to their repository.
   - Apply changes in a diff patch file to the working directory
   - Makes changes, but not commits
   - No commit history transferred
      - A diff file has not commit history in it
      - *All you're getting is a set of changes*
   - **HOW TO:**
      - ```sh
         git apply output.diff
        ```
      - ![](pictures/git_int_ch4-4_apply_diff_patches.png)
      - Doesn't always work quite so smoothly
         - **A patch can only apply if your working directory is in a state that allows it to work**
            - It doesn't have to match everything exactly
            - There can be plenty of files that don't conflict.
            - Anything that conflicts,anything that keeps it from matching up exactly, is going to prevent the patch from applying cleanly

         - ![](pictures/git_int_ch4-4_apply_diff_patches_not_smooth.png)

- **CREATE FORMATTED PATCHES:**
   - Export each commit in UNIX mailbox format
      - *WHY USE UNIX MAILBOX FORMAT?:*
         - It's the standard format it uses
         - Useful for email distribution of changes
   - Includes commit messages
      - And some metadata about the commit
   - One commit per file by default
   - **HOW TO:**
      - ```sh
         # Export all commits in the range
         git format-patch 2e33d..655da
        ```
         - Like `git diff` but this will create individual files for each one of these commits
         - If you leave out either one of the SHAs in the range, the it would assume that we meant the current head
      - ```sh
         # Export all commits on current branch
         # which are not in master branch
         git format-patch master
        ```
         - Starts with HEAD, because HEAD is implied as being the second part of the range, and it'll work its way backwards up the history until it gets to master
            - When it gets to a commit that btoh this branch and master have, that's the point at which it'll stop
            - Gets everything since you've diverged from master
      - ```sh
         # Export a single commit
         git format-patch -1 655da
        ```
         - The `-1` option says we just want to target this single commit
            - You should not think that I somehow want to go from the current HEAD all the way back until you get to commit 655da
         - (Scenario) Imagine you're going to output something that has 20 different commits
            - *You don't want to just create 20 different files, and you don't want to create them in your repository*
            - **Instead you want to say where to put those files:**
               - ```sh
                  # Put patch files into a directory
                  git format-patch master -o feature
                 ```
                  - The `-o` option tells what directory to put these files into.
                     - If the directory doesn't exist, it'll create it
      - ```sh
         # Put patch files into a directory
         git format-patch master -o feature

         # Output patches as a single file
         git format-patch 2e33d..655da --stdout > feature.patch
        ```
         - When you provide the `--stdout` option, it says **don't create the files like you normally would**
            - Instead, take all of this data and just stream it out as one long string to stdout (stdout normally be the screen by default)
         - File ending should be called `.patch` 
            - That's the name you would normall give to the files that would be inside the directory they would create
      - ![](pictures/git_int_ch4-5_formatted_patches.png)

- **APPLY FORMATTED PATCHES:**
   - Extract author, commit message, and changes from a mailbox message and apply them to the current branch
   - Similar to cherry-picking: same changes, different SHAs
      - The biggest difference between a formatted patch and a diff patch is that the **COMMIT HISTORY IS TRANSFERRED**
   - **HOW TO:**
      - ```sh
         # Apply single patch
         git am feature/0001-some-name.patch

         # Apply all patches in a directory
         git am feature/*.patch
        ```
         - `am` is short for "apply mailbox" or "apply mailbox formatted patch"
   - ![](pictures/git_int_ch4-6_apply_formatted_patches.png)
   - **Formatted patches are more useful most of the time**
      - Diff patches can serve a purpose, if we don't want to actually give someone the commits, we just want to give them a set of changes, then diff patches is the way to go
      - If you're tryingt to give someone the commits (in the same way you would if you were cherry-picking) then go with the formatted patch instead

### REBASING

- **REBASE COMMITS:**
   - Take commmits from a branch and replay them at the end of another branch
   - Useful to integrate recent commits without merging
   - Maintains a cleaner, more linear project history
   - (__IF WORKING WITH A TEAM OF DEVELOPERS__) Ensures topic branch commits apply cleanly
      - Forces each developer to take responsibility, and make sure that there won't be unnecessary merging or merge conflicts that have to be resolved
      - Some large teams actually require rebasing for any topic or feature branch before it can be considered for incorportion into the master branch
   - (Scenario):
      ![](pictures/git_int_ch5-1_pic_1.png)
      - There's a branch called master and we've started a new topic or feature branch called new_feature.
         - After starting your branch you have made commits and other people have been working on the master branch and new commits have been submitted to master branch
         - Now, we want to incorporate those new commits into your new_feature branch, not just because you want to make use of them but becuase **you want to make sure that the commits that are in your new_feature branch will fit cleanly with what's there**
            - So you do a **REBASE**
            - **REBASE** takes the commits in your new_feature branch, and it shifts them down to the tip of the master branch

      ![](pictures/git_int_ch5-1_pic_2.png)

      - When git generates a SHA to identify each one of these commits, it's based not only on the change set and the metadata of that commit, but also on the parent commit as well
         - So by moving these to the end, the SHAs that identify each of the commits will change
            - It's the same change set, but the identifiers have changed
      - **HOW GIT REBASES WORK (DETAILED):**
         - Git starts rewinding the new feature branch, or picking up each one of those comits and putting them into temporary storage
         - It keeps doing that until it gets back to a commit where it diverged from the master branch
      ![](pictures/git_int_ch5-1_pic_3.png)
         - Then, it moves to the tip of the master branch and it replays each one of those commits
      ![](pictures/git_int_ch5-1_pic_4.png)
         - So what Git is actually doing is picking up each one of those commits, one-by-one, and then replaying them at the end
         - **You don't just have to move it to the tip of master, you can rebase to the tip of other branches as well**
      ![](pictures/git_int_ch5-1_pic_5.png)
   - **HOW TO:**
      - ```sh
         # Rebase current branch to tip of master

         # The last parameter is the branch 
         # you want to use as new base
         git rebase master 

         # Rebase new_feature to the tip of master
         git rebase master new_feature

         # Useful for visualizing branches
         git log --graph --all --decorate --oneline

         # Return commit where topic branch diverges
         git merge-base master new_feature
        ```
         - *What Git's actually doing behind the scenes is it does an automatic checkout of `new_feature`, and then it does the regular rebase*
         - **This syntax is ONLY for moving new_feature from some commit in master's history to the current tip of `master`**
            - Rebasing to another point is covered down below in the notes
   - ![](pictures/git_int_ch5-2_pic_1.png)

- **MERGING VS REBASING:**
   - Two ways to incorporate changes from one branch into another branch
   - Similar results, but the means are different
   
   ![](pictures/git_int_ch5-3_pic_1.png)
   ![](pictures/git_int_ch5-3_pic_2.png) 
   - **(TOP HALF OF PICTURE)** If you want to incorporate the recent changes in the master branch into the new feature branch you could merge them in. And you would do that by creating a merge commit that woudl then link those commits into the current branch
   - **(BOTTOM HALF OF PICTURE)** If you instead perform a rebase, you would essentially taking the commits that are in the new feature branch and shifting them to the tip of the master branch so that those recent commits in master were now available to you. *Remember also though, that when you replay these commits at the tip of the branch, it's actually going to change the SHAs of those commits at the same time. It's actually recommitting them for you. The fact that it changes the SHAs and some metadata is super important*

   - **MERGING:**
      - Adds a merge commit
      - Nondestructive
      - Complete record of what happened and when it happened
      - Easy to undo (*Just do a hard reset to undo the merge*)
      - (**DOWNSIDE**) Logs can become cluttered, non-linear
         - (EXAMPLE): If you author a topic branch and then wait a whole month to merge it in. The merge commit that merged it in will be one month later in the log than those original commits.
   - **REBASING:**
      - No additional merge commit
      - **Destructive**: SHA changes, commits are rewritten (*Not such a big deal if it happens cleanly, __BUT__ become a bigger deal if there are conflicts in the process*)
      - No longer have a complete record of what happened and when
      - Tricky to undo
      - (**BENEFITS**) Logs are cleaner, more linear
   - **THE GOLDEN RULE OF REBASING:**
      - **THOU SHALT NOT REBASE A PUBLIC BRANCH**
         - Rebase abandons existing, shared commits and creates new, similar commits instead
         - Collaborators would see the project history vanish
         - Getting all collaborators back in sync can be a nightmare
         - **REBASING SHOULD BE USED ON YOUR LOCAL, PRIVATE BRANCHES OR BRANCHES THAT YOU USE EXCLUSIVELY; NOT BRANCHES THAT OTHERS ARE USING**
   - **HOW TO CHOOSE?**
      - **MERGE** to allow commits to stand out or to be clearly grouped
      - **MERGE** to bring large topic branches back into master
         - *The merge is a significant event and you want to have it clearly indicated in the history that you merged topic branch into master*
      - **REBASE** to add minor commits in master to a topic branch
         - *So that you can make sure that your topic branch still works well with what's going on in the master branch*
      - **REBASE** to move commits from one branch to another
         - *How to do that will be covered later*
      - **MERGE** anytime the topic branch is already public and being used by others (**THE GOLDEN RULE OF REBASING**)

- **RESOLVE REBASE CONFLICTS:**
   - Rebasing creates new commits on existing code
   - May conflict with existing code
   - Git pauses rebase before each conflicting commit
   - Similar to resolving merge conflicts
   - **HOW TO:**
      - ```sh
         # Says I resolved this commit, next!
         git rebase --continue

         # Throw out current commit, keep goin!
         # Use this most often when underlying 
         # code already contains the change
         # that's in your conflicting commit
         # and you'd like to use the existing 
         # one instead of yours
         git rebase --skip

         # Stop the rebase altogether
         # Code will be just like it was before
         # you did the rebase
         git rebase --abort
        ```
   ![](pictures/git_int_ch5-4_pic_1.png)

- **REBASE ONTO OTHER BRANCHES:**
   ![](pictures/git_int_ch5-1_pic_5.png)
   - **HOW TO:**
      - ```sh
         # git rebase --onto [newbase] [upstream] [branch]
         git rebase --onto master ecommerce new_feature
        ```
         - `upstream` is basically saying if you travel upstream, *how far should you go until you stop picking up commits?* *At what point did you branch off something else?*
            - (AKA) *Gather up the commits since the new_feature branch diverged from the ecommerce branch, then replay those commits as if they were based off the current tip of the master branch*
   - Create the scenario for rebasing
   ![](pictures/git_int_ch5-5_pic_3.png)
   - Illustration of where we are
   ![](pictures/git_int_ch5-5_pic_1.png)
   - Illustation of what we want to do
   ![](pictures/git_int_ch5-5_pic_2.png)
   - Rebasing showing both rebasing expenses to master and back to original placement
   ![](pictures/git_int_ch5-5_pic_4.png)