# How to make a conda package of a package from PyPI and publish it to conda-forge

0. Create and activate a new `conda` env and install packages as needed while developing the code you want to package.

```bash
$ conda create --name gradio_env python=3.10 analytics-python aiohttp h11=0.12 pydantic requests httpx pandas pydub pillow python-multipart fsspec fastapi paramiko jinja2 uvicorn pycryptodome orjson markdown-it-py linkify-it-py mdit-py-plugins matplotlib grayskull
$ conda activate gradio_env
```

    * Note, while developing a package, you can install it into your `conda` env using `pip` with the `-e` editable flag so changes to the source code are available without reinstallation.

    ```bash
    (gradio_env) user@host:~/.../pkg_dir$ python -m pip install -e .
    ```

1. Publish the package to PyPI. (the `twine` package is helpful for that)

2. Go to the package's "Download files" page on `PyPI` and copy both the source distribution (the `.tar.gz` file) download link and corresponding sha256 hash.
    * This page should have a URL like `https://pypi.org/project/<package_name>/#files`, 
    * source distribution download url: 
        * https://files.pythonhosted.org/packages/a9/26/2bec77068bc6260130c6c783fe6b822dbd9738b9ab5f470d121319000cb7/gradio-3.1.1.tar.gz
    * sha256 hash:
        * cf98ee5983b80037fbc89c03552c1d73914cd2b1b31406a0f4e3c6e8e1089e43
    * [Optional step]: Download the file and confirm the hash matches

        ```bash
        $ curl https://files.pythonhosted.org/packages/a9/26/2bec77068bc6260130c6c783fe6b822dbd9738b9ab5f470d121319000cb7/gradio-3.1.1.tar.gz -o gradio-3.1.1.tar.gz
        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                       Dload  Upload   Total   Spent    Left  Speed
        100 5381k  100 5381k    0     0  19.6M      0 --:--:-- --:--:-- --:--:-- 19.6M
        $ sha256sum gradio-3.1.1.tar.gz
        cf98ee5983b80037fbc89c03552c1d73914cd2b1b31406a0f4e3c6e8e1089e43  gradio-3.1.1.tar.gz
        $ rm gradio-3.1.1.tar.gz
        ```

3. If you don't have a fork of [conda-forge/staged-recipes repo](https://github.com/conda-forge/staged-recipes), get one (if you do have a fork, fetch upstream to update your remote and fetch to update your local clone).
    1. (In your `staged-recipes/recipes/` of your local clone) use the `grayskull` package to make the recipe file for your pure python package

        ```bash
        $ ...staged-recipes/recipes$ python -m grayskull pypi --strict-conda-forge gradio
        ```

    * Running this command listed the required packages and flagged which were available on conda-forge (they were colored green), and the one that was not (shown colored red). The one package which is not available on `conda-forge` is `ffmpy`, a python wrapper for `FFmpeg` (an open-source collection of libraries and tools for processing audio and video content, and is used by nearly every service that produces or uses audio or video). It's very thin, so I'll try packaging that, too.

        * Made a recipe file using `grayskull`, entered the created directory, and copied in the LICENSE file

            ```bash
            ...staged-recipes/recipes$ python -m grayskull pypi --strict-conda-forge ffmpy && cd ffmpy
            ...staged-recipes/recipes/ffmpy$ curl -L -O https://raw.githubusercontent.com/Ch00k/ffmpy/master/LICENSE
            ```

        * Opened the `meta.yaml` file and
            * changed the `license_file: PLEASE_ADD_LICENSE_FILE` to `license_file: LICENSE`
            * added `- ffmpeg` as both a `host` and `run` requirement
        * Committed the changes to a new branch

            ```bash
            ...staged-recipes/recipes/ffmpy$ git checkout -b ffmpy_pkg
            ...staged-recipes/recipes/ffmpy$ git add LICENSE meta.yaml
            ...staged-recipes/recipes/ffmpy$ git commit -m "Adapting ffmpy package from PyPI for conda-forge."
            ```
        * Tested my recipe locally using [conda-forge's `build-locally.py` script](https://conda-forge.org/docs/maintainer/adding_pkgs.html#staging-test-locally)

            ```bash
            (env_w_python) ...staged-recipes/recipes/ffmpy$ cd ../..
            (env_w_python) ...staged-recipes/$ python build_locally.py
            ```

        * And after that succeeded, I pushed the new branch to upstream repo

            ```bash
            ...staged-recipes/$ git push --set-upstream origin ffmpy_pkg
            ```

        * Went through the checklist for my `ffmpy` pull request and submitted.
            * The linter requires a minimum python version, so I set that the min python version is 3.6 for host and run (although the number wasn't driven by a hard version req).
        * After things succeeded, pinged @conda-forge/staged-recipes in a comment on the PR. And now it's just a matter of waiting on that PR review for that library.

* After `ffmpy` is up on conda-forge, I'll put together a pull request for gradio and come back to this TIL.



# Resources:
* [Demonstration](https://www.youtube.com/watch?v=EWh-BtdYE7M) by a few conda-forge core devs