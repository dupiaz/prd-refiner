---
description: Workflow xây dựng web app từ ý tưởng thô đến production — 12 giai đoạn, đầy đủ skill mapping và Auto/HITL markers.
---

# 🚀 Web App From Scratch — Master Workflow

Quy trình 12 giai đoạn để xây dựng web app hoàn chỉnh từ ý tưởng đến scale.
Mỗi bước đánh dấu:
- 🤖 **Auto** — AI agent thực hiện tự động
- 👤 **HITL** — Cần Human-In-The-Loop review/quyết định

---

## Phase 1: Ideation & Validation (Ý tưởng & Xác thực)

| # | Bước | Mode | Skill(s) |
|---|------|------|----------|
| 1.1 | Brainstorm ý tưởng, phân tích pain points | 🤖 Auto | `brainstorming` |
| 1.2 | Nghiên cứu thị trường & đối thủ | 🤖 Auto | `competitive-landscape`, `competitor-analysis` |
| 1.3 | Xây dựng buyer persona | 🤖 Auto | `audience-intelligence`, `audience-profile` |
| 1.4 | **Validate ý tưởng: chọn hướng đi** | 👤 HITL | `focus-group`, `message-test` |
| 1.5 | Phân tích startup metrics framework | 🤖 Auto | `startup-metrics-framework` |

**Quality Gate**: Ý tưởng được validate, có target audience rõ ràng.

---

## Phase 2: Product Planning (Lên kế hoạch sản phẩm)

| # | Bước | Mode | Skill(s) |
|---|------|------|----------|
| 2.1 | Viết PRD (Product Requirements Document) | 🤖 Auto | `product-manager-toolkit`, `writing-plans` |
| 2.2 | Thiết kế pricing strategy | 🤖 Auto | `pricing-strategy` |
| 2.3 | Lên launch strategy | 🤖 Auto | `launch-strategy`, `launch-plan` |
| 2.4 | **Review & approve PRD** | 👤 HITL | — |
| 2.5 | Lên content calendar cho launch | 🤖 Auto | `content-calendar`, `content-brief` |

**Quality Gate**: PRD approved, pricing model xác định, launch timeline có.

---

## Phase 3: UX/UI Design (Thiết kế giao diện)

| # | Bước | Mode | Skill(s) |
|---|------|------|----------|
| 3.1 | Xây dựng Information Architecture | 🤖 Auto | `ux-wireframe` |
| 3.2 | Vẽ User Flows cho core features | 🤖 Auto | `ux-wireframe` |
| 3.3 | Tạo Wireframes (low → mid fidelity) | 🤖 Auto | `ux-wireframe` |
| 3.4 | Thiết kế Component Inventory & Design Tokens | 🤖 Auto | `ux-wireframe` |
| 3.5 | **Review wireframes & UX flows** | 👤 HITL | — |
| 3.6 | Thiết kế responsive layout patterns | 🤖 Auto | `responsive-design` |
| 3.7 | Audit accessibility compliance | 🤖 Auto | `accessibility-audit` |
| 3.8 | Review theo Web Design Guidelines | 🤖 Auto | `web-design-guidelines` |

**Quality Gate**: Wireframes approved, responsive strategy xác định, a11y checklist passed.

---

## Phase 4: Tech Stack & Architecture (Kiến trúc kỹ thuật)

| # | Bước | Mode | Skill(s) |
|---|------|------|----------|
| 4.1 | Chọn tech stack (framework, DB, hosting) | 🤖 Auto | `spec-driven-development` |
| 4.2 | Thiết kế API architecture | 🤖 Auto | `api-design` |
| 4.3 | Thiết kế database schema | 🤖 Auto | `supabase-postgres-best-practices`, `neon-postgres` |
| 4.4 | Setup Docker development environment | 🤖 Auto | `docker-development`, `docker-devcontainer-windows` |
| 4.5 | **Review & approve architecture** | 👤 HITL | — |

