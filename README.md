# submodule_sandbox

Demonstration of git submodules.

For this demo we'll use the [`submodule_ex`](https://github.com/bsb808/submodule_ex) repo as a submodule of this repo.

Docs:

* https://git-scm.com/book/en/v2/Git-Tools-Submodules
* https://git-scm.com/docs/git-submodule

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

### init and update

To clone all the submodules in the repo:

```
git submodule update --init --recursive main
```

Or, if you just want to clone the submodules in the `main` path:
```
git submodule update --init --recursive main
```

This clones the submodule repo(s) as a detached HEAD at the tagged commit.  

Again, you can check the status of the submodules.  This shows that there are two submodules in the super project and that I've cloned the one in the `main` directory
```
bsb@hulihuli:~/tmp/submodule_sandbox$ git submodule status
-2dce25bc75ce7dae7ea566b2662da5cb3af2b30e dev/submodule_ex
 c0c039b626bd1d1099b4d5364338d8e503d9b0a2 main/submodule_ex (c0c039b)
```

### Chaging the tagged commit of a submodule

Continuing with the example, I've cloned the `main` branch of the `submodule_ex` repo at the original taged commit - `c0c039b`.   If I want to pull the latest commit from the branch, the HEAD, then...

```
cd main/submodule_ex
git pull origin main
```

Now I see
```
bsb@hulihuli:~/tmp/submodule_sandbox$ git submodule status
-2dce25bc75ce7dae7ea566b2662da5cb3af2b30e dev/submodule_ex
+1e87a0be8319cb3153bcb3a3ce89639e4a25fdbd main/submodule_ex (heads/main)
```
which shows that the submodule is at the new commit.

The evidence that the super project is updated can be seen with `git status` in the super project which shows:
```
bsb@hulihuli:~/tmp/submodule_sandbox$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   main/submodule_ex (new commits)
```

If I commit and push to the super repo and then clone a fresh copy, I'll see that the submodule is now tracking the latest commit:
```
bsb@hulihuli:~/tmp/submodule_sandbox$ git submodule status
-2dce25bc75ce7dae7ea566b2662da5cb3af2b30e dev/submodule_ex
-1e87a0be8319cb3153bcb3a3ce89639e4a25fdbd main/submodule_ex
```