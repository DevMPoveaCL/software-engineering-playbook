# 🚪 Dev Ports and Environments

> **Why this matters:** Every service your app connects to has a "door number." Knowing which port does what helps you debug faster and deploy correctly.

---

## 🧠 Mental Model: The Office Building

Imagine your server (computer) is a **large office building**:

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│              YOUR SERVER (Office Building)                       │
│                                                                  │
│    ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐           │
│    │ PORT    │  │ PORT    │  │ PORT    │  │ PORT    │           │
│    │   22    │  │   80    │  │  443    │  │  3000   │           │
│    │  SSH    │  │  HTTP   │  │  HTTPS  │  │ React/  │           │
│    │ Door    │  │ Door    │  │ Door    │  │ Node    │           │
│    └─────────┘  └─────────┘  └─────────┘  └─────────┘           │
│                                                                  │
│    ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐           │
│    │ PORT    │  │ PORT    │  │ PORT    │  │ PORT    │           │
│    │  5432   │  │  6379   │  │  27017  │  │  8080   │           │
│    │ Postgres│  │  Redis  │  │ MongoDB │  │ Tomcat  │           │
│    │  DB     │  │  Cache  │  │  DB     │  │ Server  │           │
│    └─────────┘  └─────────┘  └─────────┘  └─────────┘           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**Ports are doors.** Each service "listens" at a specific door. When you connect to `localhost:3000`, you're knocking on the React dev server's door.

---

## 📋 Essential Dev Ports Table

### Development Ports (Frontend & Scripting)

| Port | Technology | Common Use |
|------|------------|------------|
| **3000** | React (CRA), Node.js | Default dev server |
| **3001** | React (alternate) | Second instance, API proxy |
| **5173** | Vite | Modern fast dev server |
| **4200** | Angular | Default Angular dev server |
| **8000** | Python Django | Django development server |
| **5000** | Python Flask | Flask default (debug mode) |
| **8080** | Various | Alternative HTTP, Spring Boot |
| **9200** | Elasticsearch | ES default HTTP API |
| **3000** | Gatsby | Gatsby dev server |

### Backend Framework Ports

| Port | Technology | Common Use |
|------|------------|------------|
| **8080** | Spring Boot (Java) | Default embedded server |
| **3000** | Next.js (Node) | Full-stack React framework |
| **5000** | ASP.NET Core (old) | .NET default (newer uses random) |
| **8000** | Rust Axum/Warp | Rust web frameworks |
| **4000** | Elixir/Phoenix | Phoenix default dev server |
| **5432** | Ruby on Rails | Rails default (via Unix socket) |

### Database Ports

| Port | Technology | Common Use |
|------|------------|------------|
| **5432** | PostgreSQL | Default Postgres port |
| **3306** | MySQL | Default MySQL port |
| **27017** | MongoDB | Default MongoDB port |
| **6379** | Redis | Default Redis port |
| **9042** | Cassandra | CQL native transport |
| **27018** | MongoDB (cluster) | MongoDB shard member |
| **5433** | PostgreSQL (alternate) | Second instance |

### Infrastructure Ports

| Port | Technology | Common Use |
|------|------------|------------|
| **22** | SSH | Secure shell access |
| **80** | HTTP | Insecure web traffic |
| **443** | HTTPS | Secure web traffic |
| **2375** | Docker | Docker daemon ( unsecured) |
| **2376** | Docker | Docker daemon ( TLS) |
| **3306** | MySQL | Database access |
| **5432** | PostgreSQL | Database access |
| **5672** | RabbitMQ | AMQP protocol |
| **15672** | RabbitMQ | Management UI |
| **9200** | Elasticsearch | HTTP API |
| **5601** | Kibana | Web UI for Elasticsearch |

### Messaging & Queue Ports

| Port | Technology | Common Use |
|------|------------|------------|
| **9092** | Kafka | Kafka broker PLAINTEXT |
| **9093** | Kafka | Kafka broker SSL |
| **9094** | Kafka | Zookeeper connection |
| **5672** | RabbitMQ | AMQP |
| **15672** | RabbitMQ | Management UI |
| **6379** | Redis | Pub/Sub |

