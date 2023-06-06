# Bad BPF
A collection of malicious eBPF programs that make use of eBPF's ability to
read and write user data in between the usermode program and the kernel.

- [Overview](#Overview)
- [To Build](#Build)
- [To Run](#Run)
- [Availible Programs](#Programs)


# Overview
See my [blog](https://blog.tofile.dev/2021/08/01/bad-bpf.html) and my [DEF CON talk](https://defcon.org/html/defcon-29/dc-29-speakers.html#path) for an overview on how thee programs work and why this is interesting.

Examples have been tested on:
- Ubuntu 20.10
- Fedora 34

# Build
To use pre-build binaries, grab them from the [Releases](https://github.com/pathtofile/bad-bpf/releases) page.

To build from source, do the following:

## Dependecies
To build and run all the examples, you will need a Linux kernel version of at least [4.7](https://github.com/iovisor/bcc/blob/master/docs/kernel-versions.md).

As this code makes use of CO-RE, it requires a recent version of Linux that has BTF Type information.
See [these notes in the libbpf README](https://github.com/libbpf/libbpf/tree/master#bpf-co-re-compile-once--run-everywhere)
for more information. For example Ubuntu requries `Ubuntu 20.10`+.

To build it requires these dependecies:
- zlib
- libelf
- libbfd
- clang **11**
- make

On Ubuntu these can be installed by
```bash
sudo apt install build-essential clang-11 libelf-dev zlib1g-dev libbfd-dev libcap-dev linux-tools-common linux-tools-generic
```

**NOTE:** Some examples fail to build on Clang 12. To install specifically clang 11 on Fedora 34+ you have to run:
```bash
# First install clang 12
sudo dnf install clang
# Then downgrade to Clag 11, which was in Fedora 33
sudo dnf downgrade --releasever=33 clang
```

## Build
To Build from source, recusivly clone the respository the run `make` in the `src` directory to build:
```bash
# --recursive is needed to also get the libbpf source
git clone --recursive https://github.com/pathtofile/bad-bpf.git
cd bad-bpf/src
make
```
The binaries will built into `bad-bpf/src/bin`. If you encounter issues with related to `vmlinux.h`,
try remaking the file for your specific kernel and distribution:
```bash
cd bad-bpf/tools
./bpftool btf dump file /sys/kernel/btf/vmlinux format c > ../src/vmlinux.h
```

# Run
To run, launch each program as `root`. Every program has a `--help` option
that has required arguments and examples.

