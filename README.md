Git Survival Guide
==============
Most git commands should be executed at the base directory of a repo, I.e. where the .git folder is.

# New Repositories
### Git global setup
```bash
git config --global user.name "Arhtur Dent"
git config --global user.email "arhur.dent@thgttg.somesillydomain.com"
```

### Create a new empty repository
```bash
git clone https://gitlab.com/arthur.dent/TestRepo.git
cd TestRepo
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

### Create repository from existing folder
```bash
cd existing_folder
git init
git remote add origin https://gitlab.com/arthur.dent/TestRepo.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

### Push new local folder to existing empty repository
```bash
cd existing_repo
git remote add origin https://gitlab.com/arthur.dent/TestRepo.git
git push -u origin --all
git push -u origin --tags
```
# Checking In
### Check in all pending changes
```bash
git add .
git commit -m "some clever comment"
```

### Push local changes to remote
```bash
git push -u origin master
```

### Pull remote changes to local
```bash
git pull origin
```

### Check in all pending changes
    git add .
    git commit -m "some clever comment"

### "Undo checkout" on a single file
```bash
git checkout -- filename
```

### Undo all local uncommitted changes
```bash
git checkout .
```

### Undo last commit
...and move back into staging.
```bash
git reset --soft HEAD^
```

### Undo all changes since last commit
...and delete new files
```bash
git reset HEAD --hard
git clean -fd
```

# Branches
### List local and remote branches
```bash
git branch -a
```

### Create a branch and switch to it
```bash
git checkout -b myBranch
-- same as –
git branch myBranch	# creates a branch
git checkout myBranch	# switches to it
```

### Switch to a branch
Make sure you don’t have any uncommited changes, and then do:
```bash
git checkout myBranch
```

### List remotes
```bash
git remote -v
```


### Rebase
Most useful for local branches that should be made up-to-date with their parent (master) branch. This will make changes to your target branch only.

First make sure that your master is up to date:
```bash
git checkout master
git pull origin
```
Then make sure that your target branch is ready for rebasing:
```bash
git checkout targetBranch
```

Now there are two options  
_Option 1:_  
```bash
<add/commit all changes>
```
_Option 2:_  
```bash
git stash
```

Then, on the target branch, perform the rebase:
```bash
git rebase master
```

If you opted for _Option2_
```bash
git stash pop
```
### Rebase: Syncing the idle feature
Scenario:
1. You create branch f1 from main and make some commits to it.
2. You make a pull request from f1 to main and wait for reviewers.
3. While waiting you create branch f2 _from branch f1_ and start developing the next feature.
4. Reviews arrive for the f1 pull request, you fix, commit and push. This goes on for a few rounds.
5. You continue to develop on f2 in the "idle" time between reviews and fixes.
6. The f1 pull request is accepted and you merge to main, optionaly squashing before merge.
7. To bring f2 up to date with main with surprising ease you do:
```bash
git checkout main
git pull
git checkout f2
git rebase --onto main <commit hash>
```
Where \<commit hash\> is the last hash that is common both to f1 and f2.
Now f2 is up to date with main (squash and all) and should only contain the commits you made to f2. You can now continue to develop on it, push, and create a pull request to main.

### Force push
This is commonly needed when you have pushed a branch to remote and then rebase it locally:
```
git push origin feature/add-aliens --force
```
**Warning**: Please don't do this if other people are working on the same branch. Others that have pulled this branch to their local machines will need to do some work to continue to use it, see https://stackoverflow.com/questions/9813816/git-pull-after-forced-update.

### Merge branches
Most useful for merging your working branch into the parent (master) branch. Switch to the target branch:
```bash
git checkout master
```

Then do the merge:
```bash
git merge myBranch
```

### Delete branch
```bash
git branch -d myBranch
```

### Rename branch
While in other branch:
```bash
git branch -m oldname newname
```
Rename the current branch:
```bash
git branch -m newname
```

### Update fork to current state
```bash
git clone https://github.com/arthur.dent/myfork.git 
git remote add upstream git://github.com/sourceuser/sourceoffork.git 
git fetch upstream 
git rebase upstream/master 
```

### Save work for later without committing
```bash
git stash
```

### Oh no! I changed to much, this should really be a new branch!
```bash
git stash
git stash branch myNewBranch
```

### Resolve Merge Conflicts
```bash
git checkout --theirs myFile
```
or
```bash
git checkout --ours myFile
```
or edit file in editor then
```bash
git add myFile
```
or
```bash
git rm myFile
```
    
### Abort Merge
```bash
git merge --abort
```

### Squash commits
Two ways of doing this  
Use 
```bash
git rebase -i <after-this-commit>
```
and replace "pick" on the second and subsequent commits with "squash" or "fixup", as described in the manual.

In this example, <after-this-commit> is either the SHA1 hash or the relative location from the HEAD of the current branch from which commits are analyzed for the rebase command. For example, if the user wishes to view 5 commits from the current HEAD in the past the command is 
```bash
git rebase -i HEAD~5.  
```
Credit [StackOverflow](https://stackoverflow.com/questions/5189560/squash-my-last-x-commits-together-using-git)  
Second way of doing this  
You can do this fairly easily without git rebase or git merge --squash. In this example, we'll squash the last 3 commits.

If you want to write the new commit message from scratch, this suffices:
```bash
git reset --soft HEAD~3 &&
git commit
```
If you want to start editing the new commit message with a concatenation of the existing commit messages (i.e. similar to what a pick/squash/squash/…/squash git rebase -i instruction list would start you with), then you need to extract those messages and pass them to git commit:
```bash
git reset --soft HEAD~3 && 
git commit --edit -m"$(git log --format=%B --reverse HEAD..HEAD@{1})"
```
Both of those methods squash the last three commits into a single new commit in the same way. The soft reset just re-points HEAD to the last commit that you do not want to squash. Neither the index nor the working tree are touched by the soft reset, leaving the index in the desired state for your new commit (i.e. it already has all the changes from the commits that you are about to “throw away”).
Credit [StackOverflow](https://stackoverflow.com/questions/5189560/squash-my-last-x-commits-together-using-git) 

### Prune remote branches
This command deletes remote tracking branches that don't exist anymore on the remote.
To see which branches would be deleted:
```bash
git remote prune --dry-run origin
```
To actually prune the branches:
```bash
git remote prune origin
```

# Tagging
### List Tags
```bash
git tag
```

### Create Tag
```bash
git tag -a v1.4 -m "my version 1.4"     # Annotated tag
git tag v1.4-lw                         # Lightweight tag
```

### Checkout Tag
```bash
git checkout v1.4       # For viewing
git checkout -b v1.4.1  # ...branch v1.4 tagged source into new branch v1.4.1
```
Or, all in one command:
```bash
git checkout -b v1.4.1 v1.4     # check out source with tag v1.4 and create new branch v1.4.1
```

# History
### Show history of a file
```bash
git log -- <filename>
```
### Get old version of a file
```bash
git cat-file -p <sha1>:./file.tex > wherever.tex
```
