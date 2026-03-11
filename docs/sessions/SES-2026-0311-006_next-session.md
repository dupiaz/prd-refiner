# SES-2026-0311-006: CompeteRadar — Phase 4 HITL Review & Phase 5 Core Setup

**Ngày lên kế hoạch**: 2026-03-11  
**Phiên trước**: `2f726c97` — Phase 4 Architecture & Design  
**Trạng thái**: 🟡 Chờ thực hiện

---

## Bối Cảnh

Phiên trước (SES-005) đã hoàn thành **Phase 4** cho CompeteRadar:

| Deliverable | Trạng thái |
|-------------|-----------|
| Architecture doc (v1.2) — tech stack, DB, API | ✅ Xong, chờ HITL review (step 4.7) |
| Methodology doc — User Story → API → DB | ✅ Xong |
| 2 phiên bản cấu trúc thư mục + decision doc | ✅ Xong, **Vertical Slice đã chọn** (step 4.4) |
| Workflow `webapp-from-scratch.md` cập nhật | ✅ Xong (69 bước, 15 HITL) |

### Quyết định đã duyệt:
- **Kiến trúc thư mục**: Vertical Slice — `features/` + `shared/` + `app/` (thin routing)
- **Database**: 11 tables, 13 indexes, 12 RLS policies
- **API**: ~30 routes, RFC 7807, Zod validation

---

## Prompt Gợi Ý

Chọn 1 trong 3 hướng sau:

### Hướng A: HITL Review Phase 4 → Phase 5 Setup (Khuyến nghị ⭐)

```
Hãy đọc nội dung này để bắt đầu phiên làm việc
@[d:\github\prd-refiner\docs\sessions\SES-2026-0311-006_next-session.md]

Chọn Hướng A:
1. Review lần cuối Phase 4 (step 4.7)
2. Bắt đầu Phase 5 — Core Setup

Tham khảo:
- Architecture: @[d:\github\prd-refiner\docs\products\competeradar\phase4-architecture.md]
- Vertical Structure: @[d:\github\prd-refiner\docs\products\competeradar\phase4-structure-vertical.md]
- Workflow: @[d:\github\prd-refiner\.agents\workflows\webapp-from-scratch.md]
```

**Phase 4.7: Final HITL Review**
- Review toàn bộ 5 tài liệu Phase 4
- Approve → tiến vào coding

**Phase 5: Authentication & Core Setup**
- 5.1 Scaffold Next.js project theo Vertical Slice structure
- 5.2 Setup Better Auth (email + Google OAuth)
- 5.3 Connect Supabase — run SQL migrations
- 5.4 Setup CI/CD (GitHub Actions)
- 5.5 **HITL: Test login/signup flow**

### Hướng B: Chỉ Review Phase 4 (An toàn hơn)

```
Hãy đọc nội dung này để bắt đầu phiên làm việc
@[d:\github\prd-refiner\docs\sessions\SES-2026-0311-006_next-session.md]

Chọn Hướng B — Chỉ HITL review Phase 4.

Tôi muốn review kỹ:
- @[d:\github\prd-refiner\docs\products\competeradar\phase4-architecture.md]
- @[d:\github\prd-refiner\docs\products\competeradar\phase4-structure-vertical.md]
- @[d:\github\prd-refiner\docs\products\competeradar\phase4-architecture-decision.md]
```

### Hướng C: Phase 4 Docker Setup (Bổ sung)

```
Hãy đọc nội dung này để bắt đầu phiên làm việc
@[d:\github\prd-refiner\docs\sessions\SES-2026-0311-006_next-session.md]

Chọn Hướng C — Phase 4.6 Docker Setup trước khi Phase 5.

Tham khảo:
- Architecture: @[d:\github\prd-refiner\docs\products\competeradar\phase4-architecture.md]
- Docker skills đã cài: docker-development, docker-devcontainer-windows, docker-production-deploy
```

---

## Phase 5 — Chi Tiết Kỹ Thuật

### Scaffold theo Vertical Slice

```
competeradar/
├── src/
│   ├── app/           ← Next.js routing (thin)
│   ├── features/      ← 8 feature slices
│   │   ├── competitors/
│   │   ├── scanning/
│   │   ├── digest/
│   │   ├── battlecards/
│   │   ├── alerts/
│   │   ├── settings/
│   │   ├── billing/
│   │   └── dashboard/
│   └── shared/        ← UI, DB, auth, AI, utils
```

### Skills cần dùng

| Step | Skills |
|------|--------|
| 5.1 Scaffold | `vercel-react-best-practices` |
| 5.2 Auth | `better-auth-best-practices`, `create-auth-skill` |
| 5.3 DB | `supabase-postgres-best-practices` |
| 5.4 CI/CD | `github-actions` |
| 5.5 Test | `playwright-skill` |

### Dependencies (package.json)

```json
{
  "dependencies": {
    "next": "^14",
    "react": "^18",
    "better-auth": "latest",
    "@supabase/supabase-js": "latest",
    "drizzle-orm": "latest",
    "zod": "^3",
    "tailwindcss": "^3"
  },
  "devDependencies": {
    "drizzle-kit": "latest",
    "typescript": "^5",
    "@types/react": "^18",
    "eslint": "^9",
    "prettier": "latest"
  }
}
```

---

## Lưu Ý

> ⚠️ **Docker step (4.6)** chưa thực hiện — có thể làm ở đầu phiên sau hoặc song song Phase 5.

> 💡 **PowerShell encoding**: Tránh dùng `Set-Content` cho file UTF-8 tiếng Việt. Dùng `write_to_file` tool trực tiếp.

---

*Chuẩn bị cho phiên tiếp theo — Tạo: 2026-03-11*
