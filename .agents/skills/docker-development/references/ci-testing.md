# CI Testing Patterns for Docker Images

Comprehensive patterns for testing Docker images in CI/CD pipelines.

## The Challenge

Docker images with entrypoints, upstream dependencies, and compose configs need specific testing strategies to work reliably in CI.

## Pattern 1: Entrypoint Bypass

**Problem**: Images with ENTRYPOINT won't accept arbitrary commands.

**Solution**: Override entrypoint to run test commands directly.

```bash
# Test PHP version in an image with a startup entrypoint
docker run --rm --entrypoint php myimage -v

# Test binary exists
docker run --rm --entrypoint which myimage nginx

# Run shell commands
docker run --rm --entrypoint sh myimage -c "cat /etc/os-release"
```

**When to Use**: Any image where ENTRYPOINT prevents direct command execution.

## Pattern 2: DNS Mocking for Upstream Services

**Problem**: nginx/haproxy configs reference upstream servers that don't exist in CI.

**Solution**: Use `--add-host` to point upstream names to localhost.

```bash
docker run --rm --add-host backend:127.0.0.1 nginx-image nginx -t
```

**Multiple Upstreams**:
```bash
docker run --rm \
  --add-host api:127.0.0.1 \
  --add-host database:127.0.0.1 \
  --add-host cache:127.0.0.1 \
  myimage nginx -t
```

## Pattern 3: Docker Compose Validation

**Problem**: `docker compose config` fails when required env vars are missing.

**Solution**: Create `.env` from `.env.example` with dummy values.

```bash
cp .env.example .env
sed -i 's/CHANGE_ME_[A-Z_]*/ci_test_value/g' .env
docker compose config > /dev/null
```

**Alternative: Inline Variables**:
```bash
# Set all required vars inline
REQUIRED_VAR1=test REQUIRED_VAR2=test docker compose config > /dev/null
```

## Pattern 4: Health Check Verification

```bash
# Test Health Check Command
docker run --rm --entrypoint sh myimage -c \
  "$(docker inspect --format='{{.Config.Healthcheck.Test}}' myimage | tr -d '[]')"
```

## Pattern 5: Secret Scanning with Exclusions

**Problem**: Secret scanners flag example/docs files as security issues.

**Solution**: Configure exclusions for known-safe files.

```yaml
# .trivyignore or scanner config
.env.example
README.md
docs/
```

## Pattern 6: Multi-Platform Build Testing

```yaml
- name: Set up QEMU
  uses: docker/setup-qemu-action@v3

- name: Set up Buildx
  uses: docker/setup-buildx-action@v3

- name: Build multi-platform
  uses: docker/build-push-action@v6
  with:
    platforms: linux/amd64,linux/arm64
    load: false
    cache-from: type=gha
    cache-to: type=gha,mode=max
```

## Pattern 7: Integration Testing with Service Containers

```yaml
jobs:
  test:
    services:
      database:
        image: mariadb:11
        env:
          MYSQL_ROOT_PASSWORD: test
          MYSQL_DATABASE: testdb
        options: >-
          --health-cmd="healthcheck.sh --connect --innodb_initialized"
          --health-interval=10s
          --health-timeout=5s
          --health-retries=5

    steps:
      - name: Build app image
        run: docker build -t myapp:test .

      - name: Run integration tests
        run: |
          docker run --rm \
            --network ${{ job.container.network }} \
            -e DB_HOST=database \
            -e DB_PASSWORD=test \
            myapp:test npm test
```

## Complete CI Workflow Example

```yaml
name: Docker CI

on: [push, pull_request]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup test environment
        run: |
          cp .env.example .env
          sed -i 's/CHANGE_ME_[A-Z_]*/ci_test_value/g' .env

      - name: Validate compose
        run: docker compose config > /dev/null

  build-and-test:
    runs-on: ubuntu-latest
    needs: validate
    steps:
      - uses: actions/checkout@v4

      - uses: docker/setup-buildx-action@v3

      - name: Build images
        run: docker compose build

      - name: Test PHP version
        run: docker run --rm --entrypoint php myapp:latest -v | grep "8.4"

      - name: Test nginx config
        run: docker run --rm --add-host app:127.0.0.1 myapp-nginx nginx -t

      - name: Start stack
        run: |
          cp .env.example .env
          sed -i 's/CHANGE_ME_[A-Z_]*/ci_test/g' .env
          docker compose up -d

      - name: Wait for healthy
        run: |
          timeout 120 bash -c 'until docker compose ps | grep -q healthy; do sleep 5; done'

      - name: Test endpoint
        run: curl -f http://localhost/ || exit 1

      - name: Cleanup
        if: always()
        run: docker compose down -v
```
