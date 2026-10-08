# Docker Core Concepts

## 1. Docker, containerd and runc

Docker is a container platform that uses multiple components underneath.

- **Docker CLI**: user-facing command-line interface.
- **dockerd**: Docker daemon; handles Docker-specific operations.
- **containerd**: container runtime manager used by Docker.
- **runc**: low-level OCI runtime that creates and starts containers using Linux kernel features.
- **Linux kernel**: provides namespaces, cgroups and other isolation/resource-control mechanisms.

A simplified flow:

```text
docker run
    |
    v
Docker CLI
    |
    v
dockerd
    |
    v
containerd
    |
    v
runc
    |
    v
Linux kernel
```

## 2. LXC / LXD / LXCFS vs Docker

### LXC

LXC is a Linux container technology that provides OS-level virtualization.

### LXD

LXD is a higher-level container manager built around LXC, providing APIs and management features.

### LXCFS

LXCFS provides a filesystem layer that helps containers see virtualized `/proc` and `/sys` information in a way that is more consistent with traditional VMs.

### Docker

Docker focuses heavily on application containers, images, portability and developer workflows.

A useful interview distinction:

> LXC is a container technology/runtime ecosystem, while Docker is an application-container platform with image/build/registry/networking/storage workflows.

## 3. `docker build`: Common Problems

### Re-downloading packages on every build

Poor Dockerfile layer ordering can invalidate the cache.

Example:

```dockerfile
COPY . .
RUN apt-get update && apt-get install -y curl
```

If the application source changes, the package-install layer may be rebuilt depending on how the Dockerfile is structured.

Better:

```dockerfile
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
```

The dependency layer can now remain cached when only application code changes.

### Secrets leaking into image history

Avoid:

```bash
docker build --build-arg PASSWORD=mysecret .
```

Build arguments can become visible through image metadata/history and should not be treated as a secure secret store.

Use BuildKit secret mounts instead. See [Docker Build & BuildKit](02-docker-build-buildkit.md).

### Architecture mismatch

An image built for one CPU architecture may not run on another.

Examples:

- `linux/amd64`
- `linux/arm64`

Use multi-platform builds when required.

### Sequential independent build stages

If stages are independent, BuildKit can improve build execution by parallelizing work where possible.

## 4. Image vs Container

### Image

A read-only, layered package containing the application, runtime, libraries and filesystem content required to create a container.

### Container

A running or stopped instance created from an image, with its own writable container layer and isolated runtime environment.

```text
Image
  |
  +---- Container 1
  |
  +---- Container 2
```

## 5. Image Name Anatomy

Example:

```text
docker.io/library/nginx:latest
```

General structure:

```text
registry / namespace / repository : tag
```

For example:

```text
docker.io / library / nginx : latest
```