**Quality Gate**: Tech stack locked, API spec drafted, DB schema designed.

---

## Phase 5: Authentication & Core Setup (Xác thực & Setup cơ bản)

| # | Bước | Mode | Skill(s) |
|---|------|------|----------|
| 5.1 | Scaffold project (Next.js / Vite) | 🤖 Auto | `vercel-react-best-practices` |
| 5.2 | Setup authentication (Better Auth) | 🤖 Auto | `better-auth-create-auth`, `better-auth-best-practices` |
| 5.3 | Connect database (Supabase/Neon) | 🤖 Auto | `supabase-postgres-best-practices`, `neon-postgres` |
| 5.4 | Setup CI/CD pipeline | 🤖 Auto | `github-actions` |
| 5.5 | **Test login/signup flow end-to-end** | 👤 HITL | `playwright-testing` |

**Quality Gate**: Auth working, DB connected, CI pipeline green.

---

## Phase 6: Feature Development (Phát triển tính năng)

| # | Bước | Mode | Skill(s) |
|---|------|------|----------|
| 6.1 | Phân tách tasks từ PRD | 🤖 Auto | `writing-plans`, `spec-driven-development` |
| 6.2 | Implement features theo TDD | 🤖 Auto | `test-driven-development`, `systematic-debugging` |
| 6.3 | Build UI components (responsive + a11y) | 🤖 Auto | `responsive-design`, `accessibility-audit`, `vercel-composition-patterns` |
| 6.4 | Build API endpoints | 🤖 Auto | `api-design` |
| 6.5 | Code review | 🤖 Auto | `sentry-code-review`, `sentry-find-bugs` |
| 6.6 | **Feature review & QA** | 👤 HITL | `verification-before-completion` |

**Quality Gate**: Core features working, tests passing, code reviewed.

---

## Phase 7: Payment Integration (Tích hợp thanh toán)

| # | Bước | Mode | Skill(s) |
|---|------|------|----------|
| 7.1 | Integrate Stripe (checkout, subscriptions) | 🤖 Auto | `stripe-integration`, `stripe-best-practices` |
| 7.2 | Setup webhooks | 🤖 Auto | `stripe-integration` |
| 7.3 | **Test payment flow end-to-end** | 👤 HITL | `playwright-testing` |

**Quality Gate**: Payment flow working in test mode, webhooks verified.

---

## Phase 8: Security & Testing (Bảo mật & Kiểm thử)

| # | Bước | Mode | Skill(s) |
|---|------|------|----------|
| 8.1 | Security audit | 🤖 Auto | `insecure-defaults`, `sharp-edges`, `static-analysis` |
| 8.2 | Differential security review | 🤖 Auto | `differential-review` |
| 8.3 | E2E testing suite | 🤖 Auto | `playwright-testing` |
| 8.4 | Accessibility audit | 🤖 Auto | `accessibility-audit` |
| 8.5 | Performance audit | 🤖 Auto | `web-design-guidelines` |
| 8.6 | **Review security findings & fix** | 👤 HITL | — |

**Quality Gate**: No critical vulnerabilities, all tests green, a11y audit passed.

---

## Phase 9: Pre-Launch & SEO (Chuẩn bị launch)

| # | Bước | Mode | Skill(s) |
|---|------|------|----------|
| 9.1 | SEO audit & optimization | 🤖 Auto | `seo-audit`, `tech-seo-audit` |
| 9.2 | Setup structured data (schema.org) | 🤖 Auto | `seo-implement` |
| 9.3 | Tạo landing page copy | 🤖 Auto | `copywriting`, `content-creator` |
| 9.4 | Setup analytics (GA4, tracking) | 🤖 Auto | `analytics-insights` |
| 9.5 | Docker production build | 🤖 Auto | `docker-production-deploy` |
| 9.6 | **Deploy to staging & final review** | 👤 HITL | `deploy-to-vercel` |

