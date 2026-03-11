# CompeteRadar — Phase 4: Đánh Giá & Quyết Định Kiến Trúc Thư Mục

**Ngày**: 2026-03-11  
**So sánh**: Horizontal (Layer-based) vs Vertical Slice (Feature-based)  
**Skills sử dụng**: `brainstorming`  
**Tham chiếu**:
- [Horizontal version](file:///d:/github/prd-refiner/products/competeradar/docs/phase4-structure-horizontal.md)
- [Vertical version](file:///d:/github/prd-refiner/products/competeradar/docs/phase4-structure-vertical.md)

---

## 1. Bảng So Sánh Tổng Quan

| Tiêu chí | Horizontal | Vertical Slice | Winner |
|----------|:----------:|:--------------:|:------:|
| **Tổng số files** | ~95 | ~130 | 🏷️ Horizontal |
| **Folders cần mở cho 1 feature** | 4 | 1 (+1 shared) | 🎯 Vertical |
| **Onboarding time (dự kiến)** | 2-3 ngày | 1-2 ngày | 🎯 Vertical |
| **Xóa feature** | Tìm ở 4 nơi | Xóa 1 folder | 🎯 Vertical |
| **Quen thuộc (community)** | Rất phổ biến | Đang phổ biến | 🏷️ Horizontal |
| **Shared logic** | Tự nhiên | Cần `shared/` design | 🏷️ Horizontal |
| **AI agent context** | Phân tán | Tập trung | 🎯 Vertical |
| **Feature isolation** | Yếu | Mạnh (ESLint enforce) | 🎯 Vertical |
| **Scalability (30+ features)** | Folders phình | Folders tăng tuyến tính | 🎯 Vertical |

**Tổng**: Vertical **6** — Horizontal **3**

---

## 2. Đánh Giá Chi Tiết — Scoring Matrix

Thang điểm: 1 (kém) → 5 (xuất sắc), trọng số dựa trên bối cảnh CompeteRadar.

| Tiêu chí | Trọng số | Horizontal | Vertical | Lý do trọng số |
|----------|:--------:|:----------:|:--------:|----------------|
| **Developer Experience (DX)** | 25% | 3 | 5 | Solo founder, DX = tốc độ |
| **Onboarding** | 20% | 3 | 5 | Nhân sự mới — yêu cầu chính |
| **Maintainability** | 20% | 3 | 4 | MVP → scale, bảo trì lâu dài |
| **AI Agent Compatibility** | 15% | 2 | 5 | AI-assisted development |
| **Community / Ecosystem** | 10% | 5 | 3 | Tài liệu, Stack Overflow |
| **Setup Complexity** | 10% | 5 | 3 | ESLint rules, barrel exports |

**Điểm tổng:**

```
Horizontal:  3×0.25 + 3×0.20 + 3×0.20 + 2×0.15 + 5×0.10 + 5×0.10 = 3.25
Vertical:    5×0.25 + 5×0.20 + 4×0.20 + 5×0.15 + 3×0.10 + 3×0.10 = 4.40
```

| Kiến trúc | Điểm | Xếp hạng |
|-----------|:-----:|:--------:|
| **Vertical Slice** | **4.40 / 5.00** | 🥇 |
| Horizontal | 3.25 / 5.00 | 🥈 |

---

## 3. Phân Tích Rủi Ro

### Rủi ro nếu chọn Vertical Slice

| Rủi ro | Mức | Giảm thiểu |
|--------|:---:|-----------|
| `shared/` phình to → biến thành horizontal disguised | Trung bình | Quy tắc: chỉ đưa vào shared khi ≥ 2 features dùng |
| Dev mới chưa quen vertical | Thấp | Tài liệu methodology + folder README |
| Barrel exports overhead | Thấp | Template chuẩn, IDE auto-import |
| File count nhiều hơn (~35 files) | Thấp | Files nhỏ hơn, dễ quản lý hơn |

### Rủi ro nếu chọn Horizontal

| Rủi ro | Mức | Giảm thiểu |
|--------|:---:|-----------|
| Feature scatter → bỏ sót khi sửa/xóa | Cao | Code review kỹ |
| `lib/hooks/` và `lib/schemas/` phình | Trung bình | Chia subfolder theo feature (=quasi-vertical) |
| Circular dependency | Trung bình | ESLint import rules |
| Cognitive load cao cho AI agent | Trung bình | Cung cấp context dọc dài hơn |

---

## 4. Kiểm Chứng Thực Tế — 3 Kịch Bản

### Kịch bản A: "Thêm feature Export CSV cho Changes"

| Bước | Horizontal | Vertical |
|------|-----------|----------|
| 1. Tạo API handler | `src/app/api/v1/competitors/[id]/changes/export/route.ts` | `src/features/scanning/api/export-changes.ts` |
| 2. Thêm Zod schema | `src/lib/schemas/changes.ts` | `src/features/scanning/schemas.ts` |
| 3. Thêm button UI | `src/components/scanning/export-button.tsx` | `src/features/scanning/components/export-button.tsx` |
| 4. Thêm hook | `src/lib/hooks/use-export-changes.ts` | `src/features/scanning/hooks/use-export.ts` |
| **Folders affected** | **4** | **1** (+1 route file) |

### Kịch bản B: "Xóa toàn bộ feature Battlecards"

| Bước | Horizontal | Vertical |
|------|-----------|----------|
| 1. Xóa pages | `app/(dashboard)/battlecards/` | `app/(dashboard)/battlecards/` |
| 2. Xóa API | `app/api/v1/battlecards/` | `app/api/v1/battlecards/` |
| 3. Xóa components | `components/battlecard/` (3 files) | — (đã xóa ở bước 4) |
| 4. Xóa logic | `lib/hooks/use-battlecards.ts` + `lib/schemas/battlecard.ts` + `lib/types/battlecard.ts` + `lib/db/queries/battlecards.ts` | `features/battlecards/` ← **xóa 1 folder** |
| 5. Kiểm tra sót | Grep toàn dự án | Grep chỉ `@/features/battlecards` |
| **Rủi ro bỏ sót** | **Cao** (4 folders) | **Thấp** (1 folder + routes) |

### Kịch bản C: "Nhân sự mới vào dự án, cần làm bug fix trên Digest"

| Bước | Horizontal | Vertical |
|------|-----------|----------|
| 1. Hiểu cấu trúc | Đọc README → hiểu 4 layers | Đọc README → mở `features/digest/` |
| 2. Tìm code liên quan | Tìm ở `components/digest/`, `lib/hooks/`, `lib/schemas/`, `lib/db/queries/`, `app/api/v1/digests/` | Mở `features/digest/` → thấy tất cả |
| 3. Fix bug | Edit 2-3 files ở 2-3 folders | Edit 2-3 files trong 1 folder |
| **Cognitive load** | **Cao** — cần hiểu project structure | **Thấp** — chỉ cần hiểu 1 folder |

---

## 5. Đặt Bối Cảnh CompeteRadar

| Yếu tố | Thực tế | Ảnh hưởng |
|---------|---------|-----------|
| **Team size** | Solo founder + AI agent | → Cần DX tốt, context tập trung |
| **Feature domains** | 7-8 features rõ ràng | → Vertical slices tự nhiên |
| **Next.js App Router** | `app/` đã vertical | → Horizontal ở `lib/` tạo mâu thuẫn |
| **Giai đoạn** | MVP → Product-Market Fit | → Cần iterate nhanh, xóa/thêm feature dễ |
| **AI-assisted dev** | Dùng AI agent coding | → Vertical giúp giới hạn context window |
| **Mục tiêu nhân sự** | Onboard nhanh | → Vertical giảm learning curve |

---

## 6. Quyết Định

> [!IMPORTANT]
> **Chọn: Vertical Slice Architecture** cho CompeteRadar.

### Lý do chính:
1. **DX + AI = tốc độ** — Solo founder dùng AI agent, cần context tập trung per feature
2. **Onboarding** — Mục tiêu sư phạm rõ ràng: nhân sự mới vào 1 folder = hiểu 1 feature
3. **Iterate nhanh** — MVP giai đoạn cần thêm/xóa features linh hoạt
4. **Next.js alignment** — `app/` đã vertical, `features/` + `shared/` nhất quán

### Khi nào xem xét lại:
- Nếu team > 10 người và chia FE/BE teams riêng
- Nếu chuyển sang microservices architecture
- Nếu `shared/` chiếm > 40% tổng code → cân nhắc hybrid

### Hành động tiếp theo:
1. ✅ Phase 5 sẽ scaffold theo vertical slice structure
2. ✅ Cấu hình ESLint import boundaries
3. ✅ Tạo folder README template cho mỗi feature slice
4. ✅ Cập nhật `tsconfig.json` path aliases

---

## 7. Appendix — Tham Khảo

| Nguồn | Liên kết |
|-------|---------|
| Vertical Slice Architecture by Jimmy Bogard | https://www.jimmybogard.com/vertical-slice-architecture/ |
| Feature-Sliced Design | https://feature-sliced.design/ |
| Next.js Project Structure (Vercel) | https://nextjs.org/docs/getting-started/project-structure |
| Bulletproof React (Feature-based) | https://github.com/alan2207/bulletproof-react |

---

*Phase 4 — Architecture Decision Record v1.0*
