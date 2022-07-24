# Conda envs

Installing `miniconda` (preferred) or `anaconda` (not recommended) and then initializing `conda` will make the `conda` command available at the command line by amending your `.bashrc` to start any new terminal in the `base conda` env (which contains the `conda` package and its dependencies). If you want to be able to create reproducible environments, you shouldn't[^0] install packages into the `base` env and only install into other envs.

## Configuring `conda` for reproducibility

Each `conda` channel can has their own submission process and community of maintainers. As a result, different channels can have different recipes (eg listing different requirements) for a the same version of a package, which can make it difficult or impossible to solve dependencies, or can cause actually incompatible versions to be installed.

These settings can be avoided by configuring `conda` to strictly pull from the highest priority channel, and then set the highest priority channel (I prefer conda-forge, as it has the most robust set of packages and most active community of maintainers)

```bash
$ conda config --set channel_priority strict
$ conda config --add channels conda-forge
```

`conda config` commands update (or create if it doesn't already exist) a `.condarc` file in your home directory. You can review your non-default `conda` configurations either by simply printing out that file (`cat ~/.condarc`) or use `conda` command `conda config --show-sources`. You can see the full set of `conda` configurations via `conda config --show`.

## Creating a new env

You can use this command to create a new env named "my_new_env" with a specified version of python and installing two python packages (and their dependencies) at the start

```bash
$ conda create -n my_new_env python=3.9 geopandas jupyterlab
```

or if you just want to create an env with the default python version and no packages initially installed, run command

```bash
$ conda create -n my_new_env
```

### Installing packages into an existing env

First, you have to[^1] activate the env you want to install a package into

```bash
(base) user@host: ~/...$ conda activate my_new_env
(my_new_env) user@host: ~/...$
```

then install the package

```bash
conda install -c conda-forge geopandas
```

Note: Be careful to get the package name exactly right. To avoid victimization by [typosquatting](https://en.wikipedia.org/wiki/Typosquatting) attacks, I google the name of a package with the channel name, view the Anaconda package page for the package ([example](https://anaconda.org/conda-forge/geopandas)), check that there are a plausibly high number of downloads, copy the install command directly from that page, and paste that into the termial.


### [Note for Jupyter users] Registering your new env as a kernel

To make a `conda` env (that already has `ipykernel` (and possibly `jupyter_core`) installed) available as a kernel option in a Jupyter Notebook/Lab context, register the env via

```bash
python -m ipykernel install --user --name my_new_env --display-name "Python (my_new_env)"
```

## References

There are few pages I've visited more than `conda`'s [Managing environments](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) page.

## Footnotes

* [^0]: nearly never. The only exceptions I can think of are the [`conda-build`](https://docs.conda.io/projects/conda-build/en/latest/install-conda-build.html) and [`conda-smithy`](https://github.com/conda-forge/conda-smithy#installation) packages.

* [^1]: You can also install packages into a non-activated env by using the `--name` flag and specifying the name of the target env.