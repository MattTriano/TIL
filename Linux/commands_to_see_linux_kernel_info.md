# Linux Kernel

A **kernel** is a a layer of operating system code that facilitates and controls [interactions](https://en.wikipedia.org/wiki/Linux_kernel#/media/File:Linux_kernel_map.png) between computing hardware (eg CPU, memory, storage, I/O devices, etc) and software. The Linux kernel is a free, open-source, feature-rich kernel that has been deployed in a wide range of computing systems and is likely the most frequently used kernel on Earth.

It is written in C and comes with the `gcc` C compiler.

## Kernel info inspection method 1: `uname`

The `uname` command prints out system information. Including the `-a` (*all*) flag shows the kernel name, node host name (redacted below), kernel release, kernel version, machine hardware name, processor type, hardware platform, and OS.

```bash
$ uname -a
Linux host_name 5.15.0-41-generic #44~20.04.1-Ubuntu SMP Fri Jun 24 13:27:29 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

## Kernel info inspection method 2: `/proc` inspection

The `/proc` filesystem is a pseudo-filesystem which provides an interface to kernel data structures. You can view the kernel version via

```bash
$ cat /proc/version
Linux version 5.15.0-41-generic (buildd@lcy02-amd64-105) (gcc (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0, GNU ld (GNU Binutils for Ubuntu) 2.34) #44~20.04.1-Ubuntu SMP Fri Jun 24 13:27:29 UTC 2022
```