---

## 🌐 Standard Well-Known Ports

| Port | Protocol | Use Case |
|------|----------|----------|
| **20** | FTP | FTP data transfer (active mode) |
| **21** | FTP | FTP control commands |
| **22** | SSH | Secure shell (encrypted) |
| **25** | SMTP | Email sending (unencrypted) |
| **53** | DNS | Domain name resolution |
| **80** | HTTP | Web traffic (unencrypted) |
| **110** | POP3 | Email retrieval |
| **143** | IMAP | Email access/management |
| **443** | HTTPS | Secure web traffic |
| **465** | SMTPS | Email sending (secure) |
| **587** | SMTP | Email submission (modern) |
| **993** | IMAPS | Email access (secure) |
| **995** | POP3S | Email retrieval (secure) |

---

## 🔄 Environment-Based Ports (Development Workflow)

### ASCII: Multi-Environment Setup

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│   DEVELOPER MACHINE                                             │
│                                                                 │
│   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐       │
│   │  localhost   │   │  localhost   │   │  localhost   │       │
│   │    :3000    │   │    :8080     │   │   :5432      │       │
│   │   React     │   │  Spring Boot │   │  PostgreSQL  │       │
│   │  Frontend   │   │    Backend   │   │   Database   │       │
│   └──────────────┘   └──────────────┘   └──────────────┘       │
│                                                                 │
│                        │                                        │
│                        │ (via Docker Compose / Network)         │
│                        ▼                                        │
│   ┌──────────────────────────────────────────────────────┐     │
│   │              DOCKER COMPOSE NETWORK                   │     │
│   │                                                      │     │
│   │   postgres:5432   ← Container name resolves to IP     │     │
│   │   redis:6379                                        │     │
│   │   api:8080                                          │     │
│   │   frontend:3000                                     │     │
│   │                                                      │     │
│   └──────────────────────────────────────────────────────┘     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Docker Compose Example

```yaml
# docker-compose.yml
services:
  postgres:
    image: postgres:15
    ports:
      - "5432:5432"    # Host:Container
    
  redis:
    image: redis:7
    ports:
      - "6379:6379"
    
  api:
    build: ./api
    ports:
      - "8080:8080"
    depends_on:
      - postgres
      - redis
    environment:
      - DATABASE_URL=postgres://postgres:5432/mydb
      - REDIS_URL=redis://redis:6379
  
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    environment:
      - REACT_APP_API_URL=http://localhost:8080
```

---

## 🔍 Port Debugging Commands

### Find What's Using a Port

```bash
# Windows
netstat -ano | findstr :3000

# macOS/Linux
lsof -i :3000
# or
netstat -tlnp | grep 3000
```

### Kill Process on Port

```bash
# Windows (kill by PID from netstat)
netstat -ano | findstr :3000
# Find PID, then:
taskkill /PID 12345 /F

# macOS/Linux
lsof -ti :3000 | xargs kill -9
```

---

## ✅ What TO Do

| Do This | Why |
|---------|-----|
| **Document default ports for your stack** | New devs know what to expect |
| **Use environment variables** | `PORT=${PORT:-3000}` for flexibility |
| **Never expose databases on public IPs** | They're internal services |
| **Use reverse proxy (nginx) in production** | Single entry point, better security |
| **Check if port is available before binding** | Avoid cryptic "address in use" errors |

---

## 🚫 What NOT To Do

| Don't Do This | Why Not |
|---------------|---------|
| **Don't hardcode ports in code** | Dev :3000, prod might need :8080 |
| **Don't run as root in containers** | Security risk |
| **Don't expose ports to internet without auth** | Database breach waiting to happen |
| **Don't ignore "port already in use"** | Kill the process first |
| **Don't use ports below 1024 for dev** | Requires admin privileges |

---

[⬅️ Previous: 01 Web Protocols](../01-Web-Protocols/README.md) | [⬅️ Back to Parent](../README.md) | [➡️ Next: 03 Security and JWT](../03-Security-and-JWT/README.md)
