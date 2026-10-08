# Docker Engine & Container Runtime Architecture

## 1. Docker Engine Components

A simplified Docker architecture:

```text
Docker CLI
   |
   | REST API
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

### Docker CLI

The CLI is the user-facing client:

```bash
docker run nginx
docker ps
docker images
docker network ls
```

It sends requests to the Docker daemon.

### Docker daemon (`dockerd`)

The daemon manages Docker-specific resources and operations such as:

- Images
- Containers
- Networks
- Volumes
- Registry interactions
- Docker API

### REST API

The Docker CLI communicates with the daemon through the Docker API.

### containerd

containerd manages the container lifecycle and related runtime operations.

### runc

runc is a low-level OCI runtime.

It works closely with the Linux kernel to create the isolated container environment using mechanisms such as:

- Namespaces
- cgroups
- Process execution

## 2. Example: `docker run`

When you execute:

```bash
docker run nginx
```

a simplified flow is:

```text
docker CLI
   |
   v
dockerd
   |
   +--> image / network / volume management
   |
   v
containerd
   |
   v
runc
   |
   v
Linux kernel
   |
   +--> namespaces
   +--> cgroups
   +--> container process
```

## 3. Important Distinction

Docker is not itself the Linux isolation mechanism.

Docker orchestrates the workflow, while the underlying Linux kernel provides the isolation and resource-control primitives.
