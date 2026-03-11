---
name: github-actions
description: CI/CD pipeline templates cho GitHub Actions — lint, test, build, scan, deploy. Dùng khi cần setup hoặc tối ưu CI/CD workflows.
---

# GitHub Actions — CI/CD Pipeline Templates

Skill hướng dẫn tạo CI/CD pipelines bằng GitHub Actions, bao gồm multi-stage workflows, caching, matrix builds, secret management, và deployment patterns.

**Source**: Custom skill — tham khảo `kodustech/awesome-agent-skills` (DevOps section).

## Khi Nào Dùng

- Setup CI/CD cho dự án mới
- Tối ưu build time cho pipeline hiện có
- Thêm security scanning vào pipeline
- Setup deployment automation (Vercel, Docker, npm)
- Cần matrix builds cho multi-version testing
- Tạo reusable workflows

## Pipeline Chuẩn: 5 Stages

```
Lint → Test → Build → Scan → Deploy
```

### Full Pipeline Template

```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

env:
  NODE_VERSION: '20'
  PNPM_VERSION: '9'

jobs:
  # ═══════════════════════════════════
  # Stage 1: Lint & Format Check
  # ═══════════════════════════════════
  lint:
    name: 🔍 Lint & Format
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: pnpm/action-setup@v4
        with:
          version: ${{ env.PNPM_VERSION }}

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'pnpm'

      - run: pnpm install --frozen-lockfile
      - run: pnpm run lint
      - run: pnpm run format:check
      - run: pnpm run typecheck

  # ═══════════════════════════════════
  # Stage 2: Test
  # ═══════════════════════════════════
  test:
    name: 🧪 Test
    runs-on: ubuntu-latest
    needs: lint
    strategy:
      matrix:
        node-version: [18, 20, 22]
    steps:
      - uses: actions/checkout@v4

      - uses: pnpm/action-setup@v4
        with:
          version: ${{ env.PNPM_VERSION }}

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'pnpm'

      - run: pnpm install --frozen-lockfile
      - run: pnpm test -- --coverage

      - name: Upload coverage
        if: matrix.node-version == 20
        uses: actions/upload-artifact@v4
        with:
          name: coverage-report
          path: coverage/

  # ═══════════════════════════════════
  # Stage 3: Build
  # ═══════════════════════════════════
  build:
    name: 🏗️ Build
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4

      - uses: pnpm/action-setup@v4
        with:
          version: ${{ env.PNPM_VERSION }}

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'pnpm'

      - run: pnpm install --frozen-lockfile
      - run: pnpm build

      - name: Upload build artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/
          retention-days: 7

  # ═══════════════════════════════════
  # Stage 4: Security Scan
  # ═══════════════════════════════════
  security:
    name: 🔒 Security Scan
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/checkout@v4

      - name: Run Trivy vulnerability scanner
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          scan-ref: '.'
          format: 'sarif'
          output: 'trivy-results.sarif'
          severity: 'CRITICAL,HIGH'

      - name: Upload Trivy scan results
        uses: github/codeql-action/upload-sarif@v3
        if: always()
        with:
          sarif_file: 'trivy-results.sarif'

      - name: Audit dependencies
        run: pnpm audit --audit-level=high

  # ═══════════════════════════════════
  # Stage 5: Deploy
  # ═══════════════════════════════════
  deploy-preview:
    name: 🚀 Deploy Preview
    runs-on: ubuntu-latest
    needs: [build, security]
    if: github.event_name == 'pull_request'
    environment: preview
    steps:
      - uses: actions/checkout@v4
      - uses: actions/download-artifact@v4
        with:
          name: build-output
          path: dist/

      - name: Deploy to Vercel (Preview)
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          working-directory: ./

  deploy-production:
    name: 🚀 Deploy Production
    runs-on: ubuntu-latest
    needs: [build, security]
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'
    environment: production
    steps:
      - uses: actions/checkout@v4
      - uses: actions/download-artifact@v4
        with:
          name: build-output
          path: dist/

      - name: Deploy to Vercel (Production)
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'
          working-directory: ./
```

## Workflow Patterns Phổ Biến

### Pattern 1: Reusable Workflow

