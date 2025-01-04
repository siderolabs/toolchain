# toolchain

![Dependency Diagram](/deps.png)

This repository produces a fully bootstrapped (using [\[Stageˣ\]](https://codeberg.org/stagex/stagex)) base toolchain with the following components:

- busybox shell
- binutils
- GCC
- Linux headers
- musl libc
- GNU Make
- Go

Currently, [a fork of \[Stageˣ\]](https://github.com/siderolabs/stagex) is used to provide multiplatform support.

This toolchain should be capable of building all parts of Talos Linux without relying on tools other than image build tools.

## Development

The development of this toolchain depends on Docker 19.03.0 or greater and [`bldr`](https://github.com/siderolabs/bldr).
The toolchain can be built for different architectures (e.g. `x86_64`, `aarch64`, etc.).
To do this you will need to setup the proper `docker context` for the architecture you intend on building for.
For example, to setup the build for `aarch64` (`arm64`):

```bash
docker context create arm64 \
    --description "An arm64 builder" \
    --docker "host=tcp://$IP:2376,ca=${HOME}/.docker/client/ca.pem,cert=${HOME}/.docker/client/cert.pem,key=${HOME}/.docker/client/key.pem"
docker buildx create \
    --use \
    --name arm64-builder \
    arm64
```

> Note: You can run the above steps against an `x86_64` machine, substituting `arm64` with `amd64`.

Then, to build:

```bash
make USERNAME=${DOCKER_HUB_USERNAME} PLATFORM=linux/arm64 PUSH=true
```

> Note: You can use `PLATFORM=linux/amd64,linux/arm64` if you have configured `docker` with and `amd64` and `arm64` `context`s.

## Resources

- https://docs.docker.com/engine/security/https/#create-a-ca-server-and-client-keys-with-openssl
- https://gcc.gnu.org/onlinedocs/gccint/Configure-Terms.html
- https://wiki.osdev.org/Target_Triplet
- https://stagex.tools
- https://codeberg.org/stagex/stagex
