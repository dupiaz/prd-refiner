# SES-2026-0311-003: Skill Gap Closure & Master Workflow

**Ngày lên kế hoạch**: 2026-03-11  
**Phiên trước**: `5548683c` — Đánh giá quy trình web app & cài Docker skills  
**Trạng thái**: 🟡 Chờ thực hiện

---

## Bối Cảnh

Phiên trước đã đánh giá quy trình xây dựng web app từ ý tưởng thô → scale:
- **12 giai đoạn, 67 bước** — coverage hiện tại ~73%
- **UX/UI Design (30%)** là gap lớn nhất
- **Engineering** còn thiếu API design & CI/CD
- Đã cài 3 Docker skills, tổng hiện tại **174 skill directories**
- Cơ chế Auto/HITL đã thiết kế với 14 HITL checkpoints

Tham khảo chi tiết:
- Đánh giá quy trình: `brain/5548683c/.../web-app-lifecycle-assessment.md`
- Docker analysis: `brain/5548683c/.../docker-sandbox-impact-analysis.md`
- Engineering skills report: `docs/engineering/skills-installation-report.md`

---

## Prompt

```
Tiếp tục từ phiên trước (conversation 5548683c), chúng ta đã:
- Đánh giá quy trình web app 12 giai đoạn / 67 bước, coverage ~73%
- Xác định UX/UI Design là gap lớn nhất (30% coverage)
- Cài 3 Docker skills (docker-development, docker-devcontainer-windows, docker-production-deploy)
- Tổng hiện tại: 174 skill directories

Phiên này cần hoàn thành 4 nhiệm vụ:

1. **Bịt gap UX/UI (Priority 1)**: Tìm từ hub uy tín hoặc tạo mới 3 skills:
   - `ux-wireframe` — wireframing patterns, information architecture, user flows
   - `responsive-design` — mobile-first CSS, breakpoints, responsive patterns cho PC + Mobile
   - `accessibility-audit` — WCAG 2.1 compliance, aria labels, semantic HTML, color contrast

2. **Bổ sung Engineering gaps (Priority 2)**: Tìm hoặc tạo 2 skills:
   - `api-design` — REST best practices, OpenAPI/Swagger, versioning, error handling
   - `github-actions` — CI/CD pipeline templates: lint → test → build → scan → deploy

3. **Git sync**: Commit toàn bộ `.agents/skills/` mới lên GitHub (follow SOP tại `docs/ops/SOP-git-sync.md`)

4. **Tạo master workflow**: Viết `.agents/workflows/webapp-from-scratch.md` — workflow kết nối tất cả skills theo 12 giai đoạn, đánh dấu Auto/HITL cho mỗi bước, có thể invoke bằng `/webapp-from-scratch`

Mỗi skill mới cần: security scan + ghi nhận source (hub hay custom).
Ưu tiên dùng skills có sẵn từ hub uy tín trước, chỉ tạo custom khi không tìm được.

## Định nghĩa hoàn thành (Done Criteria)

Phiên này HOÀN THÀNH khi đạt đủ 5 tiêu chí sau:
1. **5 skills mới** cài/tạo, security scan passed → verify: `ls .agents/skills/ux-wireframe .agents/skills/responsive-design .agents/skills/accessibility-audit .agents/skills/api-design .agents/skills/github-actions`
2. **Tổng skills ≥ 179** → verify: `(Get-ChildItem -Directory .agents/skills).Count`
3. **Git synced** — tất cả push lên GitHub → verify: `git status` clean
4. **Master workflow** tại `.agents/workflows/webapp-from-scratch.md` → chứa 12 giai đoạn + skill mapping + Auto/HITL markers
5. **Coverage ≥ 80%** → báo cáo cập nhật trong `docs/engineering/`

## Lưu ý kỹ thuật
- `npx skills add` có thể fail trên Windows do interactive prompts → dùng manual clone + copy
- Kiểm tra `.gitignore` trước khi commit `.agents/skills/` — đảm bảo không bị exclude
- Master workflow cần test thử `/webapp-from-scratch` sau khi tạo
```
