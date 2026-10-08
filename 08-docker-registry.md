# Docker Registry

## 1. What is a Registry?

A container registry stores and distributes container images.

Examples:

- Docker Hub
- Private enterprise registries
- Self-hosted Docker Registry

## 2. Image Name Anatomy

Example:

```text
docker.io/library/nginx:latest
```

General structure:

```text
registry / namespace / repository : tag
```

If the registry is omitted, Docker commonly uses Docker Hub according to the configured defaults.

## 3. Private Registry Example

Start a local registry:

```bash
docker run -d   -p 5000:5000   --name registry   registry:2
```

Pull an image:

```bash
docker pull httpd:latest
```

Tag it for the local registry:

```bash
docker tag httpd:latest localhost:5000/httpd:latest
```

Push it:

```bash
docker push localhost:5000/httpd:latest
```

## 4. Verify Images in the Registry

For a basic local registry:

```bash
curl localhost:5000/v2/_catalog
```

This returns the repositories known to the registry.

## 5. Pull from the Private Registry

```bash
docker pull localhost:5000/httpd:latest
```

## 6. Registry vs Repository vs Image

A useful distinction:

```text
Registry
  |
  +-- Namespace
       |
       +-- Repository
            |
            +-- Tag / Image manifest
```

A registry is the service that stores/distributes images. A repository is a named collection of related image versions.

## 7. `docker login`

For a registry requiring authentication:

```bash
docker login <registry>
```

Then:

```bash
docker push <registry>/<namespace>/<image>:<tag>
```

## 8. Interview Questions

### Why do we tag an image before pushing to a private registry?

The tag tells Docker which registry/repository destination the image should be pushed to.

### What is the difference between Docker Hub and a private registry?

Docker Hub is a hosted public/commercial registry service. A private registry can be hosted and controlled by an organization.
