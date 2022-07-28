# Renaming "master" branch to "main"

For a number of reasons, "main" is just a better name for the main branch than the prior default, "master".

1. (In your local repo): Rename the "master" branch to "main" and push that to your remote

    ```bash
    $ git branch -m master main
    $ git push -u origin main
    ```

    *The `-m` flag stands for "move" (like the `mv` command), and it just renames the branch and corresponding reflog*

2. On GitHub (or your remote service, if it has a protected branches), switch the default branch
    * Go to the **Settings** page for the remote repo, then go to the **Branches** group of settings, then switch the default branch from "master" to "main", and click **Update** and the following confirmation.

3. (In your local repo): Delete the "master" branch from both your local and the remote

    ```bash
    $ git push origin --delete master
    ```

If you're the only person who has a local repo of this remote repo, that's all you have to do. If you have teammates who are also developing the repo, you'll have to notify them that they'll have to do a bit of cleanup in their local clone of the repo.

```bash
$ git checkout master
$ git branch -m master main
$ git fetch
$ git branch --unset-upstream
$ git branch -u origin/main
```
