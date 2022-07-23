# Basic Poetry Workflow (in conda env context)

Poetry is a tool for dependency management and python package development.

## Setting up the context

```bash
conda create -n env_env python=3.10
conda activate env_env
conda install -c conda-forge poetry
```

## Creating a new poetry project

Including the `--src` flag will create a `src/` directory in your new project directory

```bash
python -m poetry new --src project_name
```

## Installing a package from the PyPI

To add a package as a project requirement, navigate into your project directory and run command

```bash
poetry add requests
```

If a package is just a development dependency, add the `-D` flag

```bash
poetry add black -D
```