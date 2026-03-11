---
name: responsive-design
description: Mobile-first CSS, breakpoints, responsive layout patterns cho PC + Mobile. Dùng khi cần đảm bảo web app hiển thị tốt trên mọi thiết bị.
---

# Responsive Design — Mobile-First CSS Patterns

Skill hướng dẫn thiết kế responsive web app theo mobile-first approach, bao gồm breakpoints, fluid typography, layout patterns, và touch optimization.

**Source**: Custom skill — tham khảo patterns từ `addyosmani/agent-skills` (`frontend-ui-engineering`).

## Khi Nào Dùng

- Xây dựng layout responsive cho web app mới
- Chuyển đổi desktop-first sang mobile-first
- Cần fix layout bể trên mobile/tablet
- Tối ưu touch targets và mobile UX
- Implement fluid typography và spacing

## Nguyên Tắc Cốt Lõi

### 1. Mobile-First Strategy

**Luôn code mobile trước, sau đó mở rộng lên desktop:**

```css
/* ❌ SAI: Desktop-first */
.container { width: 1200px; }
@media (max-width: 768px) { .container { width: 100%; } }

/* ✅ ĐÚNG: Mobile-first */
.container { width: 100%; }
@media (min-width: 768px) { .container { max-width: 1200px; } }
```

### 2. Breakpoint System Chuẩn

```css
/* === Standard Breakpoints === */
/* Mobile: 0 - 639px (default — không cần media query) */

/* Tablet */
@media (min-width: 640px) { /* sm */ }

/* Tablet Landscape */
@media (min-width: 768px) { /* md */ }

/* Desktop */
@media (min-width: 1024px) { /* lg */ }

/* Large Desktop */
@media (min-width: 1280px) { /* xl */ }

/* Ultra-wide */
@media (min-width: 1536px) { /* 2xl */ }
```

**Quy tắc vàng:** Đừng target thiết bị cụ thể. Design theo content breakpoints — nơi layout bắt đầu "bể".

### 3. Fluid Typography

```css
/* Fluid font size: 16px ở 320px → 20px ở 1280px */
:root {
  --font-body: clamp(1rem, 0.9rem + 0.42vw, 1.25rem);
  --font-h1: clamp(1.75rem, 1.25rem + 2.08vw, 3rem);
  --font-h2: clamp(1.375rem, 1.1rem + 1.15vw, 2.125rem);
  --font-h3: clamp(1.125rem, 1rem + 0.52vw, 1.5rem);
}

body { font-size: var(--font-body); }
h1 { font-size: var(--font-h1); }
```

### 4. Fluid Spacing

```css
:root {
  --space-xs: clamp(0.25rem, 0.2rem + 0.21vw, 0.375rem);
  --space-sm: clamp(0.5rem, 0.4rem + 0.42vw, 0.75rem);
  --space-md: clamp(1rem, 0.8rem + 0.83vw, 1.5rem);
  --space-lg: clamp(1.5rem, 1.1rem + 1.67vw, 2.5rem);
  --space-xl: clamp(2rem, 1.5rem + 2.08vw, 3.25rem);
  --space-2xl: clamp(3rem, 2.2rem + 3.33vw, 5rem);
}
```

## Layout Patterns

### Pattern 1: Stack → Grid

```css
/* Mobile: stacked */
.feature-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-md);
}

/* Tablet: 2 columns */
@media (min-width: 768px) {
  .feature-grid { grid-template-columns: repeat(2, 1fr); }
}

/* Desktop: 3 columns */
@media (min-width: 1024px) {
  .feature-grid { grid-template-columns: repeat(3, 1fr); }
}
```

### Pattern 2: Auto-fit Grid (Responsive tự động)

```css
/* Auto-responsive: tự xuống dòng khi hết chỗ */
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(280px, 100%), 1fr));
  gap: var(--space-md);
}
```

### Pattern 3: Sidebar → Stack

