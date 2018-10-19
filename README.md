# busybox

[![GuardRails badge](https://badges.production.guardrails.io/moul/docker-armhf-busybox.svg)](https://www.guardrails.io)

Fork of the [progrium's busybox project](https://github.com/progrium/busybox).

Build a `Busybox` with `opkg`, for `armhf` targets from a `x86_64` build machine.

## Using and installing packages

This image is meant to be used as the base image for Busybox-based containers. It includes glibc, uclibc, and opkg with an easy-to-use wrapper for installing packages from your Dockerfiles:

	FROM moul/armhf-busybox
	RUN opkg-install curl bash git
	CMD ["/bin/bash"]

The above Dockerfile grabs the latest package index during build, installs curl, bash, git, all their dependencies, and then deletes the local package index. **The result is a Docker image that's only 10MB.** Not too shabby.

Compare that to a minimal Ubuntu 12.04 (which comes with curl, bash) after installing git: 300MB.

## Customizing buildroot configuration (Advanced)

If you want more control over how the rootfs is produced by customizing the buildroot config, there is some great tooling and an easy workflow. Delete the `rootfs.tar` file, then:

	$ cd rootfs
	$ make config

This will run an interactive menu inside a Docker container to configure buildroot. The resulting config will be placed at `rootfs/config`. Now go back up and rebuild:

	$ cd ..
	$ make build

This will cause buildroot to run again using [rootbuilder](https://github.com/progrium/rootbuilder) and your config. Building will likely take a while. The result will not only be a new `rootfs.tar`, but a new local Docker image tagged `busybox`.

## License

Original project by @progrium: BSD
This fork: BSD too
