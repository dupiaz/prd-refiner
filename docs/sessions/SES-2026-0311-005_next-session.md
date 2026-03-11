# SES-2026-0311-005: CompeteRadar — Phase 4-6 Build

**Ngày lên kế hoạch**: 2026-03-11  
**Phiên trước**: `12e84c3a` — Test Workflow Phase 1-3  
**Trạng thái**: 🟡 Chờ thực hiện

---

## Bối Cảnh

Phiên trước (SES-004) đã hoàn thành **Phase 1-3** cho sản phẩm **CompeteRadar**:

| Phase | Deliverable | File |
|-------|------------|------|
| Phase 1 | Ideation & Validation | `docs/products/competeradar/phase1-ideation.md` |
| Phase 2 | PRD v1.1 (tiếng Việt) | `docs/products/competeradar/phase2-prd.md` |
| Phase 3 | UX/UI Design v1.1 | `docs/products/competeradar/phase3-ux-design.md` |
| Session Log | Toàn bộ kết quả | `docs/sessions/SES-2026-0311-004_session-log.md` |

### Quyết định đã được duyệt:
- **Sản phẩm**: CompeteRadar — AI-powered competitor intelligence cho solo founders
- **Pricing**: Free $0 / Pro $29 / Team $79 (BYOK giảm 35-38%)
- **AI Dev**: Google Gemini 3.0 (Flash cho batch + Pro cho digest)
- **AI Prod**: Hybrid — Admin mặc định Gemini + BYOK sau PMF, qua LLM Gateway
- **Chi phí AI**: ~$1.90/user/tháng (7% doanh thu)
- **Tech Stack**: Next.js 14 + Tailwind CSS + Better Auth + Supabase + Vercel
- **17 components** đã inventory, **Design Tokens** đã define (HSL colors, Inter font, 4px spacing)
- **4 wireframes** (Dashboard, Competitor Profile, Battlecard Generator, Add Competitor)
- **4 user flows** (Onboarding, Digest, Battlecard, BYOK)

---

## Prompt Gợi Ý

Chọn 1 trong 3 hướng sau:

### Hướng A: Build Phase 4-6 (Khuyến nghị ⭐)

```
Thực hiện nội dung sau
@[d:\github\prd-refiner\docs\sessions\SES-2026-0311-005_next-session.md]

Chọn Hướng A — Build Phase 4-6 cho CompeteRadar.

Tham khảo:
- PRD: @[d:\github\prd-refiner\docs\products\competeradar\phase2-prd.md]
- UX Design: @[d:\github\prd-refiner\docs\products\competeradar\phase3-ux-design.md]
- Workflow: @[d:\github\prd-refiner\.agents\workflows\webapp-from-scratch.md]
```

**Phase 4: Tech Stack & Architecture**
- 4.1 Chọn tech stack chính thức (đã duyệt ở PRD mục 8)
- 4.2 Thiết kế database schema (Supabase/PostgreSQL)
- 4.3 Thiết kế API routes (Next.js API)
- 4.4 **HITL: Review architecture**

**Phase 5: Core Setup**
- 5.1 Init Next.js project + Tailwind CSS
- 5.2 Setup Better Auth (email + Google OAuth)
- 5.3 Setup Supabase database + migrations
- 5.4 Implement Design System (tokens → CSS variables)
- 5.5 Setup project structure (pages, components, lib)
- 5.6 **HITL: Review working skeleton**

**Phase 6: Feature Development (Sprint 1-2)**
- 6.1 F1 — Competitor Setup & Dashboard
- 6.2 F2 — Change Detection Engine (Puppeteer + Cheerio)
- 6.3 **HITL: Review core features demo**

### Hướng B: Phase 4 Only (An toàn hơn)

```
Thực hiện nội dung sau
@[d:\github\prd-refiner\docs\sessions\SES-2026-0311-005_next-session.md]

Chọn Hướng B — Chỉ Phase 4 (Architecture & Database Design).

Tham khảo:
- PRD: @[d:\github\prd-refiner\docs\products\competeradar\phase2-prd.md]
- UX Design: @[d:\github\prd-refiner\docs\products\competeradar\phase3-ux-design.md]
```

Chỉ tập trung Phase 4:
- Database schema chi tiết (ERD + SQL migrations)
- API design (OpenAPI spec)
- Architecture diagram (component relationships)
- HITL review trước khi code

### Hướng C: Refine Phase 1-3 Trước Khi Build

```
Thực hiện nội dung sau
@[d:\github\prd-refiner\docs\sessions\SES-2026-0311-005_next-session.md]

Chọn Hướng C — Bổ sung Phase 1-3 trước khi build.

Tôi muốn:
1. [Bổ sung wireframe trang nào?]
2. [Thêm user flow nào?]
3. [Cập nhật PRD phần nào?]
```

---

## Lưu Ý Kỹ Thuật Cho Phase 4-6

### Database Tables dự kiến (cần thiết kế chi tiết)
```
users              — Better Auth managed
competitors        — url, name, logo, user_id
competitor_pages   — url, page_type, competitor_id
snapshots          — html_content, screenshot, page_id, created_at
changes            — diff_type, severity, old/new values, snapshot_id
digests            — ai_summary, user_id, created_at
battlecards        — competitor_id, user_id, content, share_token
alerts             — rule_type, channel, user_id
alert_logs         — alert_id, triggered_at, content
subscriptions      — plan, status, stripe_id, user_id
byok_keys          — provider, encrypted_key, user_id
```

### API Routes dự kiến
```
/api/auth/*                — Better Auth routes
/api/competitors           — CRUD đối thủ
/api/competitors/:id/scan  — Trigger manual scan
/api/digest                — List/view digests
/api/battlecards           — CRUD battlecards
/api/battlecards/:id/share — Public share link
/api/alerts                — CRUD alert rules
/api/settings/ai           — BYOK key management
/api/cron/scan             — Daily scan job
/api/cron/digest           — Weekly digest job
```

### Skills sẽ dùng
| Phase | Skills |
|-------|--------|
| Phase 4 | `api-design`, `supabase-postgres-best-practices`, `writing-plans` |
| Phase 5 | `better-auth-best-practices`, `create-auth-skill`, `vercel-react-best-practices` |
| Phase 6 | `test-driven-development`, `playwright-skill`, `systematic-debugging` |

---

*Chuẩn bị cho phiên tiếp theo — Tạo: 2026-03-11*
