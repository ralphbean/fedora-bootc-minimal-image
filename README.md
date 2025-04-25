# Fedora bootc base images

Create and maintain base *bootable* container images from Fedora packages.

## Motivation

The original Docker container model of using "layers" to model applications has
been extremely successful. This project aims to apply the same technique for
bootable host systems - using standard OCI/Docker containers as a transport and
delivery format for base operating system updates.

## Building images

The current default user experience is to build *layered* images on top of the official
binary base images produced and tested by this project. See the documentation[5] for more info.

You can build custom base images by forking this repository; however,
<https://gitlab.com/fedora/bootc/tracker/-/issues/32> tracks a more supportable
mechanism that is not simply forking. For more information see[6].

## Build process

Building the images in this repo can be done with `podman build`, but
note the build process uses a special podman-ecosystem specific mechanism
to create fully custom images while inside a `Containerfile`.
You need to enable some privileges as nested containerization is required.

```bash
podman build --security-opt=label=disable --cap-add=all \
  --device /dev/fuse -t localhost/fedora-bootc .
```

See the `Containerfile` for more details. This builds the `minimal` image.

## Fedora versions

By default, the base images are built for Fedora rawhide. To build against a
different Fedora version, you can override the `FROM` image used to obtain the
Fedora repos and dnf variables. E.g.:

```bash
podman build --from quay.io/fedora/fedora:41 ...
```

### Deriving

You are of course also free to fork, customize, and build base images yourself.
See this page[6] of the documentation for more information.

## More information

Documentation: <https://docs.fedoraproject.org/en-US/bootc/>

## Badges

| Badge                   | Description          | Service      |
| ----------------------- | -------------------- | ------------ |
| [![Renovate][1]][2]     | Dependencies         | Renovate     |
| [![Pre-commit][3]][4]   | Static quality gates | pre-commit   |

[1]: https://img.shields.io/badge/renovate-enabled-brightgreen?logo=renovate
[2]: https://renovatebot.com
[3]: https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit
[4]: https://pre-commit.com/
[5]: https://docs.fedoraproject.org/en-US/bootc/building-containers/
[6]: https://docs.fedoraproject.org/en-US/bootc/building-custom-base/
