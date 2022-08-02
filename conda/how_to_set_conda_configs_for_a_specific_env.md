# How to set conda configs for a specific env

In general, you can make any `conda config` command specific to the active env by adding the flag `--env` to a `conda config` command. Then, the config will be recorded in the env's `.condarc` file (which is in the root directory of that env, which is typically in `~/.conda/envs/<env_name>/.condarc`), which has higher priority than the user's global `.condarc` file (typically in location `~/.condarc`).

## Relaxing channel_priority strictness

In general I recommend globally using the `conda configs` set by

```bash
$ conda config --add channels conda-forge
$ conda config --set channel_priority strict
```

as strictly pulling from conda-forge will
* 1) make your environments more reliable,
* 2) greatly reduce the time it takes to solve for dependencies, and
* 3) will ensure your env is reproducible (at least on other machines with the same OS)

but if you need a package that isn't on conda-forge, you can set configs for a specific env without interfering with your global configs.

```bash
(special_env) user@host: ~/...$ conda config --env --set channel_priority flexible
```

Now you can install packages from any channel into this env.

## Other configs

You can see the other available config options [here](https://docs.conda.io/projects/conda/en/latest/commands/config.html), or by entering `conda config --help` at a command line.