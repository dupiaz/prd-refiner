---
name: docker-devcontainer-windows
description: "Use when setting up a Docker-based development environment on Windows. Covers DevContainer configuration, docker-compose for dev stacks (Next.js + PostgreSQL + Redis), WSL2 optimizations, HMR fixes, and hybrid host-code/containerized-services approach."
risk: low
source: custom
date_added: "2026-03-11"
---

# Docker DevContainer for Windows

Set up a consistent, containerized development environment optimized for Windows with WSL2.

## When to Use

- Setting up a new web app project with Docker
- Configuring `.devcontainer/` for VS Code
- Creating `docker-compose.yml` for local development
- Fixing HMR/Hot Reload issues on Windows
- Setting up a hybrid development workflow (host code + containerized services)

## Core Principles

1. **Hybrid approach** on Windows — code on host, services in containers
2. **WSL2 first** — always use Docker Desktop WSL2 backend
3. **Pin versions** — lock Node, PostgreSQL, Redis versions
4. **Health checks** — ensure services are ready before app connects

---

## Quick Start: Hybrid Development Stack

This is the **recommended approach for Windows**: keep your source code on the host (for IDE/HMR performance) while running database and cache inside containers.

### docker-compose.yml (Development)

```yaml
# docker-compose.yml — Development Stack
services:
  db:
    image: postgres:16-alpine
    restart: unless-stopped
    volumes:
      - pgdata:/var/lib/postgresql/data
      - ./scripts/init-db.sql:/docker-entrypoint-initdb.d/01-init.sql
    environment:
      POSTGRES_USER: ${DB_USER:-postgres}
      POSTGRES_PASSWORD: ${DB_PASSWORD:-postgres}
      POSTGRES_DB: ${DB_NAME:-devdb}
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER:-postgres}"]
      interval: 5s
      timeout: 5s
      retries: 5
      start_period: 10s
    ports:
      - "${DB_PORT:-5432}:5432"

  cache:
    image: redis:7-alpine
    restart: unless-stopped
    ports:
      - "${REDIS_PORT:-6379}:6379"
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 5s
      timeout: 3s
      retries: 5

  # Optional: pgAdmin for DB management
  pgadmin:
    image: dpage/pgadmin4:latest
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@local.dev
      PGADMIN_DEFAULT_PASSWORD: admin
    ports:
      - "5050:80"
    depends_on:
      db:
        condition: service_healthy
    profiles:
      - tools

volumes:
  pgdata:
```

### .env (Development)

```bash
# .env — DO NOT COMMIT (add to .gitignore)
# Database
DB_USER=postgres
DB_PASSWORD=postgres
DB_NAME=devdb
DB_PORT=5432
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/devdb

# Redis
REDIS_PORT=6379
REDIS_URL=redis://localhost:6379

# App
NODE_ENV=development
NEXT_PUBLIC_APP_URL=http://localhost:3000

# Auth (Better Auth)
BETTER_AUTH_SECRET=dev-secret-change-in-production
BETTER_AUTH_URL=http://localhost:3000

# Stripe (test mode)
STRIPE_SECRET_KEY=sk_test_...
STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
```

### Usage

```bash
# Start services (DB + Redis)
docker compose up -d

# Start app on host (separate terminal)
npm run dev

# View DB admin (optional)
docker compose --profile tools up -d pgadmin

# Stop all
docker compose down

# Reset DB (destroy data)
docker compose down -v
```

---

## Full DevContainer Setup (Alternative)

Use when you want **everything** in a container (not recommended for Windows HMR performance, but great for consistency).

### .devcontainer/devcontainer.json

```json
{
  "name": "Web App Dev",
  "dockerComposeFile": ["../docker-compose.yml", "docker-compose.extend.yml"],
  "service": "app",
  "workspaceFolder": "/app",
  "features": {
    "ghcr.io/devcontainers/features/node:1": {
      "version": "20"
    },
    "ghcr.io/devcontainers/features/git:1": {},
    "ghcr.io/devcontainers/features/github-cli:1": {}
  },
  "customizations": {
    "vscode": {
      "extensions": [
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode",
        "bradlc.vscode-tailwindcss",
        "ms-playwright.playwright"
      ],
      "settings": {
        "terminal.integrated.defaultProfile.linux": "bash"
      }
    }
  },
  "postCreateCommand": "npm ci",
  "forwardPorts": [3000, 5432, 6379],
  "remoteUser": "node"
}
```

