# SES-2026-0311-004: [Tên phiên]

**Ngày lên kế hoạch**: 2026-03-11  
**Phiên trước**: `9de88e5c` — Skill Gap Closure & Master Workflow  
**Trạng thái**: 🟡 Chờ thực hiện

---

## Bối Cảnh

Phiên trước (SES-003) đã hoàn thành:
- Tạo 5 skills mới: `ux-wireframe`, `responsive-design`, `accessibility-audit`, `api-design`, `github-actions`
- Tạo master workflow `webapp-from-scratch.md` (12 phases, 67 bước, 14 HITL checkpoints)
- Coverage: 73% → **84%**, tổng **179 skills**
- Git synced: commit `e9a3bd6`

Tham khảo chi tiết:
- Session log: `docs/sessions/SES-2026-0311-003_skill-gap-closure.md`
- Coverage report: `docs/engineering/coverage-report-ses003.md`
- Master workflow: `.agents/workflows/webapp-from-scratch.md`

---

## Prompt Gợi Ý

Chọn 1 trong 3 hướng sau:

### Hướng A: Test Workflow (Khuyến nghị ⭐)

```
Tiếp tục từ phiên trước (conversation 9de88e5c), chúng ta đã:
- Tạo 5 skills mới, tổng 179 skills, coverage 84%
- Tạo master workflow webapp-from-scratch.md (12 phases)

Phiên này: Test thử workflow bằng cách chạy Phase 1-3 cho 1 ý tưởng app thực tế.
- Phase 1: Ideation — brainstorm & validate 1 Micro SaaS idea
- Phase 2: Product Planning — viết PRD
- Phase 3: UX/UI Design — wireframes + responsive strategy

Mục tiêu: Validate workflow hoạt động end-to-end, phát hiện gaps/improvements.
```

### Hướng B: Build App Đầu Tiên

```
Tiếp tục từ phiên trước (conversation 9de88e5c), chúng ta đã:
- Tạo 5 skills mới, tổng 179 skills, coverage 84%
- Tạo master workflow webapp-from-scratch.md (12 phases)

Phiên này: Bắt đầu build [tên app] theo /webapp-from-scratch workflow.
[Mô tả ý tưởng app ở đây]
```

### Hướng C: Tiếp Tục Bổ Sung Skills

```
Tiếp tục từ phiên trước (conversation 9de88e5c), chúng ta đã:
- Tạo 5 skills mới, tổng 179 skills, coverage 84%

Phiên này: Nâng coverage lên 90%+ bằng cách bổ sung:
1. Full-stack scaffold template (Next.js + tRPC + Prisma boilerplate)
2. Monitoring skill (PostHog/Mixpanel analytics integration)
3. [skill khác nếu cần]
```

---

*Chuẩn bị cho phiên tiếp theo — Tạo: 2026-03-11*
