# 🐳 01 — Images and Containers

> *"Images are the recipe. Containers are the cake."*

---

## 📌 The Docker Mental Model

The most common source of confusion for Docker beginners: **the difference between an image and a container.**

Think of it like a **Cookie Cutter and Cookies**:

```
┌─────────────────────────────────────────────────────────────────┐
│  IMAGE = The Cutter                                              │
│  - Static template stored on disk                               │
│  - "Ubuntu + Node.js 18 + my app files"                         │
│  - You can have 5 cutters (images) but they all make           │
│    the same shape (containers)                                  │
│  - Images are read-only (you can't bake IN the cutter)         │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  CONTAINER = The Cookie                                         │
│  - A running instance of an image (the actual process)          │
│  - Has its own writable layer on top of the image layers        │
│  - Isolated from other containers and the host                   │
│  - Start, stop, delete, inspect — it's a running process       │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🏗️ Docker Architecture (Simplified)

```
┌─────────────────────────────────────────────────────────────────┐
│  YOUR COMPUTER (Host)                                           │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  DOCKER CLIENT (CLI)                                     │    │
│  │  docker run, docker build, docker ps...                 │    │
│  └───────────────────────┬─────────────────────────────────┘    │
│                          │                                       │
│                          ▼                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  DOCKER DAEMON (dockerd)                                 │    │
│  │  - Builds images                                         │    │
│  │  - Manages containers                                    │    │
│  │  - Pulls/pushes from registry                            │    │
│  └───────────────────────┬─────────────────────────────────┘    │
│                          │                                       │
│                          ▼                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  CONTAINER RUNTIME (containerd)                          │    │
│  │  - Actually runs containers (OCI standard)              │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  REGISTRY (Docker Hub / Private)                         │    │
│  │  - Stores and distributes images                         │    │
│  └─────────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📋 The Container Lifecycle

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│   docker run              docker start                          │
│  ────────────►           ────────────►                         │
│                                                                 │
│  ┌────────┐           ┌────────┐                              │
│  │ CREATED│──────────►│ RUNNING│                              │
│  └────────┘           └────┬───┘                              │
│       ▲                   │                                    │
│       │ docker stop       │ docker kill                        │
│       │                   │                                    │
│  ┌────┴───┐           ┌───┴────┐                              │
│  │ STOPPED│◄──────────│ KILLED │                              │
│  └────────┘           └────────┘                              │
│       │                   │                                    │
│       │ docker rm         │ docker rm                          │
│       ▼                   ▼                                    │
│  ┌────────────────────────────────┐                           │
│  │         DELETED (Gone)          │                           │
│  └────────────────────────────────┘                           │
│                                                                 │
│  docker pause              docker unpause                        │
│  ────────────►           ────────────►                         │
│  ┌────────┐           ┌────────┐                              │
│  │PAUSED │◄─────────│RUNNING │                              │
│  └────────┘           └────────┘                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

| State | What It Means |
|-------|---------------|
| **Created** | Container exists but hasn't started |
| **Running** | Process is actively executing |
| **Paused** | Process frozen (not terminated) |
| **Stopped** | Process exited gracefully |
| **Killed** | Process terminated forcefully |
| **Deleted** | Gone from disk |

---

## ✅ What TO Do

### DO: Use Tags for Version Control
```bash
# Specific version: always know what you're running
docker build -t myapp:1.2.3 .
docker run myapp:1.2.3

# Latest: use cautiously in production (no predictability)
docker build -t myapp:latest .
```

### DO: Inspect Before Troubleshooting
```bash
# First stop when something breaks — see everything
docker inspect my_container

# See logs for the error
docker logs my_container

# See live resource usage
docker stats my_container
```

### DO: Clean Up After Yourself
```bash
# Remove stopped containers
docker container prune

# Remove unused images
docker image prune -a

# Nuclear option: remove everything unused
docker system prune -a
```

---

## 🚫 What NOT to Do

### DON'T: Run Containers Without Names for Production
```bash
# Bad: cryptic ID, impossible to manage
docker run nginx

# Good: self-documenting, predictable
docker run --name web_server nginx
```

### DON'T: Use `:latest` in Production
```bash
# Bad: Today it's v1.2.3. Tomorrow it's v2.0.0 (maybe breaking)
docker run myapp:latest

# Good: exact version, reproducible
docker run myapp:1.2.3
```

### DON'T: Store Data Inside Containers
```bash
# Bad: Data dies with the container
docker run -v /app/data mysql  # data disappears on container delete

# Good: Use volumes for persistence
docker run -v mysql_data:/var/lib/mysql mysql
```

### DON'T: Ignore `--rm` for Short-Lived Containers
```bash
# Good: auto-cleanup for temporary/ad-hoc containers
docker run --rm ubuntu sleep 5
# Container automatically deleted after exit
```

---

## 🎯 Why This Matters

### In the Workplace: Idempotency
A container that's `docker run nginx` once should be `docker run nginx` forever. If you manually `apt install` something inside a container and the container dies, your changes are gone. Dockerfile-based images are **idempotent** — they survive restarts.

### In the Workplace: Resource Efficiency
Docker containers start in **milliseconds**, not minutes like VMs. A single host can run hundreds of containers. In production, this means you can scale from 1 to 100 instances of your app in seconds, not hours.

---

## 🧠 Mental Model: Apartments vs. Hotels

| Hotel (VM) | Apartment (Container) |
|------------|----------------------|
| Full kitchen, bathroom, living room | Shared amenities, smaller space |
| Heavy, slow to build, slow to move | Light, fast to start, portable |
| Complete isolation | Isolated but efficient |
| **Docker Container** | **Traditional VM** |

You don't need a full apartment (VM) when you just need a place to sleep (run a process).

---

## 📚 Technical Glossary

- **Image:** A read-only template with instructions (Dockerfile) for creating containers.
- **Container:** A running instance of an image with its own isolated filesystem.
- **Layer:** A modification to an image. Images are built from stacked layers.
- **Dockerfile:** A text file with step-by-step instructions to build an image.
- **Registry:** A storage location for Docker images (Docker Hub, ECR, GCR, etc.).

---

[⬅️ Back to Parent](../README.md)
