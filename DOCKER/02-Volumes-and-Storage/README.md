# 💾 02 — Volumes and Storage

> *"Containers are born ephemeral and die ephemeral. Volumes remember."*

---

## 📌 The Storage Problem

Remember the First Law of Containers: **Containers are ephemeral.**

If you create a file inside a running container, shut down the container, and restart it — that file is **gone**. The container's filesystem is a thin writable layer on top of the image layers. Delete the container, delete the layer.

But real applications need to **persist data**. Databases store terabytes. User uploads need to survive restarts. Configuration files must survive updates.

The solution: **Volumes**.

---

## 🏗️ Storage Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│  WITHOUT VOLUMES: Data Dies With Container                      │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  CONTAINER LAYERS (Image layers, read-only)             │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Ubuntu + Node.js + MyApp (immutable)           │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Writable Layer (ephemeral, dies with container)│    │    │
│  │  │  → /var/lib/mysql (DATA HERE!)                   │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                          │ docker rm                            │
│                          ▼                                      │
│                    DATA IS GONE 😱                              │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  WITH VOLUMES: Data Survives                                    │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  CONTAINER                                               │    │
│  │  ┌─────────────────┐                                     │    │
│  │  │ Image layers    │                                     │    │
│  │  └────────┬────────┘                                     │    │
│  │           │                                                │    │
│  │  ┌────────▼────────┐                                     │    │
│  │  │ Writable layer  │                                     │    │
│  │  └────────┬────────┘                                     │    │
│  └───────────┼───────────────────────────────────────────────┘    │
│              │                                                   │
│              │ mount point: /var/lib/mysql                       │
│              │                                                   │
│  ┌───────────▼───────────────────────────────────────────────┐  │
│  │  VOLUME (Host filesystem, persisted)                       │  │
│  │  Lives at: /var/lib/docker/volumes/mysql_data/_data        │  │
│  └───────────────────────────────────────────────────────────┘  │
│                          │ docker rm                             │
│                          ▼                                       │
│              DATA SURVIVES 😎 (mounted to new container)        │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📋 Volume Types

| Type | When to Use | Syntax |
|------|-------------|--------|
| **Named Volume** | Real data (databases, user uploads) — managed by Docker | `-v mysql_data:/var/lib/mysql` |
| **Bind Mount** | Development files (live code reloading) — host files | `-v $(pwd):/app` |
| **tmpfs Mount** | Sensitive data (secrets) — stored in memory only | `--tmpfs /run/secrets` |
| **Anonymous Volume** | Fallback for legacy Dockerfiles — avoid in new work | `-v /var/lib/mysql` |

---

## ✅ What TO Do

### DO: Use Named Volumes for Real Data
```bash
# Create volume explicitly
docker volume create mysql_data

# Use it — data survives container deletion
docker run -v mysql_data:/var/lib/mysql -d mysql

# Inspect where it lives on host
docker volume inspect mysql_data
```

### DO: Use Bind Mounts for Development
```bash
# Your source code on host → container at /app
# Changes to code on host appear instantly in container
docker run -v $(pwd):/app -p 3000:3000 node_app

# Perfect for: hot reloading, debugging, live testing
```

### DO: Use Read-Only Mounts When Appropriate
```bash
# Container can read the config but not modify it
docker run -v ./config:/etc/config:ro myapp
```

---

## 🚫 What NOT to Do

### DON'T: Store Data in the Container's Writable Layer
```bash
# Bad: Data inside container is gone after rm
docker run mysql
# database files at /var/lib/mysql inside container
docker rm mysql  # DATA GONE

# Good: Use a named volume
docker run -v mysql_data:/var/lib/mysql mysql
docker rm mysql  # DATA SURVIVES
```

### DON'T: Use Bind Mounts in Production
```bash
# Bad: Host directory permissions may differ from container
# File ownership issues: container runs as root, host as user
docker run -v /home/user/app:/app production_app

# Good: Use named volumes in production
docker run -v app_data:/app production_app
```

### DON'T: Mix Up Volume Syntaxes
```bash
# Different syntaxes, same result (named volume):
docker run -v mysql_data:/var/lib/mysql mysql
docker run --mount type=volume,source=mysql_data,target=/var/lib/mysql mysql

# Bind mount (host directory):
docker run -v $(pwd):/app app  # -v shorthand
docker run --mount type=bind,source=$(pwd),target=/app app  # explicit
```

---

## 🎯 Why This Matters

### In the Workplace: Data Durability
Production databases hold months or years of customer data. Losing it means lawsuits, lost trust, and potentially company-ending events. Named volumes with regular backups are non-negotiable.

### In the Workplace: State Preservation
In CI/CD pipelines, containers are created and destroyed constantly. If your app stores sessions in memory, every deployment kicks all users out. With volumes, session data persists across deployments.

---

## 🧠 Mental Model: The Life Preserver

| Without Volume | With Volume |
|----------------|--------------|
| Data is inside the person wearing it | Life preserver is separate from the person |
| Person drowns (container dies) = data drowns | Person drowns, life preserver (volume) floats |
| Other swimmers can't retrieve it | Data can be picked up by a new container |
| **Catastrophic data loss** | **Data survives** |

---

## 📚 Technical Glossary

- **Volume:** A specially managed directory in the Docker host filesystem, isolated from the container lifecycle.
- **Bind Mount:** Maps a host directory directly into the container. No Docker management.
- **tmpfs:** A volume stored in memory, never written to disk. For secrets.
- **Mount Point:** The location inside the container where the volume is attached.
- **Data Persistence:** The property of data that allows it to survive beyond the lifetime of the process that created it.

---

[⬅️ Back to Parent](../README.md)