### .devcontainer/docker-compose.extend.yml

```yaml
services:
  app:
    build:
      context: ..
      dockerfile: .devcontainer/Dockerfile
    volumes:
      - ..:/app:cached
      - node_modules:/app/node_modules
    environment:
      - CHOKIDAR_USEPOLLING=true
      - WATCHPACK_POLLING=true
      - DATABASE_URL=postgresql://postgres:postgres@db:5432/devdb
      - REDIS_URL=redis://cache:6379
    command: sleep infinity
    depends_on:
      db:
        condition: service_healthy

volumes:
  node_modules:
```

### .devcontainer/Dockerfile

```dockerfile
FROM node:20-bookworm-slim

# Install dev tools
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      git curl ca-certificates && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Don't run as root
USER node
```

---

## Windows-Specific Fixes

### HMR / Hot Reload Not Working

**Symptom**: Changes to files don't trigger page reload in Next.js/Vite.

**Root cause**: Docker Desktop on Windows with bind-mount volumes doesn't propagate filesystem events from NTFS to the container's inotify system.

**Fix 1: Polling (works always, slightly slower)**

```bash
# In .env or docker-compose environment
CHOKIDAR_USEPOLLING=true
WATCHPACK_POLLING=true
```

For Next.js, add to `next.config.js`:
```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  webpack: (config) => {
    config.watchOptions = {
      poll: 1000,
      aggregateTimeout: 300,
    };
    return config;
  },
};
module.exports = nextConfig;
```

**Fix 2: WSL2 filesystem (recommended, native performance)**

Move your source code into the WSL2 filesystem:
```bash
# From Windows Terminal (WSL2)
cd ~
mkdir -p projects
git clone <your-repo> ~/projects/my-app
cd ~/projects/my-app
code .  # Opens VS Code with WSL remote
```

### Line Endings (CRLF → LF)

**Symptom**: Shell scripts fail with `\r: command not found`.

**Fix**: Create `.gitattributes` in project root:
```
* text=auto eol=lf
*.bat text eol=crlf
*.cmd text eol=crlf
*.ps1 text eol=crlf
```

### Path Length Issues

**Symptom**: `node_modules` installations fail due to Windows 260 char limit.

**Fix**: Use anonymous volumes for `node_modules`:
```yaml
volumes:
  - .:/app
  - /app/node_modules  # Anonymous volume — bypass Windows path limit
```

### Docker Desktop Performance

**Symptom**: Docker is slow, high CPU usage.

**Fix checklist**:
- [ ] Settings → General → Use WSL2 based engine ✅
- [ ] Settings → Resources → WSL Integration → Enable for your distro ✅
- [ ] Settings → Resources → Memory → Limit to 4-8 GB
- [ ] Avoid Hyper-V backend (slower than WSL2)

---

## Decision Matrix

| Scenario | Approach | Why |
|----------|----------|-----|
| **Windows dev, fast HMR needed** | Hybrid: host code + container services | Best HMR performance |
| **Team consistency critical** | Full DevContainer | Everyone gets same environment |
| **WSL2 available** | Code in WSL + Docker WSL2 backend | Near-native performance |
| **CI/CD pipeline** | Full container stack | Reproducible, isolated |
| **Quick prototype** | Host-only, no Docker | Zero overhead |

## Checklist: New Project Setup

- [ ] Create `docker-compose.yml` with DB + Redis services
- [ ] Create `.env` from template (add to `.gitignore`)
- [ ] Create `.gitattributes` with `eol=lf`
- [ ] Update `.dockerignore` (use `docker-development` skill patterns)
- [ ] Test `docker compose up -d` → services healthy
- [ ] Test `npm run dev` → app connects to containerized DB
- [ ] Verify HMR works (edit a file → check browser)
