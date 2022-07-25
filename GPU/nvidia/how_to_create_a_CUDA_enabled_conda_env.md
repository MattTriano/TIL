# How to create a conda env with CUDA GPU support

If you already have
0. a Linux machine[^0] with a [CUDA capable GPU](https://developer.nvidia.com/cuda-gpus) (you can see your GPU model via `lshw -C display`), and
1. miniconda (preferred) or anaconda installed

you can use `conda` to get GPU-enabled deep learning functionality without working through the [messy official installation process](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).

## Recipe for a PyTorch env

I mostly use Jupyterlab as my development environment for data science tasks. If your workflow doesn't involve the jupyter notebook/lab interface, feel free to leave `jupyterlab` off of the `conda create` line and drop the last line (the one registering the env via the `ipykernel` python module)


```bash
$ conda create -n torch_env python=3.10 jupyterlab
$ conda activate torch_env
$ conda config --env --set channel_priority flexible
$ python -m ipykernel install --user --name torch_env --display-name "Python (torch_env)"
```

Then go to the "Install PyTorch" section of the [PyTorch home page](https://pytorch.org/) to get working compatible version numbers for your system. Select (click) the latest stable build, Linux, Conda, Python, and the latest CUDA version, and it will provide the `conda install` command you need to install `pytorch` dependencies from the correct `conda` channel.

```bash
$ conda install pytorch torchvision torchaudio cudatoolkit=11.6 -c pytorch -c conda-forge
```

Now you should be able to open use `torch_env` as the kernel for a notebook. To confirm CUDA GPU functionality is available in your env and ready for use, run the commands below.

```python
import sys
import torch

print(f"PyTorch Version: {torch.__version__}")
print(f"Python {sys.version}")
if torch.cuda.is_available():
    print("CUDA-enabled GPU is available and ready for use")
else:
    print("CUDA-enabled GPU NOT AVAILABLE :(")
```

If successful, you'll see a printout like

```python
PyTorch Version: 1.12.0
Python 3.10.5 | packaged by conda-forge | (main, Jun 14 2022, 07:04:59) [GCC 10.3.0]
CUDA-enabled GPU is available and ready for use
```


[^0] I'm sure it's possible to do GPU-enabled deep learning on Windows or Mac OS, but I switched to Ubuntu as my daily driver years ago and I haven't looked back.