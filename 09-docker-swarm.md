# Docker Swarm Basics

## 1. What is Docker Swarm?

Docker Swarm is Docker's native container orchestration technology.

It allows multiple Docker hosts to work together as a cluster.

## 2. Swarm Node Types

### Manager

Managers handle cluster management and scheduling responsibilities.

### Worker

Workers run tasks assigned by the swarm.

Simplified:

```text
             Swarm Manager
             /     |                 /      |            Worker    Worker    Worker
```

## 3. Initialize a Swarm

```bash
docker swarm init
```

The command outputs a worker join command.

## 4. Join a Worker

On another Docker host:

```bash
docker swarm join   --token <token>   <manager-ip>:2377
```

## 5. List Nodes

```bash
docker node ls
```

## 6. Create a Service

```bash
docker service create   --name web   --publish 8080:80   nginx
```

List services:

```bash
docker service ls
```

Inspect tasks:

```bash
docker service ps web
```

## 7. Scale a Service

```bash
docker service scale web=3
```

The swarm scheduler attempts to maintain the requested number of replicas.

## 8. Update a Service

```bash
docker service update   --image nginx:latest   web
```

Swarm can perform rolling updates according to the service's update configuration.

## 9. Swarm vs Kubernetes

Docker Swarm is simpler and tightly integrated with Docker.

Kubernetes provides a much broader orchestration ecosystem and is the more common choice for large-scale modern container platforms.

For a DevOps interview, know the fundamentals of Swarm, but prioritize Kubernetes deeply.
