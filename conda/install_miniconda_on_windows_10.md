# How to install miniconda on a machine running Windows 10

1. Download a [miniconda installer](https://docs.conda.io/en/latest/miniconda.html), copy the corresponding SHA256 hash, calculate the SHA256 hash of the downloaded file, and compare that value to the one copied from the download link page. If the values match exactly, proceed in installation.

```bash
curl -o Miniconda3-py39_4.12.0-Windows-x86_64.exe https://repo.anaconda.com/miniconda/Miniconda3-py39_4.12.0-Windows-x86_64.exe
$ sha256sum Miniconda3-py39_4.12.0-Windows-x86_64.exe
1acbc2e8277ddd54a5f724896c7edee112d068529588d944702966c867e7e9cc *Miniconda3-py39_4.12.0-Windows-x86_64.exe
1acbc2e8277ddd54a5f724896c7edee112d068529588d944702966c867e7e9cc
```

2. Execute the installer.
* Select "Just for me" (user install vs system install)
* accept location `C:\Users\<your_username>\miniconda3`
* uncheck the Set `PATH` variable box (or check it; I always manually add it to `PATH`), and
* check Set as default Python.
* Finalize (uncheck the "Show me help files and Anaconda cloud marketing materials" check boxes)

3. From the start menu, enter "Set user environment variables" and open up that settings tool.
* In the interface, select the `PATH` environment variable and click **Edit**
* Add 3 paths to `PATH`:
    * `C:\Users\<your_username>\miniconda3`
    * `C:\Users\<your_username>\miniconda3\Scripts`
    * `C:\Users\<your_username>\miniconda3\Library\bin`

* Make sure all 3 of those are at the top of the list of paths and then click "OK"

4. Open a new terminal and enter `conda init`. Close that terminal and open another new one, and you should be set.
