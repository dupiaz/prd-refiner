# CompeteRadar — Phase 4: Project Structure — Horizontal (Layer-based)

**Phiên bản**: 1.0  
**Ngày**: 2026-03-11  
**Kiến trúc**: Horizontal / Layer-based  
**Tham chiếu**: Dùng chung tech stack, DB schema, API design từ `phase4-architecture.md`  
**Skills sử dụng**: `vercel-react-best-practices`

---

## Triết Lý

Tổ chức code theo **tầng kỹ thuật** (technical layer). Mỗi thư mục đại diện cho 1 vai trò:

```
Tầng nào? → Folder nào
──────────────────────
Routing/Pages   → app/
UI Components   → components/
Business Logic  → lib/
Styles          → styles/
```

---

## Cấu Trúc Thư Mục

```
competeradar/
├── src/
│   ├── app/                              # 📄 ROUTING LAYER
│   │   ├── (auth)/                       # Group: Auth pages
│   │   │   ├── login/page.tsx
│   │   │   ├── signup/page.tsx
│   │   │   └── layout.tsx
│   │   ├── (dashboard)/                  # Group: Authenticated pages
│   │   │   ├── dashboard/page.tsx
│   │   │   ├── competitors/
│   │   │   │   ├── page.tsx              # List competitors
│   │   │   │   └── [id]/page.tsx         # Competitor detail
│   │   │   ├── digest/
│   │   │   │   ├── page.tsx
│   │   │   │   └── [id]/page.tsx
│   │   │   ├── battlecards/
│   │   │   │   ├── page.tsx
│   │   │   │   ├── new/page.tsx
│   │   │   │   └── [id]/page.tsx
│   │   │   ├── alerts/page.tsx
│   │   │   ├── settings/
│   │   │   │   ├── page.tsx
│   │   │   │   ├── ai/page.tsx
│   │   │   │   ├── billing/page.tsx
│   │   │   │   └── integrations/page.tsx
│   │   │   └── layout.tsx                # Dashboard shell
│   │   ├── b/[token]/page.tsx            # Public battlecard
│   │   ├── api/                          # API routes
│   │   │   ├── auth/[...all]/route.ts
│   │   │   ├── v1/
│   │   │   │   ├── competitors/
│   │   │   │   │   ├── route.ts          # GET list, POST create
│   │   │   │   │   └── [id]/
│   │   │   │   │       ├── route.ts      # GET, PATCH, DELETE
│   │   │   │   │       ├── scan/route.ts
│   │   │   │   │       ├── changes/route.ts
│   │   │   │   │       └── pages/route.ts
│   │   │   │   ├── digests/
│   │   │   │   │   ├── route.ts
│   │   │   │   │   ├── [id]/route.ts
│   │   │   │   │   ├── [id]/feedback/route.ts
│   │   │   │   │   └── settings/route.ts
│   │   │   │   ├── battlecards/
│   │   │   │   │   ├── route.ts
│   │   │   │   │   ├── [id]/route.ts
│   │   │   │   │   ├── [id]/share/route.ts
│   │   │   │   │   └── shared/[token]/route.ts
│   │   │   │   ├── alerts/
│   │   │   │   │   ├── route.ts
│   │   │   │   │   ├── [id]/route.ts
│   │   │   │   │   └── logs/route.ts
│   │   │   │   └── settings/
│   │   │   │       ├── ai/
│   │   │   │       │   ├── route.ts
│   │   │   │       │   └── byok/route.ts
│   │   │   │       ├── profile/route.ts
│   │   │   │       ├── billing/
│   │   │   │       │   ├── route.ts
│   │   │   │       │   ├── checkout/route.ts
│   │   │   │       │   └── portal/route.ts
│   │   │   │       └── integrations/route.ts
│   │   │   ├── cron/
│   │   │   │   ├── scan/route.ts
│   │   │   │   └── digest/route.ts
│   │   │   └── webhooks/
│   │   │       └── stripe/route.ts
│   │   ├── layout.tsx                    # Root layout
│   │   └── page.tsx                      # Landing page
│   │
│   ├── components/                       # 🧩 UI COMPONENT LAYER
│   │   ├── ui/                           # Design system primitives
│   │   │   ├── button.tsx
│   │   │   ├── input.tsx
│   │   │   ├── card.tsx
│   │   │   ├── badge.tsx
│   │   │   ├── modal.tsx
│   │   │   ├── toast.tsx
│   │   │   ├── tabs.tsx
│   │   │   ├── skeleton.tsx
│   │   │   ├── empty-state.tsx
│   │   │   ├── dropdown.tsx
│   │   │   └── avatar.tsx
│   │   ├── competitors/                  # Feature: Competitor UI
│   │   │   ├── competitor-card.tsx
│   │   │   ├── competitor-grid.tsx
│   │   │   ├── competitor-detail.tsx
│   │   │   ├── competitor-timeline.tsx
│   │   │   └── add-competitor-form.tsx
│   │   ├── scanning/                     # Feature: Scanning UI
│   │   │   ├── change-timeline.tsx
│   │   │   ├── diff-viewer.tsx
│   │   │   └── change-badge.tsx
│   │   ├── digest/                       # Feature: Digest UI
│   │   │   ├── digest-card.tsx
│   │   │   ├── digest-detail.tsx
│   │   │   └── feedback-buttons.tsx
│   │   ├── battlecard/                   # Feature: Battlecard UI
│   │   │   ├── battlecard-editor.tsx
│   │   │   ├── battlecard-preview.tsx
│   │   │   └── share-dialog.tsx
│   │   ├── alerts/                       # Feature: Alerts UI
│   │   │   ├── alert-rule-form.tsx
│   │   │   └── alert-log-list.tsx
│   │   ├── settings/                     # Feature: Settings UI
│   │   │   ├── byok-form.tsx
│   │   │   └── integration-config.tsx
│   │   └── layout/                       # Layout components
│   │       ├── sidebar.tsx
│   │       ├── bottom-nav.tsx
│   │       ├── nav-item.tsx
│   │       └── page-header.tsx
│   │
│   ├── lib/                              # 📚 BUSINESS LOGIC LAYER
│   │   ├── db/                           # Database
│   │   │   ├── schema.ts                 # Drizzle schema (all tables)
│   │   │   ├── client.ts                 # Supabase + Drizzle client
│   │   │   ├── migrations/               # SQL migration files
│   │   │   └── queries/                  # Query functions by entity
│   │   │       ├── competitors.ts
│   │   │       ├── snapshots.ts
│   │   │       ├── changes.ts
│   │   │       ├── digests.ts
│   │   │       ├── battlecards.ts
│   │   │       ├── alerts.ts
│   │   │       ├── subscriptions.ts
│   │   │       └── byok-keys.ts
│   │   ├── auth/                         # Authentication
│   │   │   ├── server.ts                 # Better Auth server config
│   │   │   └── client.ts                 # Better Auth client
│   │   ├── ai/                           # AI Integration
│   │   │   ├── gateway.ts                # LLM Gateway client
│   │   │   ├── prompts.ts                # Prompt templates
│   │   │   └── analyze.ts                # Change analysis
│   │   ├── scraper/                      # Web Scraping
│   │   │   ├── browser.ts                # Puppeteer manager
│   │   │   ├── extractor.ts              # Cheerio HTML extraction
│   │   │   └── differ.ts                 # Text diff logic
│   │   ├── stripe/                       # Payment
│   │   │   ├── client.ts
│   │   │   └── webhooks.ts
│   │   ├── email/                        # Email Service
│   │   │   ├── client.ts                 # Resend config
│   │   │   └── templates/
│   │   │       ├── digest.tsx
│   │   │       ├── alert.tsx
│   │   │       └── welcome.tsx
│   │   ├── inngest/                      # Background Jobs
│   │   │   ├── client.ts
│   │   │   └── functions/
│   │   │       ├── scan-competitors.ts
│   │   │       └── generate-digest.ts
│   │   ├── hooks/                        # React hooks (all features)
│   │   │   ├── use-competitors.ts
│   │   │   ├── use-competitor-detail.ts
│   │   │   ├── use-digests.ts
│   │   │   ├── use-battlecards.ts
│   │   │   └── use-alerts.ts
│   │   ├── schemas/                      # Zod validation (all features)
│   │   │   ├── competitor.ts
│   │   │   ├── digest.ts
│   │   │   ├── battlecard.ts
│   │   │   ├── alert.ts
│   │   │   └── settings.ts
│   │   ├── types/                        # TypeScript types
│   │   │   ├── competitor.ts
│   │   │   ├── digest.ts
│   │   │   ├── battlecard.ts
│   │   │   ├── alert.ts
│   │   │   └── api.ts
│   │   └── utils/                        # Utilities
│   │       ├── crypto.ts
│   │       ├── rate-limit.ts
│   │       ├── pagination.ts
│   │       └── format.ts
│   │
│   └── styles/
│       └── globals.css                   # Tailwind + CSS vars
│
├── drizzle.config.ts
├── inngest.config.ts
├── next.config.ts
├── tailwind.config.ts
├── tsconfig.json
├── package.json
└── .env.local
```

