# Docker Networking

## 1. Docker Network Drivers

Common Docker network modes/drivers include:

- `bridge`
- `host`
- `none`

Other drivers exist for specialized use cases.

## 2. Bridge Network

The default bridge network provides container networking through a Linux bridge on the host.

Create a custom network:

```bash
docker network create   --driver bridge   --subnet 182.18.0.0/24   --gateway 182.18.0.1   wp-mysql-network
```

Run containers on it:

```bash
docker run -d --name db   --network wp-mysql-network   mysql:8
```

```bash
docker run -d --name wordpress   --network wp-mysql-network   wordpress
```

Containers on the same user-defined bridge network can communicate with each other.

## 3. Docker DNS

Docker provides DNS-based service discovery on user-defined networks.

For example, if the database container is named:

```text
db
```

another container on the same user-defined network can generally connect using:

```text
db
```

instead of depending on the container IP.

This is especially useful because container IP addresses can change.

## 4. Host Network

With host networking, the container uses the host's network namespace rather than getting an isolated container network stack.

Conceptually:

```text
Container
   |
   +---- Host network namespace
```

This reduces network isolation and can be useful for specialized workloads.

## 5. None Network

The `none` network mode provides a container with no normal external network connectivity through Docker networking.

```bash
docker run --network none alpine
```

## 6. Network Namespaces

Each normally isolated container gets its own network namespace.

It can have its own:

- Network interfaces
- Routing table
- IP addresses
- Network configuration

## 7. veth Pairs

A virtual Ethernet pair acts like a virtual cable with two ends.

Simplified:

```text
Container network namespace
        |
      veth
        |
        veth
        |
Host bridge
        |
     Host NIC
```

One end is placed inside the container's network namespace and the other connects to the host-side networking infrastructure.

## 8. Troubleshooting Commands

```bash
docker network ls
docker network inspect my-network
docker inspect <container>
docker exec -it <container> sh
```

Inside the container:

```bash
ip addr
ip route
cat /etc/resolv.conf
```
