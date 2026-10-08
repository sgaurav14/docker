# Docker Storage

## 1. Container Writable Layer

A container created from an image gets a writable layer on top of the image's read-only layers.

Important consequence:

> Data written only into the container's writable layer is tied to the container lifecycle and should not be used as durable application storage.

For persistent data, use volumes or bind mounts.

## 2. Docker Volumes

Create a volume:

```bash
docker volume create app-data
```

Use it:

```bash
docker run   --mount type=volume,src=app-data,dst=/var/lib/myapp   myapp
```

Volumes are managed by Docker and are generally preferred for persistent container data.

## 3. Bind Mounts

A bind mount maps a specific host path into the container.

```bash
docker run   --mount type=bind,src=/opt/myapp,dst=/app   myapp
```

Useful when:

- You need direct access to host files
- Developing locally
- Sharing configuration/source code

The host path is controlled explicitly by the user.

## 4. `-v` vs `--mount`

Short syntax:

```bash
docker run -v app-data:/data myapp
```

More explicit syntax:

```bash
docker run   --mount type=volume,src=app-data,dst=/data   myapp
```

`--mount` is generally clearer because the options are named.

## 5. Storage Drivers

Docker uses a storage driver to manage image layers and the container writable layer.

The exact driver depends on the host and Docker configuration.

Examples include:

- `overlay2`
- `fuse-overlayfs` in some environments

## 6. containerd Image Store

Modern Docker installations can use containerd-backed image/storage functionality. The important interview idea is that Docker's image/container storage involves multiple layers of the Docker/container runtime architecture rather than simply storing one flat image file.

## 7. Volume vs Bind Mount

| | Volume | Bind Mount |
|---|---|---|
| Managed by Docker | Yes | No |
| Host path chosen directly | Usually no | Yes |
| Good for persistent application data | Yes | Yes |
| Common for local development | Sometimes | Very common |

## 8. Practical Questions

### Where should database data live?

Prefer a persistent volume or external storage rather than the container writable layer.

### What happens when a container is deleted?

Data in the container's writable layer is removed with the container. A separately managed volume can survive container deletion.