```css
.layout {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-lg);
}

@media (min-width: 1024px) {
  .layout {
    grid-template-columns: 280px 1fr;
  }
}
```

### Pattern 4: Navigation — Hamburger → Full

```css
/* Mobile: hamburger menu */
.nav-items { display: none; }
.nav-toggle { display: block; }
.nav-items.open { display: flex; flex-direction: column; }

/* Desktop: full navigation */
@media (min-width: 1024px) {
  .nav-items { display: flex; gap: var(--space-md); }
  .nav-toggle { display: none; }
}
```

### Pattern 5: Container Queries (Modern)

```css
/* Container queries — responsive theo parent, không theo viewport */
.card-container { container-type: inline-size; }

@container (min-width: 400px) {
  .card { display: flex; gap: 1rem; }
  .card-image { width: 40%; }
}

@container (min-width: 700px) {
  .card { flex-direction: row; }
  .card-content { flex: 1; }
}
```

## Responsive Images

```html
<!-- Responsive image với srcset -->
<img 
  src="image-400.webp"
  srcset="image-400.webp 400w, image-800.webp 800w, image-1200.webp 1200w"
  sizes="(max-width: 640px) 100vw, (max-width: 1024px) 50vw, 33vw"
  alt="Mô tả ảnh"
  loading="lazy"
  decoding="async"
/>

<!-- Art direction với picture element -->
<picture>
  <source media="(max-width: 639px)" srcset="mobile-crop.webp" />
  <source media="(max-width: 1023px)" srcset="tablet-crop.webp" />
  <img src="desktop-full.webp" alt="Mô tả ảnh" />
</picture>
```

## Touch & Mobile Optimization

### Touch Targets

```css
/* Minimum touch target: 44×44px (WCAG), 48×48px (Google khuyến nghị) */
button, a, [role="button"], input, select, textarea {
  min-height: 44px;
  min-width: 44px;
}

/* Padding thay vì min-width cho inline elements */
.nav-link {
  padding: 12px 16px;
}
```

### Viewport Management

```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

```css
/* Ngăn zoom khi focus input trên iOS */
@media (max-width: 640px) {
  input, select, textarea { font-size: 16px; }
}

/* Safe area cho notch devices */
.app-container {
  padding-left: env(safe-area-inset-left);
  padding-right: env(safe-area-inset-right);
  padding-bottom: env(safe-area-inset-bottom);
}
```

### Scroll & Overflow

```css
/* Smooth scrolling */
html { scroll-behavior: smooth; }

/* Prevent horizontal overflow */
body { overflow-x: hidden; }

/* Scroll snap cho carousels */
.carousel {
  display: flex;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  -webkit-overflow-scrolling: touch;
}

.carousel-item {
  flex: 0 0 85%;
  scroll-snap-align: center;
}

@media (min-width: 768px) {
  .carousel-item { flex: 0 0 45%; }
}
```

## Testing Checklist

### Thiết bị cần test
- [ ] iPhone SE (375px) — small mobile
- [ ] iPhone 14 Pro (393px) — standard mobile
- [ ] iPad (768px) — tablet portrait
- [ ] iPad Landscape (1024px) — tablet landscape
- [ ] Laptop (1366px) — common laptop
- [ ] Desktop (1920px) — full HD

### Layout Checklist
- [ ] Không có horizontal scroll ở bất kỳ breakpoint nào
- [ ] Text không bị cắt hoặc overflow
- [ ] Images scale đúng, không bị stretch
- [ ] Forms usable trên mobile (input size, spacing)
- [ ] Tables có horizontal scroll hoặc responsive alternative
- [ ] Modals/popups không bị cắt trên mobile
- [ ] Fixed headers/footers không che content

### Performance Checklist
- [ ] Images dùng srcset/sizes hoặc picture element
- [ ] Lazy loading cho images below the fold
- [ ] CSS Grid/Flexbox thay vì float-based layouts
- [ ] Font size ≥ 16px trên mobile (tránh iOS zoom)
- [ ] Touch targets ≥ 44px
