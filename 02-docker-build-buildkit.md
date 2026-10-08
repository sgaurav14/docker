# Docker Build & BuildKit

## 1. BuildKit

BuildKit is Docker's modern build backend. Modern Docker installations use BuildKit for `docker build` under the hood.

The explicit Buildx command is useful when you need advanced build features:

```bash
docker buildx build -t webapp:latest .
```

## 2. What BuildKit Improves

### Content-based caching

BuildKit provides more efficient caching and can reuse build results when inputs have not changed.

### Cache mounts

Example:

```dockerfile
# syntax=docker/dockerfile:1

FROM python:3.12

RUN --mount=type=cache,target=/root/.cache/pip     pip install -r requirements.txt
```

The cache mount can preserve package-manager cache data without putting that cache into the final image layer.

### Build secrets

Instead of putting secrets in `ARG`:

```dockerfile
ARG TOKEN
```

use a BuildKit secret mount:

```dockerfile
RUN --mount=type=secret,id=mytoken     cat /run/secrets/mytoken
```

Build command:

```bash
docker buildx build   --secret id=mytoken,src=$HOME/.mytoken   -t myapp:latest .
```

The secret is mounted only for the build step and is not intended to become part of the image.

### Multi-platform builds

```bash
docker buildx build   --platform linux/amd64,linux/arm64   -t myrepo/myapp:latest   --push .
```

### Parallel execution

BuildKit can execute independent build work concurrently where the dependency graph allows it.

## 3. Multi-stage Builds

Multi-stage builds allow build dependencies to stay out of the final runtime image.

Example:

```dockerfile
FROM golang:1.24 AS builder

WORKDIR /app
COPY . .
RUN go build -o app .

FROM debian:bookworm-slim

COPY --from=builder /app/app /app
CMD ["/app"]
```

Benefits:

- Smaller final image
- Fewer packages in production
- Reduced attack surface
- Cleaner separation between build and runtime

## 4. Build Cache Strategy

Good Dockerfile ordering:

```dockerfile
COPY package*.json .
RUN npm ci

COPY . .
RUN npm run build
```

The dependency installation layer can be reused when only source code changes.

## 5. Practical Interview Questions

### Why is BuildKit better than the old builder?

It provides improved caching, secret mounts, cache mounts, multi-platform builds and more efficient build execution.

### Why should secrets not be passed using `--build-arg`?

Because build arguments are not designed as a secure secret mechanism and may be exposed through build metadata/history.

### What is a multi-stage build?

A Dockerfile using multiple `FROM` stages where artifacts are copied from a build stage into a smaller runtime stage.
