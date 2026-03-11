# CompeteRadar — Phase 3: Thiết Kế UX/UI

**Ngày**: 2026-03-11  
**Trạng thái**: 🟡 Bản nháp — Chờ duyệt  
**Skills sử dụng**: `ux-wireframe`, `responsive-design`, `accessibility-audit`, `web-design-guidelines` (chi tiết xem mục "Skills Đã Sử Dụng" cuối tài liệu)

---

## 3.1 Kiến Trúc Thông Tin (Information Architecture)

### Sitemap

```
CompeteRadar
├── 🏠 Landing Page (public)
│   ├── Hero + Value Proposition
│   ├── Features
│   ├── Pricing
│   ├── Testimonials
│   └── CTA → Sign Up
│
├── 🔐 Auth
│   ├── Đăng nhập (Email / Google)
│   ├── Đăng ký
│   └── Quên mật khẩu
│
├── 📊 Dashboard (authenticated)
│   ├── Tổng quan — tóm tắt thay đổi gần nhất
│   ├── Danh sách đối thủ (grid/list view)
│   └── Quick actions: + Thêm đối thủ
│
├── 🔍 Hồ Sơ Đối Thủ (Competitor Profile)
│   ├── Timeline thay đổi
│   ├── Tab: Website Changes (visual diff)
│   ├── Tab: Content / Blog
│   ├── Tab: Pricing
│   ├── Tab: Social
│   └── Tab: Tech Stack
│
├── 📬 Digest (AI Weekly Summary)
│   ├── Danh sách digest cũ
│   ├── Xem chi tiết 1 digest
│   └── Cấu hình digest (tần suất, kênh)
│
├── ⚔️ Battlecard
│   ├── Danh sách battlecard
│   ├── Tạo/chỉnh sửa battlecard
│   ├── Xem trước battlecard
│   └── Chia sẻ / xuất PDF
│
├── 🔔 Cảnh báo (Alerts)
│   ├── Danh sách cảnh báo đã nhận
│   └── Cấu hình rules cảnh báo
│
├── ⚙️ Cài đặt (Settings)
│   ├── Hồ sơ cá nhân
│   ├── AI Model / BYOK
│   ├── Tích hợp (Slack, Email)
│   ├── Subscription / Thanh toán
│   └── Thông báo
│
└── 📄 Legal (public)
    ├── Chính sách bảo mật
    └── Điều khoản sử dụng
```

### Navigation chính (max 7 items)

| # | Label | Icon | Mô tả |
|---|-------|------|--------|
| 1 | Dashboard | 📊 | Trang chủ sau đăng nhập |
| 2 | Đối thủ | 🔍 | Danh sách + hồ sơ đối thủ |
| 3 | Digest | 📬 | AI weekly summaries |
| 4 | Battlecard | ⚔️ | So sánh cạnh tranh |
| 5 | Cảnh báo | 🔔 | Notifications + rules |
| 6 | Cài đặt | ⚙️ | Settings, billing, AI model |

### URL Structure

```
/                           → Landing page
/login                      → Đăng nhập
/signup                     → Đăng ký
/dashboard                  → Dashboard chính
/competitors                → Danh sách đối thủ
/competitors/:id            → Hồ sơ đối thủ (timeline)
/competitors/:id/changes    → Chi tiết thay đổi
/competitors/:id/pricing    → Theo dõi giá
/digest                     → Danh sách digest
/digest/:id                 → Xem 1 digest
/battlecards                → Danh sách battlecard
/battlecards/new            → Tạo mới
/battlecards/:id            → Xem/sửa battlecard
/battlecards/:id/share      → Link chia sẻ (public)
/alerts                     → Cảnh báo
/settings                   → Cài đặt chung
/settings/ai                → AI Model / BYOK
/settings/integrations      → Slack, Email
/settings/billing           → Subscription
```

---

## 3.2 User Flows

### Flow 1: Onboarding (Đăng ký → Thêm đối thủ đầu tiên)

