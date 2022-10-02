# Set up an EC2 instance for MLOps

1. <backfill instructions on EC2 instance config>
2. Connect to your instance
3. Install miniconda

```bash
curl -o Miniconda3-py39_4.12.0-Linux-x86_64.sh https://repo.anaconda.com/miniconda/Miniconda3-py39_4.12.0-Linux-x86_64.sh
78f39f9bae971ec1ae7969f0516017f2413f17796670f7040725dd83fcff5689
~$ sha256sum Miniconda3-py39_4.12.0-Linux-x86_64.sh 
78f39f9bae971ec1ae7969f0516017f2413f17796670f7040725dd83fcff5689  Miniconda3-py39_4.12.0-Linux-x86_64.sh
```

After confirming the SHA256 matches the published value, install Miniconda via

```bash
bash Miniconda3-py39_4.12.0-Linux-x86_64.sh
```
accept the terms, accept the default location, enter `conda init` and you're set.

4. Update Ubuntu's package list via ```sudo apt update```
5. Install docker via `sudo apt install docker.io`
6. Install docker-compose
    1. create a directory off of `~` for user collected utilities, say, `soft` and enter that directory

        ```bash
        ~$ mkdir soft && cd soft
        ```

    2. Go to the `docker-compose` github page then go to its [releases](https://github.com/docker/compose/releases) page. Scroll down to the **Assets** section and copy the link for the **linux-x86_64** version, as well as the corresponding SHA256

        ```bash
        wget https://github.com/docker/compose/releases/download/v2.11.2/docker-compose-linux-x86_64
        wget https://github.com/docker/compose/releases/download/v2.11.2/docker-compose-linux-x86_64.sha256
        ```

    3. Compare the checksums

        ```bash
        ~/soft$ sha256sum docker-compose-linux-x86_64
        1178848502b0771b96895febeb4b1736acd5019c4ed71a8efbabf6915185fe8a  docker-compose-linux-x86_64
        ~/soft$ cat docker-compose-linux-x86_64.sha256
        1178848502b0771b96895febeb4b1736acd5019c4ed71a8efbabf6915185fe8a *docker-compose-linux-x86_64
        ```

    4. Rename `docker-compose-linux-x86_64` to just `docker-compose`

        ```bash
        ~/soft$ mv docker-compose-linux-x86_64 docker-compose
        ```

    5. Make `docker-compose` executable

        ```bash
        ~/soft$ chmod +x docker-compose
        ```

    6. Add this directory to PATH
        * Open up `~.bashrc` via a text editor (like `nano`) and add this line to the bottom of the file

            ```bash
            nano ~/.bashrc

            ...
            export PATH="${HOME}/soft:${PATH}"
            ```

        Save and close (if using `nano`, ctrl+x to save, enter yes, then accept the file location), and then `source` that file to load in the new path

            ```bash
            $ echo $PATH
            /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
            $ source ~/.bashrc
            $ echo $PATH
            /home/ubuntu/soft:/home/ubuntu/miniconda3/bin:/home/ubuntu/miniconda3/condabin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
            ```
    7. Add our user to the docker group (so that you don't have to invoke `sudo` every time you want to run a docker command.

        ```bash
        $ sudo groupadd docker
        $ sudo usermod -aG docker $USER
        ```

        To trigger the bit of code that actually grants your USER the permissions granted by membership in that group, you have to log out and log back in. After doint that, you should be able to confirm the change by calling `$ docker run hello-world`