```yaml
# .github/workflows/reusable-test.yml
name: Reusable Test Workflow

on:
  workflow_call:
    inputs:
      node-version:
        required: false
        type: string
        default: '20'
    secrets:
      NPM_TOKEN:
        required: false

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node-version }}
      - run: npm ci
      - run: npm test
```

```yaml
# Gọi reusable workflow
jobs:
  test:
    uses: ./.github/workflows/reusable-test.yml
    with:
      node-version: '20'
    secrets: inherit
```

### Pattern 2: Docker Build & Push

```yaml
  docker:
    name: 🐳 Docker Build & Push
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - uses: actions/checkout@v4

      - uses: docker/setup-buildx-action@v3

      - uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            ghcr.io/${{ github.repository }}:${{ github.sha }}
            ghcr.io/${{ github.repository }}:latest
          cache-from: type=gha
          cache-to: type=gha,mode=max
```

### Pattern 3: Release & npm Publish

```yaml
name: Release

on:
  push:
    tags: ['v*']

jobs:
  release:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      id-token: write
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          registry-url: 'https://registry.npmjs.org'

      - run: npm ci
      - run: npm run build
      - run: npm publish --provenance --access public
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}

      - name: Create GitHub Release
        uses: softprops/action-gh-release@v2
        with:
          generate_release_notes: true
```

### Pattern 4: Scheduled Jobs

```yaml
name: Scheduled Tasks

on:
  schedule:
    - cron: '0 6 * * 1'  # Mỗi thứ 2 lúc 6:00 UTC

jobs:
  dependency-update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npx npm-check-updates -u
      - run: npm install
      - run: npm test
      - uses: peter-evans/create-pull-request@v6
        with:
          title: 'chore: update dependencies'
          branch: 'deps/weekly-update'
```

## Caching Strategies

```yaml
# pnpm cache (khuyến nghị — nhanh hơn npm)
- uses: pnpm/action-setup@v4
  with:
    version: 9
- uses: actions/setup-node@v4
  with:
    node-version: '20'
    cache: 'pnpm'

# Docker layer cache
- uses: docker/build-push-action@v5
  with:
    cache-from: type=gha
    cache-to: type=gha,mode=max

# Custom cache
- uses: actions/cache@v4
  with:
    path: |
      ~/.cache/puppeteer
      node_modules/.cache
    key: ${{ runner.os }}-custom-${{ hashFiles('**/pnpm-lock.yaml') }}
```

## Secret Management

```yaml
# ✅ Dùng environment secrets
jobs:
  deploy:
    environment: production
    steps:
      - run: echo "Token: ${{ secrets.API_TOKEN }}"

# ✅ OIDC cho cloud providers (không cần long-lived secrets)
permissions:
  id-token: write
  contents: read
steps:
  - uses: aws-actions/configure-aws-credentials@v4
    with:
      role-to-assume: arn:aws:iam::123456789:role/GitHubActions
      aws-region: ap-southeast-1
```

**Quy tắc bảo mật:**
- KHÔNG hardcode secrets trong workflow files
- Dùng environment protection rules cho production
- Dùng OIDC thay vì static credentials khi có thể
- Review workflow permissions (least privilege)
- Pin actions tới commit SHA thay vì tag cho security

## Branch Protection Rules

```
main branch:
  ✅ Require pull request reviews (1+ approvals)
  ✅ Require status checks to pass:
     - lint
     - test
     - build
     - security
  ✅ Require branches to be up to date
  ✅ Require signed commits (optional, khuyến nghị)
  ✅ Do not allow bypassing settings
```

## CI/CD Setup Checklist

- [ ] CI workflow chạy trên PR + push to main
- [ ] Lint stage: ESLint, Prettier, TypeScript check
- [ ] Test stage: unit + integration tests với coverage
- [ ] Build stage: production build thành công
- [ ] Security stage: dependency audit + vulnerability scan
- [ ] Deploy stage: preview cho PR, production cho main
- [ ] Caching configured (pnpm/Docker layers)
- [ ] Concurrency: cancel outdated runs
- [ ] Branch protection rules enabled
- [ ] Secrets stored trong repository/environment settings
- [ ] Notifications: Slack/email cho build failures