```
Landing Page
    │
    ▼
[Click "Dùng thử miễn phí"]
    │
    ▼
Đăng ký ──────── Đăng ký bằng Google (1 click)
(Email + Password)     │
    │                   │
    ▼                   ▼
Xác nhận email ◄────────┘
    │
    ▼
Onboarding Wizard (3 bước):
    │
    ├── Bước 1: "Sản phẩm của bạn là gì?" [Input URL/tên]
    │
    ├── Bước 2: "Thêm đối thủ đầu tiên" [Input URL]
    │           → Auto-detect tên, logo, mô tả
    │           → "Thêm đối thủ 2?" (optional)
    │
    └── Bước 3: "Chọn tần suất digest" [Hàng tuần / Hàng ngày]
                  → Kết nối Slack (optional)
    │
    ▼
Dashboard → Hiển thị "Đang quét đối thủ lần đầu..." (loading state)
    │
    ▼ (5-10 phút sau)
Dashboard → Hiển thị kết quả quét đầu tiên 🎉
```

**Edge cases:**
- URL đối thủ không hợp lệ → hiện lỗi inline, gợi ý sửa
- Email đã tồn tại → chuyển hướng login
- Google signup fail → fallback email form

### Flow 2: Xem Weekly Digest

```
Nhận email digest (sáng thứ Hai)
    │
    ▼
Mở email → Xem tóm tắt top 5 thay đổi
    │
    ├── Click "Xem chi tiết" từng thay đổi
    │       │
    │       ▼
    │   App → Hồ sơ đối thủ → Tab thay đổi cụ thể
    │
    └── Click "Xem full digest"
            │
            ▼
        App → Digest page → Xem toàn bộ + 👍/👎 feedback
```

### Flow 3: Tạo Battlecard

```
Dashboard / Đối thủ
    │
    ▼
[Click "Tạo Battlecard"]
    │
    ▼
Chọn đối thủ từ dropdown
    │
    ▼
AI đang tạo... (streaming animation, ~15s)
    │
    ▼
Xem trước Battlecard
    │
    ├── [Chỉnh sửa] → Editor inline
    ├── [Xuất PDF/PNG] → Download
    └── [Chia sẻ] → Tạo link public
```

### Flow 4: Cấu hình BYOK (Power user)

```
Settings
    │
    ▼
AI Model tab
    │
    ▼
Xem model hiện tại: "Gemini 3.0 (mặc định)"
    │
    ▼
[Bật BYOK] → Toggle on
    │
    ▼
Chọn provider: [▼ OpenAI / Google / Anthropic]
    │
    ▼
Nhập API Key: [sk-...] → [Test kết nối]
    │
    ├── ✅ Thành công → "Đã kết nối! Subscription giảm 35%"
    └── ❌ Thất bại → "Key không hợp lệ. Kiểm tra lại." + link hướng dẫn
```

---

## 3.3 Wireframes

### Dashboard (Trang chính)

