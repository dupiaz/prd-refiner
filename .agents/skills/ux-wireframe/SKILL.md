---
name: ux-wireframe
description: Wireframing patterns, information architecture, user flows, và component inventory. Dùng khi cần thiết kế UI/UX từ ý tưởng thô đến wireframe chi tiết.
---

# UX Wireframe & Information Architecture

Skill hướng dẫn quy trình wireframing chuyên nghiệp — từ sitemap, user flows đến wireframe chi tiết cho web app.

**Source**: Custom skill — tham khảo `addyosmani/agent-skills` (`frontend-ui-engineering`).

## Khi Nào Dùng

- Thiết kế UI/UX cho tính năng mới hoặc toàn bộ app
- Cần tổ chức information architecture (IA) cho web app
- Vẽ user flows trước khi code
- Lập component inventory / design tokens
- Cần responsive wireframe cho cả mobile + desktop

## Quy Trình Wireframing

### Bước 1: Information Architecture (IA)

Trước khi vẽ, cần xác định cấu trúc thông tin:

1. **Sitemap** — cây phân cấp trang/sections
   ```
   Home
   ├── Dashboard
   │   ├── Overview
   │   ├── Analytics
   │   └── Settings
   ├── Products
   │   ├── Product List
   │   └── Product Detail
   ├── Auth
   │   ├── Login
   │   ├── Register
   │   └── Forgot Password
   └── Legal
       ├── Privacy Policy
       └── Terms of Service
   ```

2. **Content Inventory** — liệt kê nội dung cần hiển thị trên mỗi trang
3. **Card Sorting** — nhóm các thông tin liên quan để tổ chức navigation
4. **Tree Testing** — validate cấu trúc bằng cách kiểm tra: "Người dùng có tìm được X ở đâu?"

### Bước 2: User Flows

Vẽ luồng người dùng cho các tác vụ chính (core tasks):

```
[Entry Point] → [Decision] → [Action] → [Feedback] → [Next State]

Ví dụ: Signup Flow
Landing Page → Click "Sign Up" → Fill Form → Submit → Email Verification → Dashboard

Ví dụ: Purchase Flow
Product List → Select Product → View Detail → Add to Cart → Checkout → Payment → Confirmation
```

**Quy tắc User Flow:**
- Mỗi flow có 1 entry point rõ ràng
- Mỗi decision point có ≥ 2 nhánh (success + error)
- Luôn có error/edge case flows (validation fail, timeout, empty state)
- Max 7±2 bước cho flow chính (Miller's Law)

### Bước 3: Low-Fidelity Wireframe

Dùng text-based wireframe hoặc ASCII art:

```
┌──────────────────────────────────┐
│  [Logo]    [Nav1] [Nav2] [CTA]   │  ← Header
├──────────────────────────────────┤
│                                  │
│  ┌─────────┐  ┌───────────────┐  │
│  │  Hero    │  │  Hero Text    │  │  ← Hero Section
│  │  Image   │  │  [CTA Button] │  │
│  └─────────┘  └───────────────┘  │
│                                  │
│  ┌─────┐ ┌─────┐ ┌─────┐       │
│  │Card │ │Card │ │Card │       │  ← Feature Grid
│  │  1  │ │  2  │ │  3  │       │
│  └─────┘ └─────┘ └─────┘       │
│                                  │
│  [Footer Links]    [Social]      │  ← Footer
└──────────────────────────────────┘
```

**Wireframe Guidelines:**
- Dùng boxes cho images, lines cho text
- Không dùng màu sắc — chỉ grayscale
- Đánh dấu interactive elements: `[Button]`, `[Link]`, `[Input]`
- Ghi chú behavior: hover, click, scroll
- Chú thích responsive breakpoints

### Bước 4: Mid-Fidelity Wireframe

Thêm chi tiết vào wireframe:

- **Typography hierarchy**: H1, H2, H3, body, caption
- **Spacing system**: 4px base unit (4, 8, 12, 16, 24, 32, 48, 64)
- **Grid system**: 12-column grid, gutter 16-24px
- **Component states**: Default, hover, active, disabled, loading, error, empty
- **Responsive variants**: Mobile (320-767px), Tablet (768-1023px), Desktop (1024+)

### Bước 5: Component Inventory

Liệt kê tất cả UI components cần build:

| Component | Variants | States | Priority |
|-----------|----------|--------|----------|
| Button | Primary, Secondary, Ghost, Danger | Default, Hover, Active, Disabled, Loading | P0 |
| Input | Text, Email, Password, Search, Textarea | Default, Focus, Error, Disabled | P0 |
| Card | Basic, Image, Action | Default, Hover, Selected | P0 |
| Modal | Small, Medium, Large | Open, Closing | P1 |
| Toast | Success, Error, Warning, Info | Show, Hide | P1 |
| Table | Basic, Sortable, Selectable | Loading, Empty, Error | P1 |
| Nav | Horizontal, Vertical, Mobile Drawer | Expanded, Collapsed | P0 |

### Bước 6: Design Tokens Planning

```
Tokens = { 
  colors: { primary, secondary, neutral, semantic (success/error/warning/info) },
  typography: { font-family, sizes (xs→3xl), weights, line-heights },
  spacing: { 0, 1, 2, 3, 4, 5, 6, 8, 10, 12, 16, 20, 24 } (× 4px),
  radii: { none, sm, md, lg, full },
  shadows: { sm, md, lg, xl },
  breakpoints: { sm: 640px, md: 768px, lg: 1024px, xl: 1280px, 2xl: 1536px }
}
```

## Checklists

### IA Checklist
- [ ] Sitemap hoàn chỉnh với tất cả trang
- [ ] Navigation có max 7 items cấp 1
- [ ] Mỗi trang có breadcrumb path rõ ràng
- [ ] URL structure khớp với IA hierarchy
- [ ] Search functionality nếu > 20 trang nội dung

### Wireframe Checklist
- [ ] Tất cả core pages có wireframe
- [ ] Mobile wireframe cho mọi trang
- [ ] Empty states cho lists, search results, dashboard
- [ ] Error states cho forms, API failures
- [ ] Loading states cho async operations
- [ ] 404 và error pages
- [ ] Onboarding / first-time user experience

### User Flow Checklist
- [ ] Happy path cho mỗi core task
- [ ] Error/edge case paths
- [ ] Entry points từ multiple sources (direct, email, social)
- [ ] Exit points và retention hooks
- [ ] Accessibility: keyboard-only navigation flow
