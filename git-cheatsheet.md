# GitHub Ultimate: Master Git and GitHub - Beginner to Expert

## Useful Unix Commands

```
pwd
cd <folder-name>
ls -al
clear  # clear command line
rm -rf <folder-name>
rm <file-name>
```

## Git Set-up

```
git version
git config --global --list  # see configurations
git config --global user.name "armandolara24"
git config --global user.email "armandolaramail@gmail"
```

Install Nodepad++ and append exe file to PATH.
Restart Git Bash and create or update `~/.bash_profile` in user's home directory:

```
notepad++ ~/.bash_profile
```

Then add a line such as below:

```
alias npp='notepadd++ -multiInst -nosession'
```

To configure Notepad++ as Git default editor execute:

```
git config --global core.editor "notepad++ -multiInst -nosession"
```

To test Notepad++ integration execute:

```
git config --global -e
```

Install P4Merge (visual merge tool), during installation only select Visual Merge Tool.

```
git config --global diff.tool p4merge
git config --global difftool.p4merge.path "C:/Program Files/Perforce/p4merge.exe"
git config --global difftool.promt false
git config --global merge.tool p4merge
git config --global mergetool.p4merge.path "C:/Program Files/Perforce/p4merge.exe"
git config --global mergetool.promt false
```

## Initialization

```
git init my-new-project  # create new repository
rm -rf .git              # remove .git and make git stop managing working directory
```

## Git States

- Working directory (local)
- Staging area (local)
- Repository (`.git` folder) (local)
- Remote (remote)

## First Commit

```
git status                               # get git status
git add <filename>                       # add file to staging environment
git add .                                # add all files in current directory to staging area
git commit -m "this is my first commit"  # commit with inline commit message
```

## Starting with Existing Project

```
git init .  # dot means current folder
```

## Commit and Messages

```
git commit                           # use the core editor configured with git for the commit message
git commit --amend -am "my message"  # amend a commit
```

## Commit Details with Log and Show

```
git log   # show all commits part of current repository
git show  # show the last commit and a diff containing all the changes, press q to exit
```

## Express Commit

```
git ls-files    # to check which files git is tracking
git commit -am  # -a add the file to staging area first, express commit.
```

## Back Out Changes

```
git restore <file-name>           # (modern) discard changes in a single file
git restore .                     # (modern) discard all changes
git restore --staged <file-name>  # (modern) untag a file but keep changes in working directory

git reset HEAD <file-name>        # (old) unstage a file but keep changes in working directory
git reset -- <file-name>          # (old) unstage a file but keep changes in working directory (same as above)

git checkout -- <file-name>       # (old) discard changes in working directory
git checkout .                    # (old) remove working directory, there is no way to bring all back.

git clean -xdf                    # remove files which are not added/tracked by git. d for directories, f for forcefully.
git reset HEAD~2                  # uncommit 2 commits.
git reset HEAD^^                  # uncommit 2 commits.
```

## History and Making New Commands with Alias

```
git log                                     # display commits history
git log --oneline
git log --oneline --graph --decorate --all  # display branches history with graph-like format
git config --global alias.hist "log --oneline --graph --decorate --all"
git hist                                    # run command defined in the line above

git help log                                # display log command options
git config --global --list                  # display configurations
```

## Rename and Delete Files

```
git mv oldname.txt newname.txt  # rename commited files; a new commit is necessary after this

git rm file.txt                 # remove files with git; using this command instead of an OS command gives the extra benefit of deleting the tracking as well. A commit after this command is necessary.
```

## Managing Files Outside Git

```
git add -u  # stage modifications and deletions to tracked files only (new/untracked files are not added)
git add -A  # unlike -u, also stage new/untracked files, in addition to modifications and deletions to tracked files
```

## Excluding Unwanted Files

```
npp .gitignore  # open (or create) .gitignore in Notepad++ using the npp alias defined earlier

git add .gitignore
git commit -m 'Adding .gitignore file'
```

Create a file named `.gitignore` to get git to ignore files, e.g., by `git add .`.
`.gitignore` file should include the names of the files or directories which should be ignored, each name in a new line (it also accepts wildcards), e.g.,

```
filename.txt
/dirname
*.log
*.orig
```

`.gitignore` has to be commited to repo in order to take effect.

## Advanced Section

### Create and Move to Branch