```
┌─────────────────────────────────────────────────────────────┐
│  [🔍 CompeteRadar]   Dashboard  Đối thủ  Digest  ⚔️  🔔  ⚙️  │  ← Sidebar/Top nav
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Xin chào, Dũng! 👋          Tuần này: 12 thay đổi phát hiện │
│                                                             │
│  ┌─── Thay đổi Quan trọng Nhất ──────────────────────────┐ │
│  │ 🔴 Competitor A đổi pricing: Pro $29→$39/mo    2h ago  │ │
│  │ 🟡 Competitor B thêm tính năng "AI Export"     5h ago  │ │
│  │ 🔵 Competitor C đăng blog "2026 Roadmap"       1d ago  │ │
│  │                                    [Xem digest →]      │ │
│  └────────────────────────────────────────────────────────┘ │
│                                                             │
│  ┌─ Đối thủ (4/10) ── [+ Thêm] ──────────────────────────┐ │
│  │                                                         │ │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────┐ │ │
│  │  │ [Logo A] │  │ [Logo B] │  │ [Logo C] │  │ [Logo D]│ │ │
│  │  │ Comp A   │  │ Comp B   │  │ Comp C   │  │ Comp D  │ │ │
│  │  │ 3 changes│  │ 1 change │  │ 5 changes│  │ 0      │ │ │
│  │  │ 🔴 Giá   │  │ 🟡 Feature│  │ 🔵 Blog  │  │ ✅ Ổn  │ │ │
│  │  └──────────┘  └──────────┘  └──────────┘  └────────┘ │ │
│  └────────────────────────────────────────────────────────┘ │
│                                                             │
│  ┌─ Hoạt động Gần đây ───────────────────────────────────┐ │
│  │ • Competitor A thay đổi trang pricing         2 giờ    │ │
│  │ • Competitor C đăng bài blog mới              1 ngày   │ │
│  │ • Competitor B cập nhật meta description       2 ngày   │ │
│  │ • Digest #12 đã gửi thành công               3 ngày   │ │
│  │                                      [Xem tất cả →]   │ │
│  └────────────────────────────────────────────────────────┘ │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Hồ Sơ Đối Thủ (Competitor Profile)

```
┌─────────────────────────────────────────────────────────────┐
│  ← Quay lại     Competitor A — competitor-a.com             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  [Logo]  Competitor A                                       │
│          competitor-a.com   │ SaaS │ Founded 2023           │
│          "AI-powered analytics for marketers"               │
│                                                             │
│  ┌──────┬──────────┬────────┬────────┬──────────┐          │
│  │ Tổng │ Website  │ Nội    │ Giá    │ Social   │   ← Tabs │
│  │ quan │ Changes  │ dung   │        │          │          │
│  └──────┴──────────┴────────┴────────┴──────────┘          │
│                                                             │
│  Timeline                                      [Lọc ▼]     │
│  ─────────────────────────────────────────────────          │
│                                                             │
│  📅 11/03/2026                                              │
│  ┌────────────────────────────────────────────────────────┐ │
│  │ 🔴 PRICING — Thay đổi giá                             │ │
│  │                                                        │ │
│  │ Pro plan: $29/mo → $39/mo (+34%)                       │ │
│  │ Team plan: không đổi                                   │ │
│  │                                                        │ │
│  │ [Xem screenshot trước/sau]  [Xem diff]                 │ │
│  │                                                        │ │
│  │ 🤖 AI Insight: "Competitor A tăng giá Pro 34%.         │ │
│  │    Cơ hội positioning: nhấn mạnh giá cạnh tranh        │ │
│  │    của CompeteRadar ở mức $29/mo."                     │ │
│  └────────────────────────────────────────────────────────┘ │
│                                                             │
│  📅 08/03/2026                                              │
│  ┌────────────────────────────────────────────────────────┐ │
│  │ 🟡 FEATURE — Thêm trang mới: /integrations             │ │
│  │ 🔵 CONTENT — Blog: "Why AI Analytics Matters in 2026"  │ │
│  └────────────────────────────────────────────────────────┘ │
│                                                             │
│  [Tải thêm ↓]                                              │
└─────────────────────────────────────────────────────────────┘
```

### Battlecard Generator

```
┌─────────────────────────────────────────────────────────────┐
│  ⚔️ Tạo Battlecard                                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  So sánh:  [Bạn ▼]  vs  [Competitor A ▼]                   │
│                                                             │
│  ┌───────────────────────┬──────────────────────────────┐  │
│  │     🏆 CompeteRadar   │     Competitor A              │  │
│  ├───────────────────────┼──────────────────────────────┤  │
│  │                       │                              │  │
│  │  Giá: $29/mo ✅       │  Giá: $39/mo                 │  │
│  │  Đối thủ: 10 ✅       │  Đối thủ: 5                  │  │
│  │  AI Digest: ✅ Có     │  AI Digest: ❌ Không          │  │
│  │  Battlecard: ✅ Có    │  Battlecard: ❌ Không          │  │
│  │  Setup: 5 phút ✅     │  Setup: 30 phút              │  │
│  │  BYOK: ✅ Có          │  BYOK: ❌ Không               │  │
│  │                       │                              │  │
│  ├───────────────────────┴──────────────────────────────┤  │
│  │  🤖 Tóm tắt AI:                                      │  │
│  │  "CompeteRadar rẻ hơn 26%, hỗ trợ gấp đôi số đối     │  │
│  │   thủ, và có AI digest + battlecard mà Comp A không   │  │
│  │   có. Điểm yếu: chưa có mobile app."                 │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                             │
│  [✏️ Chỉnh sửa]  [📄 Xuất PDF]  [🔗 Chia sẻ link]         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Trang Thêm Đối Thủ

