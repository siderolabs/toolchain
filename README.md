# toolchain

This repository produces a C toolchain with the following components:

- binutils
- linux headers
- libstdc++
- musl-libc
- gcc

The toolchain is configured to produce binaries with a search path set to `/toolchain/lib`.
This allows for a clean separation from the host distribution on which the toolchain will be used.

We suggest using this toolchain to create an intermediate rootfs with the tools required to build source code.
For example, `make` can be built using this toolchain, and it will be installed to `/toolchain`.
Once a complete set of binaries are built and installed to `/toolchain`, the original host is no longer required to build source code.
At that point you can set you $PATH to `/toolchain/bin` and build source code independent of the host distribution.

## Development

The development of this toolchain depends on Docker 19.03.0 or greater and [`bldr`](https://github.com/talos-systems/bldr).
The toolchain can be built for different architectures (e.g. x86_86, aarch64, etc.).
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
