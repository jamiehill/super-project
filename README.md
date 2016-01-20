Git Submodules
--------------

Add Submodule tracked to it's master branch

    git submodule add -b master https://github.com/jamiehill/submodule-b.git
    
Now .gitmodules has the usual reference to the submodule, but has also added the `branch` attribute:

    [submodule "submodule-b"]
    	path = submodule-b
    	url = https://github.com/jamiehill/submodule-b.git
    	branch = master
    	
Now we're tracking a remote branch, we need to add the `remote` flag when updating the submdoules:

    git submodule update --init --recursive --remote
    
This recursively updates all of the submodules as defined in `.gitmodules` to the latest commit in the specified branch.

If we want all submodules checked out to a branch, we have to update the `branch` flag in `.gitmodules` and also let the super project know to use the branched versions of the submodules

1. Change the branch name in .gitmodules for all branches, to `branch-a`

    git submodule foreach -q --recursive 'git config -f $toplevel/.gitmodules submodule.$path.branch branch-a'

2. Update all submodules to the latest commit of the new branch

    git submodule update --init --recursive --remote




1. Checkout all submodules to a particular branch.  Will create if not exists

    git submodule foreach -q --recursive 'git checkout -B branch-name'
    

2. Checkout all submodules to the branch:

    git submodule foreach -q --recursive 'branch="$(git config -f $topLevel/.gitmodules submodule.$name.branch)"; git checkout $branch'
    
    
    
git config --file=.gitmodules --get-regexp ^^submodule.*\.url$ | cut -d " "


Change .gitmodules branch for certain submodule

    git config -f .gitmodules submodule.$path.branch brancgith-a