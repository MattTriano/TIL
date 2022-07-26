# Risks in extracting archive files

Archive files are used to bundle one or more files together into a files into portable filesystem that can be moved around as though it was a single file. Common archive file extensions are `.zip`, `.tar`, `.rar`, or `.7z`. To economize on data transmissions and to make it easier to confirm transmission errors, they're often compressed via a compression algorithm like `gzip` or `bzip2` (although some archive formats, like `.zip`, `.7z`, and `.rar`, archiving and compression).

File systems use paths to specify the locations of files, and many file systems include special symbols to enable relative paths (eg `..` to go up a directory level), or aliased paths (eg `~` for the home directory, or `/` for the root dir), but a malicious or inexperienced user could define the path to a file within an archive file to be `~/.bashrc` or something in `/bin/` some other commonly used path on regular file systems so that extracting the archive would cause that file to overwrite the real file in that location, which could compromise a network or make a system unusable.


## How to check filepaths before extracting an archive file using python

`conda` packages are distributed as `.tar.bz2` compressed archive files. To safely view the filesystem contents within a python context, the snippet below will inspect and display the files of the archive file without extracting them. If there are files starting with `~/` or that would overwrite important system or user files, don't extract!

```python
import pathlib
import tarfile

with tarfile.open(pathlib.Path("fastbook-0.0.26-py_0.tar.bz2"), "r:bz2") as tf:
    tf.list(verbose=True)
```

## How to check filepaths before extracting an archive file using Linux

Most Linux systems have the `vi` or `vim` text editor by default. Checking the archive file via `vi` or `vim` will display the filepaths in the archive file. Note: to close `vi` or `vim`, press Esc + :q (colon then q), or if you edited text and don't want to save those changes, press Esc + Shift ZQ.