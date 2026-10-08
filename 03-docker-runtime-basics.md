# Docker Runtime Basics

## 1. Docker Init

`docker init` helps generate starter Docker configuration for an application.

Depending on the application, it can generate files such as:

- `Dockerfile`
- `.dockerignore`
- Docker Compose configuration

It is useful for bootstrapping containerization rather than being a runtime component.

## 2. ENTRYPOINT vs CMD

This is an important Docker interview topic.

### CMD

Provides the default command or default arguments.

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

### ENTRYPOINT

Defines the main executable for the container.

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

Running:

```bash
docker run myimage
```

effectively gives:

```text
python app.py
```

### Override behavior

`docker run image another-command` normally replaces `CMD`.

An exec-form `ENTRYPOINT` remains the executable unless explicitly overridden with:

```bash
docker run --entrypoint /bin/sh image
```

### Why exec form matters

Prefer:

```dockerfile
ENTRYPOINT ["python", "app.py"]
```

over:

```dockerfile
ENTRYPOINT python app.py
```

Exec form makes signal handling and PID 1 behavior more predictable.

## 3. Environment Variables

Set an environment variable during container startup:

```bash
docker run -e APP_ENV=production nginx
```

Inside the container:

```bash
echo $APP_ENV
```

Dockerfile example:

```dockerfile
ENV APP_ENV=production
```

Runtime values can override image defaults:

```bash
docker run -e APP_ENV=staging myapp
```

### Important

Do not treat environment variables as a secure place for highly sensitive secrets. Prefer a dedicated secret-management mechanism.

## 4. Docker Compose

Docker Compose is used to define and run multi-container applications declaratively.

Example:

```yaml
services:
  web:
    image: nginx
    ports:
      - "8080:80"

  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: example
```

Historically, Compose had multiple file-format versions. Modern Docker Compose uses the Compose Specification, so old `version:`-based tutorials may not match current practice.

Useful commands:

```bash
docker compose up -d
docker compose ps
docker compose logs
docker compose down
```

## 5. Compose vs Docker Swarm

| Docker Compose | Docker Swarm |
|---|---|
| Local/multi-container application definition | Cluster orchestration |
| Common for development/testing | Designed for clustered services |
| Uses `docker compose` | Uses `docker stack` / `docker service` |
| No built-in cluster scheduler | Has manager/worker architecture |