---

## Path Aliases (tsconfig.json)

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"],
      "@/components/*": ["./src/components/*"],
      "@/lib/*": ["./src/lib/*"],
      "@/ui/*": ["./src/components/ui/*"]
    }
  }
}
```

---

## Import Patterns

```typescript
// Trong page component
import { CompetitorGrid } from '@/components/competitors/competitor-grid';
import { useCompetitors } from '@/lib/hooks/use-competitors';
import { Badge } from '@/ui/badge';

// Trong API route
import { listCompetitors } from '@/lib/db/queries/competitors';
import { CreateCompetitorSchema } from '@/lib/schemas/competitor';

// Trong component
import { Button } from '@/ui/button';
import type { Competitor } from '@/lib/types/competitor';
```

---

## Developer Workflow

**Kịch bản: Thêm trường `industry` vào Competitor**

```
Files cần sửa:
1. src/lib/db/schema.ts                           ← Thêm column
2. src/lib/db/queries/competitors.ts              ← Update query
3. src/lib/schemas/competitor.ts                  ← Update validation
4. src/lib/types/competitor.ts                    ← Update type
5. src/components/competitors/competitor-detail.tsx ← Hiển thị UI
6. src/app/api/v1/competitors/[id]/route.ts       ← API response

→ 6 files, 4 folders khác nhau
```

---

## Ưu / Nhược Điểm

### Ưu điểm
- ✅ **Phổ biến, quen thuộc** — hầu hết tutorial/boilerplate dùng cấu trúc này
- ✅ **Shared logic dễ** — `lib/hooks/`, `lib/schemas/` tự nhiên dùng chung
- ✅ **Code review theo layer** — FE reviewer xem `components/`, BE reviewer xem `lib/`
- ✅ **Design system tập trung** — `components/ui/` rõ ràng

### Nhược điểm
- ❌ **Scatter**: 1 feature trải qua 4+ folders
- ❌ **Cognitive load cao**: cần nhớ vị trí file trong nhiều folders
- ❌ **Xóa feature khó**: phải tìm xóa ở 4 nơi (dễ bỏ sót)
- ❌ **Circular dependencies**: components import lib, lib import types, types reference components...
- ❌ **Growing pains**: `lib/hooks/` và `lib/schemas/` sẽ lớn dần theo features

---

## File Count Breakdown

| Folder | Số files | Vai trò |
|--------|---------|---------|
| `app/` | ~35 | Routing + API routes |
| `components/` | ~25 | UI components |
| `lib/` | ~35 | Logic, hooks, schemas, types, utils |
| **Tổng** | **~95** | |

---

*Phase 4 — Horizontal Structure v1.0*
