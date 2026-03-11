---
name: docker-production-deploy
description: "Use when building production Docker images, pushing to registries, or deploying containerized apps. Covers multi-stage builds, security scanning, registry management, and deploy to Vercel/Fly.io/Railway."
risk: low
source: custom
date_added: "2026-03-11"
---

# Docker Production Deploy

Patterns for building, scanning, and deploying production-ready Docker container images.

## When to Use

- Building a production Docker image from a Next.js/Node.js app
- Pushing images to container registries (GHCR, Docker Hub)
- Scanning images for vulnerabilities before deploy
- Deploying to Vercel (Docker), Fly.io, Railway, or Cloud Run
- Setting up production environment variables and secrets

## Core Principles

1. **Multi-stage builds** — separate build deps from runtime
2. **Scan before deploy** — never push unscanned images
3. **Non-root user** — production containers never run as root
4. **Minimal attack surface** — Alpine/distroless, no dev dependencies
5. **Health checks** — every production container must have one

---

## Production Dockerfile (Next.js)

```dockerfile
# Stage 1: Install dependencies
FROM node:20-alpine AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --only=production --ignore-scripts && \
    cp -R node_modules /prod_node_modules
RUN npm ci --ignore-scripts

# Stage 2: Build the application
FROM node:20-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

# Build args for public env vars (baked into client bundle)
ARG NEXT_PUBLIC_APP_URL
ARG NEXT_PUBLIC_STRIPE_KEY

ENV NEXT_TELEMETRY_DISABLED=1
RUN npm run build

# Stage 3: Production image
FROM node:20-alpine AS runner
WORKDIR /app

ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1

# Create non-root user
RUN addgroup --system --gid 1001 nodejs && \
    adduser --system --uid 1001 nextjs

# Copy only production artifacts
COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs

EXPOSE 3000
ENV PORT=3000
ENV HOSTNAME="0.0.0.0"

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:3000/api/health || exit 1

CMD ["node", "server.js"]
```

> **Important**: This requires `output: 'standalone'` in `next.config.js`:
> ```javascript
> const nextConfig = { output: 'standalone' };
> ```

## .dockerignore (Production)

```
# Git
.git
.gitignore

# Dependencies (rebuilt in container)
node_modules

# Build output (rebuilt in container)
.next
out
dist
build

# Development files
.devcontainer
docker-compose*.yml
*.test.*
*.spec.*
__tests__
coverage
.env.local
.env.development

# IDE
.vscode
.idea
*.swp
.DS_Store

# Docs
*.md
LICENSE
docs

# Secrets — NEVER include
.env
.env.*
*.pem
*.key
```

---

## Security Scanning

### Using Trivy (Recommended)

```bash
# Scan locally before push
docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  aquasec/trivy:latest image myapp:latest

# Fail on HIGH/CRITICAL vulnerabilities
docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  aquasec/trivy:latest image \
  --severity HIGH,CRITICAL \
  --exit-code 1 \
  myapp:latest
```

### In GitHub Actions

```yaml
- name: Build image
  run: docker build -t myapp:${{ github.sha }} .

- name: Scan with Trivy
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: myapp:${{ github.sha }}
    format: table
    exit-code: 1
    severity: CRITICAL,HIGH
```

---

## Container Registry Push

### GitHub Container Registry (GHCR)

```bash
# Login
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin

# Tag
docker tag myapp:latest ghcr.io/USERNAME/myapp:latest
docker tag myapp:latest ghcr.io/USERNAME/myapp:$(git rev-parse --short HEAD)

# Push
docker push ghcr.io/USERNAME/myapp:latest
docker push ghcr.io/USERNAME/myapp:$(git rev-parse --short HEAD)
```

### GitHub Actions (Automated)

```yaml
name: Build & Push

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - uses: actions/checkout@v4

      - uses: docker/setup-buildx-action@v3

      - name: Login to GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: |
            ghcr.io/${{ github.repository }}:latest
            ghcr.io/${{ github.repository }}:${{ github.sha }}
          cache-from: type=gha
          cache-to: type=gha,mode=max
```

