Git Submodules
--------------

Add Submodule tracked to it's master branch

    git submodule add -b master https://github.com/jamiehill/submodule-b.git
    
Now .gitmodules has the usual reference to the submodule, but has also added the `branch` attribute:

    [submodule "submodule-b"]
    	path = submodule-b
    	url = https://github.com/jamiehill/submodule-b.git
    	branch = master
    	
