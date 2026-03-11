---
name: accessibility-audit
description: WCAG 2.1 compliance audit — ARIA labels, semantic HTML, color contrast, keyboard navigation, screen reader patterns. Dùng khi cần kiểm tra hoặc cải thiện accessibility cho web app.
---

# Accessibility Audit — WCAG 2.1 Compliance

Skill hướng dẫn kiểm tra và cải thiện accessibility theo chuẩn WCAG 2.1 AA, bao gồm semantic HTML, ARIA, color contrast, keyboard navigation, và screen reader support.

**Source**: Custom skill — tham khảo `addyosmani/web-quality-skills` (`accessibility`) và `kodustech/awesome-agent-skills` (`wcag-audit-patterns`).

## Khi Nào Dùng

- Audit accessibility cho web app hiện có
- Xây dựng web app mới cần đạt WCAG 2.1 AA
- Fix accessibility issues từ automated testing (axe-core, Lighthouse)
- Chuẩn bị cho compliance review (ADA, Section 508, EN 301 549)
- Cải thiện trải nghiệm cho người dùng khuyết tật

## 4 Nguyên Tắc POUR

### 1. Perceivable (Nhận biết được)

Nội dung phải được trình bày theo cách người dùng có thể nhận thức.

#### Text Alternatives (1.1)
```html
<!-- ✅ Hình ảnh có alt text mô tả -->
<img src="chart.png" alt="Biểu đồ doanh thu Q1 2026: tăng 23% so với Q4 2025" />

<!-- ✅ Decorative images: alt rỗng -->
<img src="divider.png" alt="" role="presentation" />

<!-- ✅ Complex images: mô tả chi tiết -->
<figure>
  <img src="org-chart.png" alt="Sơ đồ tổ chức công ty" />
  <figcaption>Chi tiết: CEO → 3 VP → 12 Director → 48 Manager</figcaption>
</figure>

<!-- ✅ Icon buttons: aria-label -->
<button aria-label="Đóng dialog">
  <svg><!-- X icon --></svg>
</button>
```

#### Color Contrast (1.4.3 / 1.4.6)
```
Tỷ lệ tương phản tối thiểu:
- Văn bản thường: 4.5:1 (AA) hoặc 7:1 (AAA)
- Văn bản lớn (≥18pt hoặc ≥14pt bold): 3:1 (AA) hoặc 4.5:1 (AAA)
- UI components & graphical objects: 3:1 (AA)

Công cụ kiểm tra:
- Chrome DevTools → Elements → Styles → color swatch → Contrast ratio
- WebAIM Contrast Checker: https://webaim.org/resources/contrastchecker/
- axe DevTools extension
```

```css
/* ✅ Đảm bảo contrast ratio đủ */
:root {
  --text-primary: #1a1a2e;     /* trên nền trắng: ~16:1 ✅ */
  --text-secondary: #4a4a68;   /* trên nền trắng: ~7:1 ✅ */
  --text-on-primary: #ffffff;  /* trên nền #2563eb: ~5.2:1 ✅ */
}

/* ❌ Không chỉ dùng màu để truyền tải thông tin */
.error { color: red; }

/* ✅ Dùng màu + icon + text */
.error {
  color: var(--color-error);
  border-left: 3px solid var(--color-error);
}
.error::before { content: "⚠ "; }
```

#### Resize Text (1.4.4)
```css
/* ✅ Dùng rem/em, không dùng px cho font-size */
body { font-size: 1rem; }     /* 16px default */
h1 { font-size: 2.25rem; }    /* 36px */

/* ❌ TRÁNH: px cho font-size */
body { font-size: 14px; }
```

### 2. Operable (Thao tác được)

Giao diện phải thao tác được qua bàn phím và các thiết bị hỗ trợ.

#### Keyboard Navigation (2.1)
```css
/* ✅ Focus indicator rõ ràng */
:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}

/* ❌ KHÔNG BAO GIỜ ẩn focus hoàn toàn */
/* :focus { outline: none; } ← TUYỆT ĐỐI KHÔNG */
```

```html
<!-- ✅ Tất cả interactive elements phải focusable -->
<button>Xác nhận</button>              <!-- focusable mặc định -->
<a href="/about">Giới thiệu</a>        <!-- focusable mặc định -->

<!-- Custom interactive element cần tabindex -->
<div role="button" tabindex="0" 
     onkeydown="if(event.key==='Enter'||event.key===' ')handleClick()">
  Custom Button
</div>

<!-- Skip navigation link -->
<a href="#main-content" class="skip-link">
  Bỏ qua navigation
</a>
```

```css
.skip-link {
  position: absolute;
  left: -9999px;
  z-index: 999;
}
.skip-link:focus {
  left: 50%;
  transform: translateX(-50%);
  top: 0;
  padding: 8px 16px;
  background: var(--color-primary);
  color: white;
}
```

#### Focus Management cho Modals/Dialogs (2.4.3)
```javascript
// ✅ Trap focus trong modal
function trapFocus(modal) {
  const focusable = modal.querySelectorAll(
    'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
  );
  const firstEl = focusable[0];
  const lastEl = focusable[focusable.length - 1];

  modal.addEventListener('keydown', (e) => {
    if (e.key === 'Tab') {
      if (e.shiftKey && document.activeElement === firstEl) {
        e.preventDefault();
        lastEl.focus();
      } else if (!e.shiftKey && document.activeElement === lastEl) {
        e.preventDefault();
        firstEl.focus();
      }
    }
    if (e.key === 'Escape') closeModal();
  });

  firstEl.focus();
}
```

