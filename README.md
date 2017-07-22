Git Survival Guide
==============
Most git commands should be executed at the base directory of a repo, I.e. where the .git folder is.

### Git global setup
    git config --global user.name "Helgi Hafþórsson"
    git config --global user.email "helgihaf@gmail.com"

### Create a new repository
    git clone git@gitlab.com:helgihaf/Prufa.git
    cd Prufa
    touch README.md
    git add README.md
    git commit -m "add README"
    git push -u origin master

### Existing folder
    cd existing_folder
    git init
    git remote add origin git@gitlab.com:helgihaf/Prufa.git
    git add .
    git commit -m "Initial commit"
    git push -u origin master

### Existing Git repository
    cd existing_repo
    git remote add origin git@gitlab.com:helgihaf/Prufa.git
    git push -u origin --all
    git push -u origin --tags

### "Undo checkout" on a single file
    git checkout -- filename

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
