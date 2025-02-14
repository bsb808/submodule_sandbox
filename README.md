# submodule_sandbox

Demonstration of git submodules.

For this demo we'll use the [`submodule_ex`](https://github.com/bsb808/submodule_ex) repo as a submodule of this repo.

## Adding repos as submodules

Add the `main` branch of the [`submodule_ex`](https://github.com/bsb808/submodule_ex) repo to the local path `main/submodule_ex`.

```
git submodule add -b main git@github.com:bsb808/submodule_ex.git main/submodule_ex
```

This creates the `.gitmodules` file - and adds it to git.  It also adds the file `main/submodule_ex` and clones the HEAD of the `main` branch.

Also add the `dev` branch:
```
git submodule add -b dev git@github.com:bsb808/submodule_ex.git dev/submodule_ex
```

Now commit and push to the remote.


## Cloning repo with submodules

Cloning super project repo will include the `.gitmodules` file and empty directories for the submodules, but won't clone the submodules.

```
git clone git@github.com:bsb808/submodule_sandbox.git
```

We can check the status
```
 git submodule status
```
which will show the commit SHAs for each submodule
```
-2dce25bc75ce7dae7ea566b2662da5cb3af2b30e dev/submodule_ex
-c0c039b626bd1d1099b4d5364338d8e503d9b0a2 main/submodule_ex
```

We can look at the commit tied to a specific submodule
```
git ls-tree HEAD main/submodule_ex/
```
which shows...
```
160000 commit c0c039b626bd1d1099b4d5364338d8e503d9b0a2	main/submodule_ex
```

This commit is not necessarily the latest commit in the `main` branch of the `submodule_ex` repo.  It is the commit at the HEAD of the branch when we added it as a submodule.

The actual SHA for each submodule is stored as a tree option in the git object database - it isn't in the `.gitmodules` file.   You can poke around in the git database with the 
```
git cat-file -p HEAD^{tree}
```
which reads the tree object from `.git/objects` to show tracked files and submodules and their SHA hashes.