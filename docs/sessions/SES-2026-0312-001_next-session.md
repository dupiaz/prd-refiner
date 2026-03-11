# SES-2026-0312-001: CompeteRadar — Phase 4.7 HITL Review & Phase 5 Core Setup

**Ngày lên kế hoạch**: 2026-03-12  
**Phiên trước**: `cfd04f4a` — Phase 4.6 Docker Setup & Monorepo  
**Trạng thái**: 🟡 Chờ thực hiện

---

## Bối Cảnh

Phase 4 đã hoàn thành **tất cả steps** (4.1 → 4.6):

| Step | Deliverable | Trạng thái |
|------|------------|-----------|
| 4.1 | Tech Stack — 14 technologies | ✅ |
| 4.2 | Database — 11 tables, 13 indexes, 12 RLS | ✅ |
| 4.3 | API Design — ~30 routes, RFC 7807 | ✅ |
| 4.4 | Structure Decision → Vertical Slice | ✅ |
| 4.5 | Methodology doc (User Story → API → DB) | ✅ |
| 4.6 | Docker Setup + Monorepo consolidation | ✅ |
| **4.7** | **HITL Review toàn bộ Phase 4** | 🟡 **Chờ** |

### Monorepo Structure (mới)
```
products/competeradar/
├── docs/                    ← 8 design docs
├── docker-compose.yml       ← PG 16 + pgAdmin
├── Dockerfile               ← Multi-stage production
├── .env.example             ← 16+ env vars
└── .dockerignore            ← Build context exclusions
```

---

## Prompt Gợi Ý

### Hướng A: HITL Review Phase 4 → Phase 5 (Khuyến nghị ⭐)

```
Hãy đọc nội dung sau và bắt đầu phiên làm việc
@[d:\github\prd-refiner\docs\sessions\SES-2026-0312-001_next-session.md]

Chọn Hướng A:
1. Review lần cuối Phase 4 (step 4.7)
2. Bắt đầu Phase 5 — Core Setup

Tham khảo:
- Architecture: @[d:\github\prd-refiner\products\competeradar\docs\phase4-architecture.md]
- Vertical Structure: @[d:\github\prd-refiner\products\competeradar\docs\phase4-structure-vertical.md]
- Docker: @[d:\github\prd-refiner\products\competeradar\docker-compose.yml]
- Workflow: @[d:\github\prd-refiner\.agents\workflows\webapp-from-scratch.md]
```

### Hướng B: Thẳng Phase 5 (bỏ qua HITL review)

```
Hãy đọc nội dung sau và bắt đầu phiên làm việc
@[d:\github\prd-refiner\docs\sessions\SES-2026-0312-001_next-session.md]

Chọn Hướng B — Bắt đầu Phase 5 Core Setup:
5.1 Scaffold Next.js trong products/competeradar/
5.2 Setup Better Auth
5.3 Connect Supabase + Drizzle migrations

Tham khảo:
- Architecture: @[d:\github\prd-refiner\products\competeradar\docs\phase4-architecture.md]
- Workflow: @[d:\github\prd-refiner\.agents\workflows\webapp-from-scratch.md]
```

### Hướng C: Git sync trước

```
Hãy đọc nội dung sau và bắt đầu phiên làm việc
@[d:\github\prd-refiner\docs\sessions\SES-2026-0312-001_next-session.md]

Chọn Hướng C — Sync tất cả thay đổi lên GitHub trước khi Phase 5.
```

---

## Phase 5 Quick Reference

| Step | Task | Skills | Output |
|------|------|--------|--------|
| 5.1 | Scaffold Next.js trong `products/competeradar/` | `vercel-react-best-practices` | Project structure |
| 5.2 | Better Auth (email + Google OAuth) | `better-auth-best-practices`, `create-auth-skill` | Auth routes + UI |
| 5.3 | Supabase + Drizzle migrations | `supabase-postgres-best-practices` | DB connection + schema |
| 5.4 | CI/CD (GitHub Actions) | `github-actions` | `.github/workflows/` |
| 5.5 | **HITL: Test login/signup flow** | `playwright-skill` | E2E test evidence |

### Dependencies (Phase 5)
```json
{
  "next": "^14", "react": "^18",
  "better-auth": "latest", "@supabase/supabase-js": "latest",
  "drizzle-orm": "latest", "zod": "^3", "tailwindcss": "^3"
}
```

---

## Lưu Ý

> ⚠️ **File paths đã đổi**: Tất cả docs CompeteRadar giờ nằm trong `products/competeradar/docs/` (không còn ở `docs/products/competeradar/`).

> 💡 **Workflow updated**: `webapp-from-scratch.md` đã có section `Conventions — Monorepo Structure`.

---

*Chuẩn bị cho phiên tiếp theo — 2026-03-12*