---

## Deploy Patterns

### Option 1: Fly.io (Recommended for containers)

```toml
# fly.toml
app = "my-app"
primary_region = "sin"  # Singapore

[build]

[http_service]
  internal_port = 3000
  force_https = true
  auto_stop_machines = "stop"
  auto_start_machines = true
  min_machines_running = 1

[checks]
  [checks.health]
    port = 3000
    type = "http"
    interval = "15s"
    timeout = "2s"
    path = "/api/health"
```

```bash
# Deploy
fly deploy

# Set secrets
fly secrets set DATABASE_URL="postgresql://..." STRIPE_SECRET_KEY="sk_live_..."

# Scale
fly scale count 2
```

### Option 2: Railway

```bash
# railway.json
{
  "build": { "dockerfilePath": "Dockerfile" },
  "deploy": {
    "healthcheckPath": "/api/health",
    "healthcheckTimeout": 30,
    "restartPolicyType": "ON_FAILURE"
  }
}
```

### Option 3: Vercel (Serverless + Docker)

Vercel natively deploys Next.js without Docker. Use Docker only for non-Next.js services or custom runtimes.

```bash
# For standard Next.js — no Docker needed
vercel deploy --prod

# For custom Docker — use vercel.json
{
  "builds": [{ "src": "Dockerfile", "use": "@vercel/static-build" }]
}
```

---

## Environment Variables in Production

### Hierarchy

```
1. Platform secrets (Fly.io secrets, Railway env, Vercel env)  ← Preferred
2. CI/CD secrets (GitHub Actions secrets)
3. Docker --env-file at runtime
4. Baked into image at build time (ONLY for NEXT_PUBLIC_*)
```

### Rules

| Variable Type | Where to Set | Why |
|--------------|-------------|-----|
| `NEXT_PUBLIC_*` | Build arg (`ARG` in Dockerfile) | Baked into client JS bundle |
| `DATABASE_URL` | Platform secrets | Never in image layers |
| `STRIPE_SECRET_KEY` | Platform secrets | High security |
| `BETTER_AUTH_SECRET` | Platform secrets | Session encryption |
| `NODE_ENV` | Dockerfile `ENV` | Always `production` |

### Health Check Endpoint

Every production app needs this:

```typescript
// app/api/health/route.ts (Next.js App Router)
import { NextResponse } from 'next/server';

export async function GET() {
  try {
    // Optional: check DB connectivity
    // await db.query('SELECT 1');
    return NextResponse.json({ status: 'ok', timestamp: new Date().toISOString() });
  } catch (error) {
    return NextResponse.json({ status: 'error' }, { status: 503 });
  }
}
```

---

## Rollback Strategy

```bash
# Fly.io — rollback to previous version
fly releases
fly deploy --image ghcr.io/user/app:previous-sha

# Railway — auto-rollback on failed health check
# Configured automatically

# GHCR — keep last 5 tagged versions
# Use GitHub Actions to tag with git SHA
```

## Checklist: Production Deploy

- [ ] `output: 'standalone'` in `next.config.js`
- [ ] Multi-stage Dockerfile created
- [ ] `.dockerignore` updated (no secrets, no .git)
- [ ] Non-root user in final stage
- [ ] `HEALTHCHECK` instruction in Dockerfile
- [ ] `/api/health` endpoint in app
- [ ] Trivy scan passes (no CRITICAL/HIGH vulnerabilities)
- [ ] Image pushed to registry
- [ ] Platform secrets configured (DB, Stripe, Auth)
- [ ] `NEXT_PUBLIC_*` vars provided as build args
- [ ] DNS configured for custom domain
- [ ] SSL/TLS enabled (auto on most platforms)
- [ ] Monitoring connected (Sentry, Uptime)
