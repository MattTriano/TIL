# Setting up a local installation of Go dev tools (Linux)

Go compiles code into binaries that can run on any system, but if you want to compile Go code, you'll need Go's dev tools.

## Downloading a Go binary

Start by selecting the Go binary download link for your system and desired stable version from Go's [downloads page](https://go.dev/dl/) and downloading that archived binary to the target system.

If you want to download it from the command line, copy the correct download link and `curl` it as below (the `-L` flag is necessary as the listed link redirects to a google download server).

```bash
$ cd ~/Downloads/
$ curl -L https://go.dev/dl/go1.18.4.linux-amd64.tar.gz -o go1.18.4.linux-amd64.tar.gz
```

### Validating your downloaded file

Calculate the SHA256 hash of the downloaded file via the command below (if you're on windows, I think powershell command `certutil -hashfile path/to/downloaded/file sha256` works)

```bash
$ sha256sum ~/Downloads/go1.18.4.linux-amd64.tar.gz
c9b099b68d93f5c5c8a8844a89f8db07eaa58270e3a1e01804f17f4cf8df02f5  go1.18.4.linux-amd64.tar.gz
```

and then compare it to the SHA256 checksum listed on the above download page for the downloaded binary file.

```bash
c9b099b68d93f5c5c8a8844a89f8db07eaa58270e3a1e01804f17f4cf8df02f5
```

If the SHA256 hash you calculated is an exact match for the SHA256 published on the download page, proceed (I find it's easiest to copy the calculation output and published hash to a text file as shown and visually inspecting).

```text
c9b099b68d93f5c5c8a8844a89f8db07eaa58270e3a1e01804f17f4cf8df02f5  go1.18.4.linux-amd64.tar.gz
c9b099b68d93f5c5c8a8844a89f8db07eaa58270e3a1e01804f17f4cf8df02f5
```

If they differ at all, do not proceed.

## Installing the binary

If there's already a Go installation on your system, you'll want to remove that first before unpacking and installing the newly downloaded binary. You can ignore this step if Go hasn't been installed on your system, but if Go isn't installed, there won't be a `/usr/local/go/` directory (so running this command would do nothing in that case).

```bash
$ sudo rm -rf /usr/local/go/
```

Then, unpack your downloaded binary gzipped tarball to `/usr/local/` via

```bash
$ sudo tar -C /usr/local/ -xzf ~/Downloads/go1.18.4.linux-amd64.tar.gz
```

and then enable the OS to find the `go` binary by adding that path to your `$PATH`.

You can do that temporarily via

```bash
$ export PATH=$PATH:/usr/local/go/bin
```

Or you can do that more persistantly by adding the command to your `~/.profile` file and then executing that file (technically re-executing, as it's run any time the system starts up), via

```bash
$ echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.profile
$ source ~/.profile
```

## Confirming your installation.

Enter `go version` and it will print out the installed version of Go if everything is set up correctly.

```bash
$ go version
go version go1.18.4 linux/amd64
```
