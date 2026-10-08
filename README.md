# Docker Revision Notes

My Docker revision notes, organized by topic for quick reference and interview preparation.

## Topics Covered

1. [Docker Core Concepts](01-docker-core-concepts.md)
2. [Docker Build & BuildKit](02-docker-build-buildkit.md)
3. [Docker Init, Entrypoint, CMD, Environment Variables & Compose](03-docker-runtime-basics.md)
4. [Docker Engine & Container Runtime Architecture](04-docker-engine-runtime.md)
5. [Namespaces & cgroups](05-namespaces-cgroups.md)
6. [Docker Storage](06-docker-storage.md)
7. [Docker Networking](07-docker-networking.md)
8. [Docker Registry](08-docker-registry.md)
9. [Docker Swarm Basics](09-docker-swarm.md)

## Quick Revision Flow

```text
Docker CLI
   |
   v
Docker Engine / dockerd
   |
   v
containerd
   |
   v
runc
   |
   v
Linux kernel
   ├── namespaces
   └── cgroups
```

## Commands Worth Remembering

```bash
docker build -t myapp:latest .
docker buildx build -t myapp:latest .

docker run -d --name web nginx
docker run -e APP_ENV=prod nginx

docker run --cpus=0.5 ubuntu
docker run --memory=100m ubuntu

docker volume create app-data
docker network create my-network

docker login
docker pull nginx:latest
docker tag myapp:latest localhost:5000/myapp:latest
docker push localhost:5000/myapp:latest

curl localhost:5000/v2/_catalog
```

> These are revision notes rather than a complete Docker reference. Commands and examples are intentionally kept practical.
