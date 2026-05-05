# 🎭 03 — Docker Compose

> *"One container is a tool. Ten containers working together is an architecture."*

---

## 📌 The Problem of Multi-Container Applications

In the real world, your application isn't just one process. It's a:
- **Web server** (NGINX/Apache)
- **Backend API** (Node.js/Java/Python)
- **Database** (PostgreSQL/MySQL)
- **Cache** (Redis)
- **Message Queue** (RabbitMQ)
- **Search engine** (Elasticsearch)

Each needs its own container. Managing 6 `docker run` commands — with networks, volumes, port mappings, and dependencies — is a **nightmare**.

**Docker Compose** solves this by letting you define your entire architecture in one YAML file.

---

## 🏗️ Docker Compose Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│  docker-compose.yml                                              │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  services:                                               │   │
│  │    web:          (your NGINX/frontend)                   │   │
│  │    api:          (your Node.js/Python backend)          │   │
│  │    database:     (PostgreSQL)                           │   │
│  │    cache:        (Redis)                                │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  networks: (docker network create shared_backend)      │   │
│  │    backend: (internal network, db not exposed)         │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  volumes: (docker volume create persistent_data)        │   │
│  │    db_data: (shared between compose runs)               │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘

                           │
                           │ docker-compose up
                           ▼

┌─────────────────────────────────────────────────────────────────┐
│  NETWORK: shared_backend                                         │
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐           │
│  │  web:80     │  │  api:3000   │  │  cache:6379 │           │
│  │  (NGINX)    │──│──(Node.js)  │──│──(Redis)    │           │
│  └─────────────┘  └──────┬──────┘  └─────────────┘           │
│                          │                                     │
│                   ┌──────▼──────┐                             │
│                   │  database   │                             │
│                   │  (Postgres) │                             │
│                   │  :5432      │                             │
│                   └─────────────┘                             │
│                                                                 │
│  All services share 'shared_backend' network                   │
│  'web' can reach 'api' but NOT 'database' directly (subnet)   │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📋 docker-compose.yml Reference

```yaml
version: "3.8"  # Docker Compose file format version

services:
  # --- Web Frontend ---
  web:
    build: ./web  # Build from ./web/Dockerfile
    ports:
      - "80:80"   # Host port : Container port
    depends_on:
      - api       # Start 'api' before 'web'
    networks:
      - frontend
      - backend   # Can reach services on 'backend'

  # --- API Backend ---
  api:
    build: ./api
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgres://database:5432/mydb
    depends_on:
      - database
      - cache
    networks:
      - backend   # Cannot be reached from 'frontend'

  # --- Database ---
  database:
    image: postgres:15-alpine
    volumes:
      - db_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=mydb
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=secret
    networks:
      - backend   # Isolated, web cannot see this

  # --- Cache ---
  cache:
    image: redis:7-alpine
    networks:
      - backend

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge

volumes:
  db_data:  # Named volume — persists across compose runs
```

---

## ✅ What TO Do

### DO: Use `depends_on` for Startup Order
```yaml
# Services start in order: database → api → web
services:
  web:
    depends_on:
      - api
  api:
    depends_on:
      - database
```

### DO: Use Environment Variables for Secrets (in Dev)
```yaml
# Good for dev: pass via .env or docker-compose.yml
services:
  api:
    environment:
      - DATABASE_URL=postgres://user:pass@database:5432/mydb
```

### DO: Use Named Volumes for Persistence
```yaml
volumes:
  db_data:  # Created automatically if not exists
```

### DO: Use `.env` for Environment Variables
```bash
# .env file
DATABASE_PASSWORD=supersecret123

# docker-compose.yml
services:
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: ${DATABASE_PASSWORD}
```

---

## 🚫 What NOT to Do

### DON'T: Hardcode Secrets in docker-compose.yml
```yaml
# Bad: Secrets in plain text, committed to git
services:
  api:
    environment:
      - DATABASE_PASSWORD=supersecret123  # IN GIT! 😱

# Good: Use .env file (gitignored) or secrets management
environment:
  - DATABASE_PASSWORD=${DATABASE_PASSWORD}
```

### DON'T: Use `links` (Deprecated)
```yaml
# Bad: Old syntax, deprecated
services:
  api:
    links:
      - database

# Good: Use default network + service name as hostname
services:
  api:
    # 'database' is automatically resolvable as hostname
```

### DON'T: Use `:latest` Tags
```yaml
# Bad: Unpredictable, different version tomorrow
image: postgres:latest

# Good: Pin to specific version
image: postgres:15-alpine
```

---

## 🎯 Why This Matters

### In the Workplace: Reproducibility
With a single `docker-compose up`, a new developer can have the entire stack running in minutes — not hours of installation and configuration.

### In the Workplace: Consistent Environments
Production bugs that don't reproduce locally usually come from environment differences. Docker Compose ensures development, staging, and production all run the same images, same networks, same volumes.

---

## 🧠 Mental Model: The Apartment Building

| Single Family Home | Apartment Building (Docker Compose) |
|-------------------|-------------------------------------|
| One person, one utility | Multiple apartments sharing infrastructure |
| If the furnace breaks, no hot water | One service down doesn't take down the whole building |
| Hard to duplicate | Exact copies of floor plans |
| **Single Container** | **Multi-Container Architecture** |

Each apartment (service) has its own address (port), its own utility connection (volume), and shares the building infrastructure (network).

---

## 📚 Technical Glossary

- **docker-compose.yml:** A YAML file defining services, networks, and volumes.
- **Service:** A container definition (image or build) with its configuration.
- **depends_on:** Declares startup order dependencies between services.
- **Network:** A Docker network that isolates and connects services.
- **Named Volume:** A volume managed by Docker, surviving container deletions.

---

[⬅️ Back to Parent](../README.md)
