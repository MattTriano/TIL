# How does Jupyter save the state of user work?

After restarting my machine, starting a jupyter server, and returning to a `jupter lab` tab after my browser has restored my prior session, the `jupyter lab` ide shows the files that were open before. Going to another `jupter lab` tab for another workflow, I find `jupyter` has reopened that workflow's set of files with the same layout. How does `jupyter` store this state information?

`jupyter` stores these **sessions** in a **workspace**, and `jupyter` stores each workspace's state information in its own file in a `/workspaces/` directory. 

The default location for the `/workspaces/` directory is `~/.jupyter/lab/workspaces`. If you want to use a different location, export that location to the `JUPYTERLAB_WORKSPACES_DIR` environment variable.

## How to export a workspace to be imported on another machine

The id of a workspace is contained in the URL. The general form of a `jupyter lab` url is

`[http(s)://]<server:port>/<lab-location>/lab/workspaces/<workspace-id>/tree/<path_to_notebook_or_file>`

Allegedly (per the [documentation](https://jupyterlab.readthedocs.io/en/stable/user/urls.html#managing-workspaces-cli)), you should be able to export that workspace via the command

```bash
jupyter lab workspaces export <workspace_id> > <workspace_filename>.json
```

but you can find the workspace file for your workspace via this grep search

```bash
$ grep -le '"metadata":{"id":"<workspace_id>"}' <your_workspaces_directory>*
```

which, for my machine (where I'm using the default `jupter` lab workspace directory and searching for a workspace named "auto-T"), is

```bash
$ grep -le '"metadata":{"id":"auto-T"}' ~/.jupyter/lab/workspaces/*
/home/matt/.jupyter/lab/workspaces/auto-t-9e5b.jupyterlab-workspace
```

and then I could (presumably, as I haven't tested this), import the workspace from that file on another machine by copying that file to the other machine and (from an env with `jupyter`) running the command

```bash
$ jupyter lab workspaces import auto-t-9e5b.jupyterlab-workspace
```