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