```
git switch -b <branch-name>    # (modern) create a new branch and move to it
git switch <branch-name>       # (modern) move to another branch

git checkout -b <branch-name>  # (old) create new branch and move to it
git checkout <branch-name>     # (old) change to another branch

git merge <branch-name>        # (while being in the branch where the changes will be added)
```

### Comparing Differences

```
git hist                                        # use the previously user-defined command to check full commit history; two commit ids are obtained from here

git diff <old-commit-id> <newer-commit-id>      # see differences between stage files and working directory files; the special HEAD pointer can be used. HEAD points to the last commit by default

git difftool <old-commit-id> <newer-commit-id>  # same as diff but with a tool

git diff                                        # without arguments, show the difference between HEAD and working directory

git difftool                                    # without arguments, show the difference between HEAD and working directory

git help diff                                   # for much more diff options, it's very powerful
```

### Simple Branching Example

A branch is a timeline of commits.

```
git branch                              # display branches
git branch -a                           # display all branches (including remote)
git switch -b <branch-name>             # create branch and move to the new branch
git diff                                # visualize difference between working directory and last commit
git diff <branch-one> <branch-two>      # visualize differences between branches
git switch master                       # move to master branch
git merge <branch-name>                 # execute from master branch to merge a certain branch to master branch. If after this, HEAD points to master and the branch then a fast-forward merge took place.
git branch -d <branch-name>             # delete branch locally
git push origin --delete <branch-name>  # delete remote branch
git mergetool                           # merge conflicts
git fetch -p                            # update local repository and delete local branches that were deleted remotely
```

### Conflict Resolution

After having purposely modified the same file in master and another branch so that while merging there is a conflict. After executing the command:

```
git merge <branch-with-conflict>
```

The command line will show `(master|MERGING)` indicating that command line is in merging state, additionally, the conflicting files names will be shown.
Catting the files:

```
cat <name-of-conclicting-file>
```

will show up which parts of the file are conflicting.

```
git mergetool  # execute previously configured merge tool
```

After saving the fixed changes, commit can be applied and command line then will change from `(master|MERGING)` to normal `(master)`.

### Saving Work in Progress with Stashing

```
git stash                  # hide WIP temporarily
git stash save "my description"
git stash list             # show stashes
git stash pop              # go back to the hidden WIP and delete the stash
git stash apply            # it doesn't delete from stash
git stash apply stash@{0}  # it doesn't delete from stash
git stash clean            # remove all stashes?
```

### Time Travel with Reset and Reflog

```
git reset --soft <commit-id>   # repoint HEAD, preserve working directory and staging area

git reset <commit-id>          # repoint HEAD and remove files from staging area
git reset --mixed <commit-id>  # repoint HEAD and remove files from staging area

git reset --hard <commit-id>   # repoint HEAD, remove files from staging area and working directory

git log --oneline              # show all logs until HEAD with one line only of each one of them
git reflog                     # display commit ids as "git log" but also show all previous actions
```

With reset and reflog it is possible to reset any commit made history.

### SSH

```
mkdir .ssh  # Execute this in users directory
cd .ssh/
ssh-keygen -t rsa -C "armandolaramail@gmail.com"
```

Then add the `id_rsa.pub` public key to GitHub website in account settings SSH.

```
ssh -T git@GitHub.com  # test connection to remote repository
```

### Creating a Local Copy with Clone

```
git clone https://GitHub.com/armandolara24/reponame.git                # when there is already a remote repository and we want to clone it locally
git clone https://GitHub.com/armandolara24/reponame.git <folder-name>  # specify folder name for local repository
git clone https://github.com/LinkedInLearning/javascript-essential-training-2832077.git
```

### Public Back to GitHub

```
git push origin master                   # commit to remote repo, then enter username and password

git push                                 # default behaviour depends on git version, in newer versions >=2.0 default is simple, which only pushes the current branch to the corresponding remote branch that 'git pull' uses to update the current branch.

git config --global push.default simple  # configure simple as default behaviour (just to make sure and hide warning)
```

### Fetch and Pull

```
git fetch  # fetch from remote repo; it is non-destructive. Run git status later to verify that git pull can be executed directly instead of git merge.

git pull   # fetch from remote repo and merge changes to current local repo
```

Doing a fetch/pull is best practice before doing a push.
A PULL is two commands in one, it is a fetch and merge.