**Quality Gate**: SEO optimized, analytics tracking verified, staging deployment working.

---

## Phase 10: Launch & GTM (Launch & Go-To-Market)

| # | Bước | Mode | Skill(s) |
|---|------|------|----------|
| 10.1 | Deploy to production | 🤖 Auto | `deploy-to-vercel`, `docker-production-deploy` |
| 10.2 | Tạo campaign plan | 🤖 Auto | `campaign-plan`, `campaign-orchestrator` |
| 10.3 | Viết ad creatives | 🤖 Auto | `ad-creative`, `copywriting` |
| 10.4 | Launch social media campaign | 🤖 Auto | `social-strategy`, `schedule-social` |
| 10.5 | Setup email sequences | 🤖 Auto | `email-sequence`, `send-email-campaign` |
| 10.6 | **Monitor launch metrics** | 👤 HITL | `performance-check`, `anomaly-scan` |

**Quality Gate**: App live, campaigns running, monitoring active.

---

## Phase 11: Growth & Retention (Tăng trưởng)

| # | Bước | Mode | Skill(s) |
|---|------|------|----------|
| 11.1 | Phân tích user behavior & funnels | 🤖 Auto | `funnel-audit`, `cohort-analysis` |
| 11.2 | A/B testing plan | 🤖 Auto | `ab-test-plan`, `cro` |
| 11.3 | Setup retargeting | 🤖 Auto | `retargeting-strategy` |
| 11.4 | Optimize conversion rate | 🤖 Auto | `cro`, `landing-page-audit` |
| 11.5 | Growth loops & referral programs | 🤖 Auto | `growth-engineering`, `loop-detect` |
| 11.6 | **Review growth metrics & pivot decisions** | 👤 HITL | `performance-report`, `roi-calculator` |

**Quality Gate**: Growth metrics trending up, retention improving.

---

## Phase 12: Scale & Operations (Mở rộng & Vận hành)

| # | Bước | Mode | Skill(s) |
|---|------|------|----------|
| 12.1 | Performance optimization | 🤖 Auto | `web-design-guidelines` |
| 12.2 | Budget optimization | 🤖 Auto | `budget-optimizer`, `budget-tracker` |
| 12.3 | Competitive monitoring | 🤖 Auto | `competitor-monitor`, `competitor-alerts` |
| 12.4 | Content scaling & repurposing | 🤖 Auto | `content-repurpose`, `content-engine` |
| 12.5 | AEO/GEO optimization (AI visibility) | 🤖 Auto | `aeo-geo`, `geo-monitor` |
| 12.6 | **Quarterly business review** | 👤 HITL | `exec-summary`, `qbr-plan` |

**Quality Gate**: Scaled operations, sustainable growth.

---

## Tổng Kết

| Metric | Giá trị |
|--------|---------|
| **Tổng phases** | 12 |
| **Tổng bước** | 67 |
| **🤖 Auto steps** | 53 (79%) |
| **👤 HITL checkpoints** | 14 (21%) |
| **Skills sử dụng** | ~70 skills |

### HITL Checkpoints (14)

1. Phase 1.4 — Validate ý tưởng
2. Phase 2.4 — Review PRD
3. Phase 3.5 — Review wireframes
4. Phase 4.5 — Review architecture
5. Phase 5.5 — Test auth flow
6. Phase 6.6 — Feature QA
7. Phase 7.3 — Test payment
8. Phase 8.6 — Review security
9. Phase 9.6 — Staging review
10. Phase 10.6 — Monitor launch
11. Phase 11.6 — Growth review
12. Phase 12.6 — QBR
13. (Implicit) Any step can escalate to HITL nếu AI không chắc chắn
14. (Implicit) User có thể interrupt bất kỳ lúc nào

### Cách invoke

```
/webapp-from-scratch
```

Workflow sẽ bắt đầu từ Phase 1. Để nhảy đến phase cụ thể:

```
Bắt đầu từ Phase 3: UX/UI Design
```
