# How to install `youtube-dl` on a Linux system
how_to_install_youtube_dl_on_a_Linux_system


1. Go to [youtube-dl download age](https://ytdl-org.github.io/youtube-dl/download.html) and copy the download link to the shown version as well as the SHA256 hash
    * At time of writing, these are:
        * download url: https://yt-dl.org/downloads/2021.12.17/youtube-dl
        * sha256 hash: 7880e01abe282c7fd596f429c35189851180d6177302bb215be1cdec78d6d06d
2. Download the `youtube-dl` binary

    ```bash
    ~/Downloads$ curl -L -O https://yt-dl.org/downloads/2021.12.17/youtube-dl
    ```

3. Calculate the hash of the download and confirm it matches the expected hash exactly

    ```bash
    ~/Downloads$ sha256sum youtube-dl
    7880e01abe282c7fd596f429c35189851180d6177302bb215be1cdec78d6d06d  youtube-dl
    ```

4. If and only if the hash matches exactly, move the binary into the location `/usr/local/bin/youtube-dl` and `chmod` it to be readable and executable by system users

    ```bash
    $ sudo mv ~/Downloads/youtube-dl /usr/local/bin/youtube-dl
    $ sudo chmod a+rx /usr/local/bin/youtube-dl
    ```

Now `youtube-dl` should be available for you as a command at the command line.