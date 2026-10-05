# Official Docker Images

## Contents

  * 1 Introduction
    * 1.1 Docker
    * 1.2 Alpine Linux
    * 1.3 Debian
    * 1.4 Ubuntu
  * 2 Design considerations
  * 3 TODO (help needed!)
  * 4 GitLab CI
  * 5 Further Reading
  * 6 See Also



## Introduction

In the last years the use of containerization, often with Docker - the world's leading software container platform [[1]](<https://www.geeksforgeeks.org/containerization-using-docker/>), has grown rapidly due to the high usage of [Continuous Integration / Continuous Delivery](<https://www.redhat.com/en/topics/devops/what-is-ci-cd>) (short: CI/CD) in the development process. There are multiple repositories on DockerHub that pretend to provide Docker files and images for FPC but none of them is 'official' and it's also unsure if they are still maintained. Often they also only provide images for a single architecture. 

To ease the local development within containers and the use of CI/CD with FPC - also on different architectures - the FPC team provides official Docker containers on [DockerHub](<https://hub.docker.com/r/freepascal/fpc>) and [GitLab](<https://gitlab.com/freepascal.org/fpc/docker>). 

  


### Docker

A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings. Container images become containers at runtime. Containers isolate software from its environment and ensure that it works uniformly despite differences for instance between development and staging. 

Docker containers run on Windows, Linux, macOS and cloud service providers like Amazon Web Services and Microsoft Azure. 

Applications that traditionally run as Unix daemons or Windows services, such as web servers and databases, increasingly run containerized. 

For more information and the difference between virtual machines and containers, see [What is a Container? | App Containerization | Docker](<https://www.docker.com/resources/what-container>)

### Alpine Linux

[Alpine Linux](<https://alpinelinux.org/>) is a security-oriented, lightweight Linux distribution based on musl libc and BusyBox. The image is only 5 MB in size and has access to a [package repository](<https://pkgs.alpinelinux.org/packages>) that is much more complete than other BusyBox based images. [[2]](<https://hub.docker.com/_/alpine>)

### Debian

[Debian](<https://www.debian.org/>) is a Linux operating system and one of the most popular Linux distributions for personal computers and network servers. It has been used as a base for several other Linux distributions. [[3]](<https://hub.docker.com/_/debian>)

### Ubuntu

[Ubuntu](<https://ubuntu.com/>) is a Debian-based Linux operating system that runs from the desktop to the cloud. It is the world's most popular operating system across public clouds and OpenStack clouds. It is the number one platform for containers; from Docker to Kubernetes to LXD, Ubuntu can run your containers at scale. Fast, secure and simple, Ubuntu powers millions of PCs worldwide. [[4]](<https://hub.docker.com/_/ubuntu>)

## Design considerations

  * The images can be pulled with the specific, stable tags, e.g. `3.2.2-focal-full` or `3.2.2-slim`, but also with the latest tag which will always point to the latest official FPC version. At the time of writing this article `latest-focal-full` points to `3.2.2-focal-full`, `latest-slim` to `3.2.2-slim`, etc.


  * Dockerfiles should be stored inside the GitLab FPC project and images be build automatically through the GitLab CI.


  * The Debian or Ubuntu version covers exactly what a user would install on their PC so using this Docker image should give the same user experience as a normal installation except that it's inside a container. This easily allows using different FPC versions without local installation conflicts.


  * FPC has support for many different architectures and platforms and thus also the Docker images should support as many as possible. Therefore we're using [Docker Buildx](<https://docs.docker.com/buildx/working-with-buildx/>) to provide multi-architecture Docker images. If you do `docker pull freepascal/fpc:latest-full` the image appropriate to your hardware architecture will be fetched by Docker. You can also force a platform via the [`--platform`](<https://docs.docker.com/engine/reference/commandline/pull/>) flag of `docker pull`. A list of available platform images can be seen on [DockerHub](<https://hub.docker.com/r/freepascal/fpc/tags>). Unfortunately the Gitlab Docker registry does [not show multi-architecture images in the browser yet](<https://gitlab.com/gitlab-org/gitlab/-/issues/232815>) but `docker manifest inspect registry.gitlab.com/freepascal.org/fpc/docker/fpc:latest-full` lists the same architectures as `docker manifest inspect freepascal/fpc:latest-full`.


  * The Dockerfiles makes extensive use of [multi-stage builds](<https://docs.docker.com/develop/develop-images/multistage-build/>) to reduce the size of each layer and to have everything within a single Dockerfile. Therefore the desired build stage of your own local container buil from the provided Dockerfile needs to be specified by the `--target` option. Otherwise the last stage inside the Dockerfile will be built.


  * For avoidance of splitting the Dockerfile content over multiple files the download script is generated as a separate stage and called later. Thus users need to use [Docker Buildx](<https://docs.docker.com/buildx/working-with-buildx/>) and run `docker buildx build` as we build multi-platform images.


  * The Alpine Linux images are provided because these images are always lightweight and it comes in different variants to save some additional space and bandwidth in case one knows that only a few specific things are needed to built its tool. This might also reflect the security-oriented approach of Alpine Linux so that nothing is shipped that isn't really used. 
    * The full version provides every unit as well as binary but it comes without examples and documentation.
    * The slim version removes most of the packages and keeps only the core packages, known as RTL, and all binaries.
    * The minimal image includes all binaries and the part of the RTL that is needed to fully compile and test a newly built compiler.


  * Alpine Linux uses [musl libc](<https://musl.libc.org/>) as the C standard library as musl is lightweight, fast, simple, free, and strives to be correct in the sense of standards-conformance and safety. Therefore the image comes with the musl C library implementation (libc, libpthread, etc) and a lightweight glibc compatibility layer (`libc6-compat` package) to provide symlinks for libc, libpthread, etc. Additionally the GNU make utility `make` is needed to run the commands for building a new compiler, e.g. `make fullcycle` or `make all`.


  * To suppress several warnings of FPC, the Alpine Linux also provides the gcc binary and libgcc library that is fetched in the make script via gcc: `gcc -print-libgcc-file-name`.


  * SourceForge is unfortunately often quite slow or even timeouts during download (depends on the mirror they allocate you) but fortunately there is the [Free Mirror Project](<http://freemirror.org/>) which also provides the FPC tar files and is super-fast.


  * To allow updates or improvements to be applied for a specific version, each major version gets its own branch. But this has the negative side-effect that the `latest` might not point to the latest FPC release anymore. Thus you've to make sure that if something is merged into an older version, the pipeline for the newest version needs to be run again. **Thus it's probably the simplest to avoid adding stuff to old FPC branches and to only maintain the recent major FPC version - the version the`latest` tag points to.**


  * A newer version branch (e.g. 3.2.8) might base on a previous version branch (e.g. 3.2.6) but it doesn't be forked off from the nightly branch as those are fully independent.


  * To have the commit SHA in the version output of the trunk compiler, only `git clone --branch main` is a feasible solution as options like `git clone --branch main --single-branch --depth=1` won't provide the required git describe information.


  * Branches must be marked as `protected` (See GitLab project: Settings -> Repository -> Protected branches) to get access to the required CI variables (login and password).



## TODO (help needed!)

  * Provide separate Dockerfiles for various combinations of cross-compilers inside the version branch (suggested naming convention: `<version>-<start_architecture>-to-<destination_architecture>`), e.g.: 
    * 3.2.x-x86_64-to-win64 - image to compile from Linux x86_64 to Windows 64bit
    * 3.2.x-armv7-to-win64 - image to compile from Linux ARMv7 to Windows 64bit
    * 3.2.x-x86_64-to-aarch64 - image to compile from Linux x86_64 to Linux ARMv8 (AArch64)
    * ...


  * Create images that are based on the FPC ones but include Lazarus (maybe with (=Debian and/or Ubuntu) and without GUI part for CI (=Alpine)).


  * Specific Dockerfile for pas2js that can build projects but also images for secure deployment in the cloud.


  * Review (and fix?) the few other linting hints from [Haskell Dockerfile Linter](<https://github.com/hadolint/hadolint>).


  * Verify if the QEMU install steps in the GitLab CI make a difference or if it's enough to run `docker buildx create --use` as described by [Docker Cross-Platform Build](<https://leimao.github.io/blog/Docker-Cross-Platform-Build/>).


  * [Sign the provided images](<https://docs.docker.com/engine/security/trust/>) with the Docker Content Trust (DCT).


  * [Scan the images for vulnerabilities](<https://docs.docker.com/get-started/09_image_best/>) after pushing.


  * Consider [adding and use a non-root user](<https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#user>) as default.


  * Adjust download script to verify file against a provided sha256 checksum. _Therefore the checksum must be provided for all official files by FPC first_ .


  * The GitLab CI scripts uses only a single stage as it's not possible yet to export the created multi-architecture image and the only way was to directly push it and then run the same command again to mirror it to another container registry. This also means it's not possible to test the newly created image before pushing it...



## GitLab CI

The `.gitlab-ci.yml` file checks if a Dockerfile exists to prevent jobs from running if no Dockerfile is provided for this distribution. The script uses the branch name to distinguish between different build flavors and create tags based on that. Therefore it is assumed that the default branch (=nightly) is used for nightly builds and versioned branch names are for dedicated FPC versions (regex for semantic versioning). 

For Debian versions bumps (e.g. buster to bullseye) the `DEBIAN_VERSION` inside the `.gitlab-ci.yml` file needs to be changed and not only the version argument `ARG DEBIAN_VERSION=buster` of the Dockerfile, otherwise the pull tags will be broken. 

For Ubuntu versions bumps (e.g. focal to jammy) the `UBUNTU_VERSION` inside the `.gitlab-ci.yml` file needs to be changed and not only the version argument `ARG UBUNTU_VERSION=focal` of the Dockerfile, otherwise the pull tags will be broken. 

## Further Reading

  * [Arm Developer Documentation - Getting started with Docker](<https://developer.arm.com/documentation/102475/0100/Multi-architecture-images>)
  * [Docker Cross-Platform Build](<https://leimao.github.io/blog/Docker-Cross-Platform-Build/>)



  


## See Also

  * [Qemu and other emulators](<Qemu_and_other_emulators.md> "Qemu and other emulators")
  * [Small Virtual Machines](<Small_Virtual_Machines.md> "Small Virtual Machines")
  * [Docker Containerization](<Docker_Containerization.md> "Docker Containerization")

---

_Source: [https://wiki.freepascal.org/Official_Docker_Images](https://web.archive.org/web/20250219044003/https://wiki.freepascal.org/Official_Docker_Images)_
