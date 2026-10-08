# Linux Namespaces & cgroups

## 1. Namespaces

Linux namespaces provide isolation.

They allow processes inside a container to have an isolated view of system resources.

Important namespace types include:

- **PID** - process IDs
- **NET** - network interfaces/routes
- **MNT** - mount points
- **UTS** - hostname/domain name
- **IPC** - inter-process communication
- **USER** - user/group IDs

Example concept:

```text
Host
 ├── PID namespace
 │    ├── container process
 │    └── child processes
 │
 └── another PID namespace
      └── another container process
```

A process can therefore see a different process tree inside the container.

## 2. cgroups

Control groups (**cgroups**) limit and account for resource usage.

Common resources:

- CPU
- Memory
- PIDs
- Block I/O

### CPU example

```bash
docker run --cpus=0.5 ubuntu
```

This limits the container to approximately 0.5 CPU worth of compute capacity.

It does **not** mean the container gets a permanently reserved 50% of one host CPU.

### Memory example

```bash
docker run --memory=100m ubuntu
```

This sets a memory limit of approximately 100 MB.

If a container exceeds its memory limit, the kernel may invoke the OOM mechanism and a process can be killed.

## 3. Namespaces vs cgroups

| Feature | Purpose |
|---|---|
| Namespaces | Isolation / what the process can see |
| cgroups | Resource control / how much it can use |

Easy interview answer:

> Namespaces isolate a container from other processes and system resources, while cgroups control and account for resource consumption.
