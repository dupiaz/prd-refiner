# SOP: Đồng Bộ Dự Án với GitHub

> **SOP ID**: `SOP-GIT-SYNC-001`  
> **Version**: 1.0  
> **Ngày tạo**: 2026-03-11  
> **Cập nhật lần cuối**: 2026-03-11  
> **Tác giả**: dungn  
> **Áp dụng cho**: Tất cả dự án trong `d:\github\`

---

## Mục Đích

Quy trình chuẩn để đồng bộ dự án từ local lên GitHub — bao gồm khởi tạo lần đầu, commit thường ngày, và xử lý sự cố.

---

## Điều Kiện Tiên Quyết

| Yêu cầu | Kiểm tra | Cài đặt |
|----------|----------|---------|
| Git | `git --version` | [git-scm.com](https://git-scm.com) |
| GitHub CLI | `gh --version` | `winget install GitHub.cli` |
| GitHub Auth | `gh auth status` | `gh auth login` |

---

## Quy Trình A — Khởi Tạo Repo Lần Đầu

> Dùng khi dự án **chưa có** `.git/`

### Bước 1: Tạo .gitignore

Tạo file `.gitignore` ở root dự án với nội dung tối thiểu:

```gitignore
# Dependencies
node_modules/

# Environment
.env
.env.local
.env.*.local

# AI Agent data (private)
.gemini/

# IDE-specific
.windsurf/
.roo/
.vscode/
.idea/

# OS files
.DS_Store
Thumbs.db

# Build output
dist/
build/
out/

# Logs
*.log
```

> [!IMPORTANT]
> `.agents/skills/` **KHÔNG** được ignore — đây là nội dung cốt lõi của dự án cần version control.

### Bước 2: Khởi tạo Git

```powershell
git init
```

### Bước 3: Stage & Commit

```powershell
git add -A
git commit -m "Initial commit: <mô tả ngắn về dự án>"
```

### Bước 4: Tạo repo GitHub + Push

```powershell
# Public repo
gh repo create <tên-repo> --public --source=. --remote=origin --push --description "<mô tả>"

# Hoặc Private repo
gh repo create <tên-repo> --private --source=. --remote=origin --push --description "<mô tả>"
```

### Bước 5: Xác nhận

```powershell
git remote -v
# Expected: origin  https://github.com/dupiaz/<tên-repo>.git (fetch/push)
```

---

## Quy Trình B — Commit & Push Thường Ngày

> Dùng khi repo đã có trên GitHub, cần cập nhật thay đổi mới.

### Bước 1: Kiểm tra thay đổi

```powershell
git status
```

### Bước 2: Stage thay đổi

```powershell
# Stage tất cả
git add -A

# Hoặc stage file cụ thể
git add <đường-dẫn-file>
```

### Bước 3: Commit với message rõ ràng

```powershell
git commit -m "<type>: <mô tả ngắn>"
```

**Commit message convention:**

| Prefix | Dùng khi |
|--------|----------|
| `feat:` | Thêm tính năng mới |
| `fix:` | Sửa lỗi |
| `docs:` | Thêm/sửa tài liệu |
| `style:` | Thay đổi format, không ảnh hưởng logic |
| `refactor:` | Tái cấu trúc code |
| `chore:` | Cập nhật config, dependencies |
| `skill:` | Thêm/cập nhật skills |

**Ví dụ:**
```powershell
git commit -m "docs: add session log SES-002"
git commit -m "skill: add playwright-testing skill"
git commit -m "feat: implement auth flow with Better Auth"
```

### Bước 4: Push

```powershell
git push
```

> [!TIP]
> Trên PowerShell, dùng `;` thay vì `&&` để nối lệnh:
> ```powershell
> git add -A; git commit -m "docs: update SOP"; git push
> ```

---

## Quy Trình C — Pull Thay Đổi Từ Remote

> Dùng khi có thay đổi trên GitHub mà local chưa có.

```powershell
git pull origin main
```

---

## Quy Trình D — Làm Việc Với Branch

> Dùng khi phát triển tính năng mới hoặc thử nghiệm.

### Tạo branch mới

```powershell
git checkout -b feature/<tên-feature>
```

### Push branch lên GitHub

```powershell
git push -u origin feature/<tên-feature>
```

### Tạo Pull Request

```powershell
gh pr create --title "<tiêu đề>" --body "<mô tả>"
```

### Merge PR (sau khi review)

```powershell
gh pr merge <PR-number> --merge
```

---

## Xử Lý Sự Cố

### Lỗi: `fatal: not a git repository`
→ Chưa khởi tạo Git. Chạy **Quy Trình A** từ đầu.

### Lỗi: `gh auth` chưa đăng nhập
```powershell
gh auth login
# Chọn: GitHub.com → HTTPS → Login with a web browser
```

### Lỗi: Push bị reject (remote có thay đổi mới hơn)
```powershell
git pull --rebase origin main
git push
```

### Lỗi: Commit nhầm file nhạy cảm (.env, keys)
```powershell
# Xóa file khỏi Git history (giữ file local)
git rm --cached <file>
# Thêm vào .gitignore
echo "<file>" >> .gitignore
git add .gitignore
git commit -m "chore: remove sensitive file from tracking"
git push
```

> [!CAUTION]
> Nếu đã push `.env` hoặc credentials lên GitHub, hãy **rotate keys ngay lập tức** vì Git history vẫn giữ lại nội dung cũ.

### Lỗi: PowerShell không nhận `&&`
→ Dùng `;` thay cho `&&` trên PowerShell. Chi tiết ở Quy Trình B, Bước 4.

---

## Checklist Nhanh

```
□ .gitignore đã có và đúng?
□ Không commit file nhạy cảm (.env, keys, .gemini/)?
□ Commit message rõ ràng, đúng convention?
□ Push thành công, kiểm tra trên GitHub?
□ Session log đã cập nhật (nếu có thay đổi quan trọng)?
```

---

## Tài Liệu Liên Quan

- [Session Log: GitHub Sync](file:///d:/github/prd-refiner/docs/sessions/SES-2026-0311-002_github-sync.md)
- [.gitignore](file:///d:/github/prd-refiner/.gitignore)

---

*SOP-GIT-SYNC-001 v1.0 — Cập nhật: 2026-03-11*
