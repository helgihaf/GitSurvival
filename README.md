Git Survival Guide
==============
Most git commands should be executed at the base directory of a repo, I.e. where the .git folder is.

### Git global setup
    git config --global user.name "Arhtur Dent"
    git config --global user.email "arhur.dent@thgttg.somesillydomain.com"

### Create a new empty repository
    git clone https://gitlab.com/arthur.dent/TestRepo.git
    cd TestRepo
    touch README.md
    git add README.md
    git commit -m "add README"
    git push -u origin master

### Create repository from existing folder
    cd existing_folder
    git init
    git remote add origin https://gitlab.com/arthur.dent/TestRepo.git
    git add .
    git commit -m "Initial commit"
    git push -u origin master

### Push new local folder to existing empty repository
    cd existing_repo
    git remote add origin https://gitlab.com/arthur.dent/TestRepo.git
    git push -u origin --all
    git push -u origin --tags

### Check in all pending changes
    git add .
    git commit -m "some clever comment"

### Push local changes to remote
    git push origin master

### Pull remote changes to local
    git pull origin

### "Undo checkout" on a single file
    git checkout -- filename

### List local and remote branches
    git branch -a

### Create a branch and switch to it

    git checkout -b myBranch
    -- same as –
    git branch myBranch	# creates a branch
    git checkout myBranch	# switches to it

### Switch to a branch
Make sure you don’t have any uncommited changes, and then do:

    git checkout myBranch

### Merge branches
Switch to the target branch:

    git checkout master

Then do the merge:

    git merge myBranch

### Delete branch

    git branch -d myBranch

### Update fork to current state

    git clone https://github.com/arthur.dent/myfork.git 
    git remote add upstream git://github.com/sourceuser/sourceoffork.git 
    git fetch upstream git rebase upstream/master

### Save work for later without committing
    git stash

### Oh no! I changed to much, this should really be a new branch!
    git stash
    git stash branch myNewBranch

### Resolve Merge Conflicts
    git checkout --theirs myFile
      or
    git checkout --ours myFile
      or
    edit file in editor 
    
      then
    git add myFile
      or
    git rm myFile
    
### Abort Merge
    git merge --abort