### 3. Understandable (Hiểu được)

Nội dung và giao diện phải dễ hiểu.

#### Form Labels & Errors (3.3)
```html
<!-- ✅ Labels liên kết với inputs -->
<label for="email">Email</label>
<input type="email" id="email" name="email" 
       required aria-required="true"
       aria-describedby="email-hint email-error" />
<span id="email-hint" class="hint">Ví dụ: name@company.com</span>
<span id="email-error" class="error" role="alert" aria-live="polite">
  <!-- Error messages injected here -->
</span>

<!-- ✅ Form validation messages rõ ràng -->
<script>
  function showError(inputId, message) {
    const errorEl = document.getElementById(`${inputId}-error`);
    errorEl.textContent = message;
    document.getElementById(inputId).setAttribute('aria-invalid', 'true');
  }
</script>
```

#### Language (3.1)
```html
<!-- ✅ Khai báo ngôn ngữ trang -->
<html lang="vi">

<!-- ✅ Đánh dấu đoạn text khác ngôn ngữ -->
<p>Xin chào! <span lang="en">Welcome to our platform.</span></p>
```

### 4. Robust (Bền vững)

Nội dung phải tương thích tốt với công nghệ hỗ trợ.

#### Semantic HTML
```html
<!-- ✅ Dùng semantic elements -->
<header><!-- site header --></header>
<nav aria-label="Main navigation"><!-- navigation --></nav>
<main id="main-content">
  <article>
    <h1>Tiêu đề bài viết</h1>
    <section aria-labelledby="section-1">
      <h2 id="section-1">Phần 1</h2>
    </section>
  </article>
  <aside aria-label="Related content"><!-- sidebar --></aside>
</main>
<footer><!-- site footer --></footer>

<!-- ❌ TRÁNH: div soup -->
<div class="header">
  <div class="nav">
    <div class="nav-item">...</div>
  </div>
</div>
```

#### ARIA Landmarks & Roles
```html
<!-- ARIA roles cho custom components -->
<div role="tablist" aria-label="Product tabs">
  <button role="tab" aria-selected="true" aria-controls="panel-1" id="tab-1">
    Mô tả
  </button>
  <button role="tab" aria-selected="false" aria-controls="panel-2" id="tab-2">
    Đánh giá
  </button>
</div>
<div role="tabpanel" aria-labelledby="tab-1" id="panel-1">
  Nội dung mô tả...
</div>
<div role="tabpanel" aria-labelledby="tab-2" id="panel-2" hidden>
  Nội dung đánh giá...
</div>

<!-- Live regions cho dynamic content -->
<div aria-live="polite" aria-atomic="true">
  <!-- Nội dung cập nhật được đọc bởi screen reader -->
</div>

<div role="status">Đang tải... 45%</div>
<div role="alert">Lỗi: Email không hợp lệ</div>
```

## Công Cụ Testing

### Automated Testing
```bash
# axe-core (npm)
npm install -D @axe-core/cli
npx axe http://localhost:3000

# Lighthouse CI
npx lighthouse http://localhost:3000 --only-categories=accessibility --output=json

# Pa11y
npx pa11y http://localhost:3000
```

### Playwright Accessibility Testing
```javascript
// Test a11y với @axe-core/playwright
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test('accessibility audit', async ({ page }) => {
  await page.goto('/');
  const results = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa', 'wcag21aa'])
    .analyze();
  expect(results.violations).toEqual([]);
});
```

### Manual Testing Checklist
- [ ] Tab through entire page — tất cả interactive elements reachable
- [ ] Screen reader test (NVDA/VoiceOver) — nội dung có nghĩa khi đọc
- [ ] Zoom 200% — layout không bể
- [ ] Zoom 400% — text vẫn đọc được
- [ ] High contrast mode — nội dung vẫn hiển thị
- [ ] Tắt CSS — nội dung vẫn có cấu trúc logic
- [ ] Tắt JavaScript — core content vẫn accessible
- [ ] Keyboard-only navigation — mọi tác vụ đều thực hiện được

## Audit Checklist Nhanh

### Perceivable
- [ ] Tất cả images có alt text (hoặc alt="" cho decorative)
- [ ] Video có captions/subtitles
- [ ] Color contrast ≥ 4.5:1 cho text thường
- [ ] Không dùng chỉ màu sắc để truyền tải thông tin
- [ ] Font size dùng rem/em, không px

### Operable
- [ ] Tất cả chức năng thao tác được bằng keyboard
- [ ] Focus indicator visible (:focus-visible)
- [ ] Skip navigation link
- [ ] No keyboard traps
- [ ] Focus management cho modals/dialogs

### Understandable
- [ ] `lang` attribute trên `<html>`
- [ ] Form inputs có labels liên kết
- [ ] Error messages rõ ràng, gợi ý cách sửa
- [ ] Consistent navigation across pages

### Robust
- [ ] Semantic HTML (header, nav, main, footer, article, section)
- [ ] ARIA roles đúng cho custom components
- [ ] Valid HTML (no duplicate IDs, proper nesting)
- [ ] Live regions cho dynamic content updates