### Backing Out Changes from Remote Repository

```
git revert <commit-id-to-remove>  # revert/remove a commit from remote repository (git push has to be executed after this). It creates a new commit which cancels out the previous commit.
git push
```

### Updating Repository and Remote References

```
git remote -v                                                      # check if there is a remote repository connected
git remote remove origin                                           # remove origin remote connection
git remote add origin https://GitHub.com/armandolara24/PyGaze.git  # in this case "origin" is the name given to the remote repository
git remote set-url origin git://new.url.here                       # re-direct remote connection
git remote show origin                                             # get additional info about origin
git push -u origin master --tags                                   # set up a tracking branch relationship between master branch in the local and the master branch in the remote repository. --tags send all the text in the local repository up to GitHub.
```

### Cleaning Up by Deleting Branches and References

```
git merge <branch-name>         # run from master to merge both branches
git push origin :<branch-name>  # delete <branch-name> branch in remote repo from local env; empty <src> before the colon means "push nothing", deleting <dst> on the remote
```

### Pull with Rebase

Rebase rewinds the current commits that are on your branch to a point where the branch you're merging in originally diverged; then playing back the commits that happened on the branch you're wanting to bring in; and then, after that, playing on top of that any commits that have happened on the branch you're currently on. This is a good strategy when you're working on something and you want to make sure that your branch is ahead of whatever is going on on the remote side.

```
git status         # it should show a diverge message

git pull --rebase  # first rewind local branch and then apply any commit in local branch; use 'git hist' to track the changes

git push           # push changes made locally
```

### Setting the Default Branch

It's a good practice to create a 'develop' branch since in some workflows the 'master' branch represents what is in production, and then 'develop' merges with master when code reaches production release status.

### Pull Conflict How to Solve Them

```
git pull       # this will return a merge conflict message
git mergetool  # execute pre-configured merge tool
```

After conflict has been manually resolved, then commit:

```
git commit -am "Conflict resolved"
```

If everything went well we'll be out of MERGING status.

```
git status  # it will show that there is one file laying around <file-name>.orig
rm *.orig   # delete *.orig files
```

### Combine Multiple Git Commits into One

```
git rebase -i HEAD~2  # instead of 2, place the number of commits to be combined; then type "squash" instead of "pick" in the commits to be squashed.
git push --force origin <branch-name>
```

### Making Special Events with Tagging

Tags are labels that can be put to commit points, if no commit point is specified by default it would be the current commit or HEAD.

```
git tag                                         # list tags if they exist
git tag --list                                  # display all tags
git tag <tag-name>                              # create a tag
git tag -d <tag-name>                           # delete a tag locally
git tag -a v0.1-alpha -m "Release 0.1 (Alpha)"  # -a for annotated tag, v1.0 is the tag name.
git show <tag-name>                             # display the annotated tag info
git push origin <tag-name>                      # push into remote repository a specific tag
git push --tags                                 # push all tags
```

### Updating Tags: Creating Floating Tags

```
git tag -f <tag-name-to-modify> <commit-id>  # update where a tag is pointing (if no <commit-id> is provided, the default is HEAD)
git pull
git push origin develop                      # this doesn't update the tag update
git push origin <tag-name>                   # it will show that tag already exists
git push --force origin <tag-name>           # force-push to update the tag
```

### Local Tags (a Bit of Review)

Simple tags:

```
git tag <tag-name-1> develop  # create a tag pointing to HEAD from develop branch
git tag <tag-name-2> master   # create a tag pointing to HEAD from master branch
```

Release tags:

```
git tag -a v0.1-alpha -m "Release 1.0 (Alpha)" 85fe102  # create an annotated tag
git show v0.1-alpha                                     # show info from specific tag (only works for annotated tags)
```

### Pushing Local Tags to GitHub

```
git pull                    # sync both repos
git push                    # by default tags or releases are not pushed
git push origin <tag-name>  # push one tag to remote repo
git push --tags             # push all local tags into remote repo
```

### Deleting Tags

After having deleted a tag in GitHub:

```
git status                   # see current state
git tag                      # list the current tags (a deleted tag will still appear)
git fetch -p                 # update all current references to be in sync
git tag -d <tag-name>        # delete a tag locally
git push origin :<tag-name>  # delete a tag in remote repo from local env
```
