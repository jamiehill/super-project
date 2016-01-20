Git Submodules
==============

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

1. Change the branch name in .gitmodules for all branches, to `some-branch`
------------------------------------------------------------------------

    git submodule foreach -q --recursive 'git config -f $toplevel/.gitmodules submodule.$path.branch some-branch'
    
This affectively tells the super project what branch needs to be tracked for each submodule.  This obviously can be master.  Will pull in the latest commit from said branch, to the project.
    
`-q` flag denotes that the operation on each submodule be silent.  Omitting this will print to standard out the module name before each operation.

`-recursive` denotes that submodules are traversed recursively (i.e. the given shell command is evaluated in nested submodules as well).
    
2. Create the required branch for all submodules (if it doesn't already exist)
------------------------------------------------------------------------------

    git submodule foreach --recursive 'git branch -f some-branch'
    
Iterates through each of the submodules, creating the required branch (if it doesn't exist)

3. Update all submodules to the latest commit of the new branch
---------------------------------------------------------------

    git submodule update --init --recursive --remote
    
Updates all of the submodules.  This happens according to the `[submodule]` config in `.gitmodules`.  

`--init` ensures the submodule is initialized in the super project.
`--recursive` ensures that all submodules - including nested modules - are updated.
`--remote` required as we're tracking a branch


4. Create super project branch and checkout
-------------------------------------------

    git checkout -B some-branch
    
Checks out the required branch for the super project.

`-B` creates the branch if it doesn't exist.

Is transactionally equivalent to doing:

    git branch -f some-branch
    git checkout some-branchgit -version
    
    
5. Recursively push the project
-------------------------------

    git push --recurse-submodules=on-demand
    
Pushes all changes - super project and all submodules - from the super project.
 
> Make sure all submodule commits used by the revisions to be pushed are available on a remote tracking branch.


Git Hooks
=========


