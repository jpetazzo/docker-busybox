# Jpetazzo's Busybox Image Builders

*A.k.a. "The Little Containers That Could!"*

This repository contains:

- a `rootfs.tar` archive suitable to build a minimal [Docker] image,
  based on [Busybox];
- a `Dockerfile` referencing this tarball, suitable for inclusion into
  [Stackbrew];
- multiple subdirectories providing `Dockerfiles` to build this tarball.

The goal of the latter is to solve a difficult question, *id est*
"who builds the builders?".

Those meta-builders are called "tarmakers". They obey the following "API":

- they are built with a Dockerfile themselves;
- the Dockerfile doesn't use any local content (no `ADD` of local file);
- they generate `rootfs.tar` in their root directory.

The script `mkrootfs.sh` is a helper to build `rootfs.tar`. It requires
as first and only argument the name of a subdirectory containing a tarmaker.

The following tarmakers are available:

- buildroot: uses the [Buildroot] toolchain to compile everything from
  scratch (**WARNING: THIS IS SLOW**, it takes half an hour on my quad-core
  laptop with badass SSDs, so it will probably take a few hours in a typical
  VM!)
- ubuntu: installs the `busybox-static` Ubuntu package and use it as the
  base for the newly built image. This is very fast but the resulting
  image is a bit bigger, because we have to slap a few fat greasy libraries
  on top of it. Would you like some fries with that?


## To rebuild a busybox image using one tarmaker or the other...

```
# Use buildroot or ubuntu
TARMAKER=ubuntu
./mkrootfs.sh tarmaker-ubuntu
docker build -t busybox .
docker run -t -i busybox
```


[Buildroot]: http://buildroot.uclibc.org/
[Busybox]: http://www.busybox.net/
[Docker]: http://docker.io/
[Stackbrew]: https://github.com/dotcloud/stackbrew