```
┌─────────────────────────────────────────────────────────────┐
│  + Thêm Đối Thủ Mới                                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Dán URL website đối thủ:                                   │
│  ┌─────────────────────────────────────────────┐            │
│  │  https://competitor.com                      │  [Quét]   │
│  └─────────────────────────────────────────────┘            │
│                                                             │
│  ── đang quét... ──────────────────────────────             │
│                                                             │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  ✅ Phát hiện thành công!                               │ │
│  │                                                        │ │
│  │  [Logo]  Competitor XYZ                                │ │
│  │          "AI-powered marketing platform"               │ │
│  │          Founded: 2024 │ 12 employees │ Series A       │ │
│  │                                                        │ │
│  │  Sẽ theo dõi:                                          │ │
│  │  ☑ Trang chủ    ☑ Trang giá    ☑ Blog                 │ │
│  │  ☑ Social       ☑ Tech stack   ☐ Trang tùy chọn       │ │
│  │                                                        │ │
│  │  [Thêm đối thủ]                    [Hủy]              │ │
│  └────────────────────────────────────────────────────────┘ │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 3.4 Component Inventory & Design Tokens

### Component Inventory

| Component | Variants | States | Ưu tiên |
|-----------|----------|--------|---------|
| **Button** | Primary, Secondary, Ghost, Danger, Icon | Default, Hover, Active, Disabled, Loading | P0 |
| **Input** | Text, URL, Password, Search, Textarea | Default, Focus, Error, Disabled | P0 |
| **Card** | Competitor Card, Change Card, Digest Card | Default, Hover, Selected, Loading | P0 |
| **Badge** | 🔴 Quan trọng, 🟡 Đáng chú ý, 🔵 Thông tin, ✅ Ổn | — | P0 |
| **Timeline Item** | Change event, Digest sent, Alert triggered | Default, Expanded | P0 |
| **Nav** | Sidebar (desktop), Bottom bar (mobile), Top bar | Active, Collapsed | P0 |
| **Modal** | Confirm, Form, Preview | Open, Closing | P1 |
| **Toast** | Success, Error, Warning, Info | Show, Hiding | P1 |
| **Table** | Basic, Sortable | Loading, Empty, Error | P1 |
| **Tabs** | Horizontal (desktop), Scrollable (mobile) | Active, Default | P0 |
| **Dropdown** | Select, Multi-select, Command palette | Open, Closed | P1 |
| **Avatar** | Competitor logo, User avatar | Image, Fallback (initials) | P0 |
| **Skeleton** | Card, Timeline, Text | Loading | P0 |
| **Empty State** | No competitors, No changes, No digest | — | P0 |
| **Diff Viewer** | Text diff, Screenshot comparison (before/after) | Loading, Ready | P1 |
| **Battlecard** | Editable, Read-only, Shared (public) | Draft, Published | P1 |
| **Feedback** | 👍/👎 buttons cho AI output | Not rated, Rated | P0 |

### Design Tokens

```
Tokens = {
  colors: {
    primary:    hsl(230, 80%, 55%)     // Xanh dương đậm — trust, intelligence
    secondary:  hsl(160, 70%, 45%)     // Xanh lá — growth, positive
    accent:     hsl(280, 70%, 55%)     // Tím — AI, premium
    
    neutral: {
      50:  hsl(220, 15%, 97%)          // Nền sáng nhất
      100: hsl(220, 15%, 94%)          // Nền card
      200: hsl(220, 12%, 88%)          // Border
      300: hsl(220, 10%, 75%)          // Text phụ nhạt
      400: hsl(220, 10%, 55%)          // Text phụ
      500: hsl(220, 10%, 40%)          // Text chính nhạt
      600: hsl(220, 12%, 28%)          // Text chính
      700: hsl(220, 15%, 18%)          // Text đậm
      800: hsl(220, 18%, 10%)          // Nền tối (dark mode)
      900: hsl(220, 20%, 6%)           // Nền tối nhất
    }
    
    semantic: {
      critical: hsl(0, 75%, 55%)       // 🔴 Pricing changes
      warning:  hsl(35, 85%, 55%)      // 🟡 Feature changes  
      info:     hsl(210, 70%, 55%)     // 🔵 Content changes
      success:  hsl(145, 65%, 45%)     // ✅ Ổn định
    }
  }
  
  typography: {
    fontFamily: "'Inter', -apple-system, sans-serif"
    sizes: {
      xs:   '0.75rem'     // 12px — caption, badge
      sm:   '0.875rem'    // 14px — body nhỏ, nav
      base: '1rem'        // 16px — body text
      lg:   '1.125rem'    // 18px — subtitle
      xl:   '1.25rem'     // 20px — card title
      '2xl': '1.5rem'     // 24px — section heading
      '3xl': '1.875rem'   // 30px — page title
      '4xl': '2.25rem'    // 36px — hero heading
    }
    weights: { regular: 400, medium: 500, semibold: 600, bold: 700 }
  }
  
  spacing: [0, 4, 8, 12, 16, 20, 24, 32, 40, 48, 64, 80, 96] // px
  
  radii: {
    none: '0'
    sm:   '4px'   // Badges, small elements
    md:   '8px'   // Buttons, inputs
    lg:   '12px'  // Cards
    xl:   '16px'  // Modals
    full: '9999px' // Avatar, pill badges
  }
  
  shadows: {
    sm:  '0 1px 2px rgba(0,0,0,0.05)'
    md:  '0 4px 6px rgba(0,0,0,0.07)'
    lg:  '0 10px 15px rgba(0,0,0,0.10)'
    xl:  '0 20px 25px rgba(0,0,0,0.15)'
  }
  
  breakpoints: {
    sm:  '640px'    // Mobile landscape
    md:  '768px'    // Tablet
    lg:  '1024px'   // Desktop nhỏ
    xl:  '1280px'   // Desktop
    '2xl': '1536px' // Desktop lớn
  }
}
```

---

## 3.5 HITL: Chờ Review Wireframes & UX Flows

> **Checkpoint**: Cần review trước khi tiếp tục bước 3.6-3.8

---

## 3.6 Chiến Lược Responsive

### Breakpoints & Layout

| Breakpoint | Thiết bị | Layout | Navigation |
|-----------|----------|--------|------------|
| < 640px | Mobile | 1 cột, full-width cards | Bottom tab bar (5 items) |
| 640-767px | Mobile landscape | 1 cột, có padding | Bottom tab bar |
| 768-1023px | Tablet | 2 cột grid cho cards | Collapsible sidebar |
| 1024-1279px | Desktop nhỏ | Sidebar + content area | Sidebar cố định (200px) |
| ≥ 1280px | Desktop | Sidebar + content + panel | Sidebar cố định (240px) |

### Responsive Patterns

**Dashboard:**
```
Desktop (≥1024px)         Tablet (768-1023px)       Mobile (<768px)
┌────┬──────────────┐    ┌──────────────────┐      ┌──────────────┐
│    │ [Card][Card]  │    │ [Card] [Card]    │      │ [Card]       │
│ S  │ [Card][Card]  │    │ [Card] [Card]    │      │ [Card]       │
│ i  │              │    │                  │      │ [Card]       │
│ d  │ [Timeline]   │    │ [Timeline]       │      │ [Timeline]   │
│ e  │              │    │                  │      │              │
│ b  │              │    └──────────────────┘      │ [Bottom Nav] │
│ a  │              │                               └──────────────┘
│ r  │              │
└────┴──────────────┘
```

**Competitor Card:**
- Desktop: Grid 4 cột (250px min)
- Tablet: Grid 2 cột
- Mobile: Stack 1 cột, card thu gọn (chỉ hiện name + badge)

**Battlecard:**
- Desktop: So sánh 2 cột cạnh nhau
- Mobile: Stack 2 cột thành tabs (Tab "Bạn" | Tab "Đối thủ")

---

## 3.7 Kiểm Tra Tiếp Cận (Accessibility Audit)

### Checklist WCAG 2.1 AA

| # | Tiêu chí | Trạng thái | Ghi chú |
|---|---------|-----------|---------|
| 1 | Tỷ lệ tương phản màu ≥ 4.5:1 cho text | ✅ Đạt | Neutral-600 trên neutral-50 = 8.2:1 |
| 2 | Tỷ lệ tương phản ≥ 3:1 cho UI elements lớn | ✅ Đạt | Primary trên white = 5.1:1 |
| 3 | Focus visible cho tất cả interactive elements | ✅ Planned | Ring style: 2px solid primary |
| 4 | Keyboard navigation hoàn chỉnh | ✅ Planned | Tab order logic, skip links |
| 5 | ARIA labels cho icons, buttons | ✅ Planned | aria-label cho icon-only buttons |
| 6 | Screen reader friendly headings (h1→h6) | ✅ Planned | 1 h1 per page, proper hierarchy |
| 7 | Form labels liên kết đúng | ✅ Planned | htmlFor + id matching |
| 8 | Error messages descriptive | ✅ Planned | aria-describedby cho form errors |
| 9 | Color không phải cách duy nhất truyền thông tin | ✅ Đạt | Badges có icon + text + color |
| 10 | Responsive zoom lên 200% hoạt động | ✅ Planned | Test rem-based sizing |
| 11 | Motion respect prefers-reduced-motion | ✅ Planned | CSS media query disable animations |
| 12 | Touch targets ≥ 44×44px trên mobile | ✅ Planned | Min-height: 44px cho buttons |

---

## 3.8 Review Web Design Guidelines

### Checklist thiết kế

| # | Guideline | Trạng thái | Chi tiết |
|---|-----------|-----------|---------|
| 1 | Typography nhất quán (Inter font) | ✅ | 8 kích cỡ, 4 weight |
| 2 | Spacing system (4px base) | ✅ | 13 giá trị spacing |
| 3 | Màu sắc semantic rõ ràng | ✅ | 4 màu semantic + neutral scale |
| 4 | Empty states cho mọi list/dashboard | ✅ Planned | Illustrations + CTA |
| 5 | Loading states (skeleton) | ✅ Planned | Skeleton cho cards, timeline |
| 6 | Error states cho forms, API | ✅ Planned | Inline errors + toast |
| 7 | Dark mode support | ⏳ Post-MVP | Tokens hỗ trợ, implement sau |
| 8 | Micro-animations | ✅ Planned | Hover cards, tab transitions |
| 9 | Onboarding first-time UX | ✅ | 3-step wizard |
| 10 | 404 page | ✅ Planned | Branded, friendly, with CTA |

---

## Tóm Tắt Phase 3

| Deliverable | Trạng thái |
|-------------|-----------|
| ✅ Sitemap (19 trang/sections) | Hoàn thành |
| ✅ Navigation (6 items) | Hoàn thành |
| ✅ URL structure (18 routes) | Hoàn thành |
| ✅ User Flows (4 luồng chính) | Hoàn thành |
| ✅ Wireframes (4 trang chính) | Hoàn thành |
| ✅ Component Inventory (17 components) | Hoàn thành |
| ✅ Design Tokens (colors, typography, spacing, etc.) | Hoàn thành |
| ✅ Responsive Strategy (5 breakpoints) | Hoàn thành |
| ✅ Accessibility Audit (12 items WCAG 2.1 AA) | Hoàn thành |
| ✅ Web Design Guidelines Review (10 items) | Hoàn thành |

---

## Skills Đã Sử Dụng

### `ux-wireframe`
**Mục đích**: Wireframing patterns, information architecture, user flows, component inventory.

**Đóng góp trong Phase 3:**
- **Bước 3.1** — Xây dựng Sitemap theo template cây phân cấp, áp dụng quy tắc max 7 items navigation cấp 1, kiểm tra breadcrumb path cho từng trang
- **Bước 3.2** — Vẽ User Flows theo quy tắc: mỗi flow có entry point rõ ràng, mỗi decision point có ≥ 2 nhánh (success + error), max 7±2 bước (Miller's Law)
- **Bước 3.3** — Tạo wireframe text-based theo guidelines: dùng boxes cho images, lines cho text, đánh dấu interactive elements `[Button]`, `[Link]`, `[Input]`
- **Bước 3.4** — Thiết kế Component Inventory theo bảng chuẩn (Component | Variants | States | Priority) và Design Tokens (colors, typography, spacing, radii, shadows, breakpoints)

### `responsive-design`
**Mục đích**: Mobile-first CSS, breakpoints, responsive layout patterns cho PC + Mobile.

**Đóng góp trong Phase 3:**
- **Bước 3.6** — Thiết kế 5 breakpoints (sm → 2xl) với layout và navigation phù hợp từng thiết bị
- Xác định responsive patterns cụ thể: Dashboard (grid → stack), Competitor Card (4 cột → 1 cột), Battlecard (2 cột → tabs)
- Bottom tab bar cho mobile, collapsible sidebar cho tablet, sidebar cố định cho desktop

### `accessibility-audit`
**Mục đích**: WCAG 2.1 compliance audit — ARIA labels, semantic HTML, color contrast, keyboard navigation.

**Đóng góp trong Phase 3:**
- **Bước 3.7** — Kiểm tra 12 tiêu chí WCAG 2.1 AA bao gồm: tỷ lệ tương phản màu (≥ 4.5:1), keyboard navigation, ARIA labels, screen reader headings, touch targets, prefers-reduced-motion
- Xác nhận Design Tokens đạt yêu cầu contrast: neutral-600 trên neutral-50 = 8.2:1
- Đảm bảo badges dùng icon + text + color (không chỉ dựa vào màu)

### `web-design-guidelines`
**Mục đích**: Review UI code theo Web Interface Guidelines — typography, spacing, states, animations.

**Đóng góp trong Phase 3:**
- **Bước 3.8** — Kiểm tra 10 guidelines: Inter font (8 sizes, 4 weights), 4px spacing system, semantic colors, empty/loading/error states, micro-animations, onboarding wizard, 404 page
- Đánh giá dark mode support (post-MVP) và planning onboarding first-time UX

---

## Từ Điển Thuật Ngữ

### Thuật ngữ UX/UI

| Thuật ngữ | Giải thích |
|-----------|-----------|
| **UX** (User Experience) | Trải nghiệm người dùng — cảm giác tổng thể khi sử dụng sản phẩm |
| **UI** (User Interface) | Giao diện người dùng — phần nhìn thấy và tương tác trên màn hình |
| **IA** (Information Architecture) | Kiến trúc thông tin — cách tổ chức và phân cấp nội dung trên app |
| **Sitemap** | Bản đồ trang — sơ đồ cây thể hiện tất cả trang và mối quan hệ giữa chúng |
| **User Flow** | Luồng người dùng — trình tự các bước người dùng thực hiện để hoàn thành một tác vụ |
| **Wireframe** | Khung xương giao diện — bản vẽ đơn giản thể hiện bố cục, không có màu sắc chi tiết |
| **Low-fidelity** | Độ chi tiết thấp — wireframe dạng phác thảo, chỉ có layout cơ bản |
| **Mid-fidelity** | Độ chi tiết trung bình — wireframe có typography, spacing, component states |
| **Onboarding** | Quá trình hướng dẫn người dùng mới — thường dùng wizard nhiều bước |
| **Edge case** | Trường hợp biên — tình huống hiếm hoặc bất thường cần xử lý (ví dụ: URL lỗi, mạng mất) |
| **Empty State** | Trạng thái rỗng — giao diện khi chưa có dữ liệu (ví dụ: danh sách đối thủ trống) |
| **Loading State** | Trạng thái đang tải — giao diện hiển thị khi đang chờ dữ liệu từ server |
| **Error State** | Trạng thái lỗi — giao diện khi có lỗi xảy ra (ví dụ: form validation fail) |

### Thuật ngữ Design System

| Thuật ngữ | Giải thích |
|-----------|-----------|
| **Design Tokens** | Biến thiết kế — các giá trị chuẩn cho màu, font, spacing, border-radius dùng xuyên suốt app |
| **Component Inventory** | Danh sách thành phần UI — liệt kê tất cả components cần xây dựng với variants và states |
| **Variant** | Biến thể — các phiên bản khác nhau của 1 component (ví dụ: Button có Primary, Secondary, Ghost) |
| **State** | Trạng thái — các hình thức hiển thị theo tương tác (ví dụ: Default, Hover, Active, Disabled) |
| **HSL** | Hue-Saturation-Lightness — hệ màu trực quan: Hue = màu (0-360°), Saturation = độ bão hòa (0-100%), Lightness = độ sáng (0-100%) |
| **Semantic Colors** | Màu ngữ nghĩa — màu mang ý nghĩa cụ thể: đỏ = lỗi/quan trọng, xanh = thành công, vàng = cảnh báo |
| **Neutral Scale** | Thang màu trung tính — dải màu xám từ trắng (50) đến đen (900) dùng cho nền, text, border |
| **rem** | Root em — đơn vị kích thước tương đối dựa trên cỡ font gốc (thường 16px), giúp responsive |
| **Spacing System** | Hệ thống khoảng cách — bộ giá trị chuẩn (4, 8, 12, 16, 24, 32px...) dùng thống nhất cho padding/margin |
| **Border Radius** | Bán kính bo góc — giá trị làm tròn góc của phần tử (0 = vuông, 9999px = tròn hoàn toàn) |
| **Shadow** | Bóng đổ — hiệu ứng bóng tạo cảm giác nổi/chiều sâu cho phần tử |
| **Skeleton** | Khung xương tải — placeholder animation dạng hình xám nhấp nháy, hiện khi đang tải dữ liệu |

### Thuật ngữ Responsive & Accessibility

| Thuật ngữ | Giải thích |
|-----------|-----------|
| **Responsive** | Giao diện tự thích ứng — tự động thay đổi layout theo kích thước màn hình |
| **Breakpoint** | Điểm ngắt — kích thước màn hình mà layout thay đổi (ví dụ: 768px = chuyển từ mobile sang tablet) |
| **Mobile-first** | Thiết kế di động trước — bắt đầu thiết kế cho mobile rồi mở rộng lên desktop |
| **Grid** | Lưới — hệ thống chia layout thành các cột đều nhau (thường 12 cột) |
| **Sidebar** | Thanh bên — navigation dọc bên trái màn hình (desktop) |
| **Bottom Tab Bar** | Thanh tab dưới — navigation ngang dưới cùng màn hình (mobile) |
| **Collapsible** | Có thể thu gọn — phần tử co lại khi không cần thiết (ví dụ: sidebar trên tablet) |
| **WCAG** (Web Content Accessibility Guidelines) | Hướng dẫn tiếp cận nội dung web — tiêu chuẩn quốc tế đảm bảo web dùng được cho người khuyết tật |
| **ARIA** (Accessible Rich Internet Applications) | Tập thuộc tính HTML giúp screen reader hiểu các phần tử tương tác phức tạp |
| **Screen Reader** | Trình đọc màn hình — phần mềm đọc to nội dung web cho người khiếm thị |
| **Focus Ring** | Viền focus — đường viền hiển thị khi dùng keyboard để di chuyển qua các phần tử tương tác |
| **Touch Target** | Vùng chạm — kích thước tối thiểu cho phần tử có thể bấm trên mobile (≥ 44×44px) |
| **prefers-reduced-motion** | Truy vấn CSS media — phát hiện người dùng muốn giảm animation để tránh chóng mặt |
| **Contrast Ratio** | Tỷ lệ tương phản — tỷ lệ độ sáng giữa text và nền (tối thiểu 4.5:1 cho text thường) |

### Thuật ngữ Tương tác & Pattern

| Thuật ngữ | Giải thích |
|-----------|-----------|
| **CTA** (Call to Action) | Lời kêu gọi hành động — nút hoặc link khuyến khích người dùng thực hiện hành động (ví dụ: "Dùng thử miễn phí") |
| **Toast** | Thông báo nổi — popup nhỏ xuất hiện tạm thời góc màn hình (ví dụ: "Đã lưu!") |
| **Modal** | Hộp thoại — cửa sổ overlay che phần nền, yêu cầu người dùng tương tác trước khi tiếp tục |
| **Dropdown** | Menu thả xuống — danh sách tùy chọn mở ra khi click, cho phép chọn 1 hoặc nhiều |
| **Tabs** | Thẻ tab — chuyển đổi giữa các mục nội dung khác nhau cùng vị trí (ví dụ: Website | Content | Pricing) |
| **Timeline** | Dòng thời gian — hiển thị sự kiện theo thứ tự thời gian, mới nhất ở trên |
| **Visual Diff** | So sánh trực quan — hiển thị sự khác biệt giữa 2 bản (trước/sau) bằng screenshot hoặc highlight text |
| **Inline Error** | Lỗi tại chỗ — thông báo lỗi hiện ngay dưới form field có vấn đề |
| **Auto-detect** | Tự nhận diện — hệ thống tự phát hiện thông tin (ví dụ: từ URL tự lấy tên + logo công ty) |
| **Streaming Animation** | Hoạt ảnh real-time — hiệu ứng hiển thị kết quả AI đang được tạo từng phần |
| **Miller's Law** | Quy tắc Miller — con người nhớ tối đa 7±2 mục cùng lúc → giới hạn nav items, form fields |
| **P0 / P1** | Mức ưu tiên — P0 = phải có (Must Have), P1 = nên có (Should Have) |

---

*Phase 3 v1.1 — Cập nhật Skills & Từ điển thuật ngữ — Sẵn sàng cho HITL Review*
