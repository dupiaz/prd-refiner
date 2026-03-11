# 🔒 Security Audit Report: Skills Directory

**Date:** 2026-03-11 | **Scope:** 153 skills in `.agents/skills/` | **Method:** Static analysis (read-only)

---

## Executive Summary

> [!TIP]
> **Overall Risk: LOW** — The vast majority of skills (146/153) are markdown-only instruction files with no executable code. No prompt injection, data exfiltration, or malicious patterns were detected.

| Metric | Count |
|---|---|
| Total skills scanned | 153 |
| Markdown-only skills | 146 |
| Skills with executable files | 3 |
| Total executable files found | 7 |
| Critical issues | 0 |
| Medium issues | 2 |
| Low issues | 1 |

---

## Scan Methodology

Searched all 153 skill directories for:

1. **Executable files** — `.py`, `.sh`, `.ps1`, `.bat`, `.cmd`, `.js`, `.ts`, `.zip`, `.exe`
2. **Dangerous function calls** — `eval()`, `exec()`, `subprocess`, `os.system()`, `os.remove()`, `shutil.rmtree()`
3. **Network calls** — `curl`, `wget`, `fetch()`, `requests.get/post`, `Invoke-WebRequest`
4. **Destructive operations** — `rm -rf`, `del /`, `rmdir`
5. **Credential patterns** — `api_key`, `secret`, `password`, `token`
6. **Prompt injection** — `ignore previous`, `disregard`, `forget your instructions`, `you are now`
7. **Data obfuscation** — `base64`, `encode`, `obfuscate`, `encrypt`

---

## Findings by Risk Level

### 🟡 MEDIUM — `deploy-to-vercel` (External API Calls)

| File | Risk |
|---|---|
| [deploy.sh](file:///d:/github/prd-refiner/.agents/skills/deploy-to-vercel/resources/deploy.sh) | Sends project tarball to `claude-skills-deploy.vercel.com` |
| [deploy-codex.sh](file:///d:/github/prd-refiner/.agents/skills/deploy-to-vercel/resources/deploy-codex.sh) | Same as above (codex variant) |
| `Archive.zip` | Unverified archive — contents unknown |

**Details:**
- Line 9: Hardcoded endpoint `https://claude-skills-deploy.vercel.com/api/deploy`
- Line 239: `curl -s -X POST "$DEPLOY_ENDPOINT" -F "file=@$TARBALL"` — uploads your project code
- Line 175: `rm -rf "$TEMP_DIR"` — deletes temp directory (normal cleanup, scoped to temp)
- Line 204: Excludes `.env` files from tarball ✅ (good practice)

> [!WARNING]
> These scripts **upload your entire project source code** (excluding `node_modules`, `.git`, `.env`) to a third-party Vercel deployment endpoint. Only use if you trust this service and intend to deploy.

**Recommendation:** Verify the `claude-skills-deploy.vercel.com` endpoint origin. Consider whether you want project code sent to this service.

---

### 🟢 LOW — `content-creator` (Safe Python Scripts)

| File | Risk |
|---|---|
| [brand_voice_analyzer.py](file:///d:/github/prd-refiner/.agents/skills/content-creator/scripts/brand_voice_analyzer.py) | ✅ Safe — offline text analysis |
| [seo_optimizer.py](file:///d:/github/prd-refiner/.agents/skills/content-creator/scripts/seo_optimizer.py) | ✅ Safe — offline text analysis |

**Details:** Pure Python scripts using only `re`, `json`, `typing` stdlib modules. Read local files, analyze text, output results. No network calls, no file writes (except stdout), no dangerous imports.

---

### 🟢 LOW — `product-manager-toolkit` (Safe Python Scripts)

| File | Risk |
|---|---|
| [rice_prioritizer.py](file:///d:/github/prd-refiner/.agents/skills/product-manager-toolkit/scripts/rice_prioritizer.py) | ✅ Safe — offline calculation |
| [customer_interview_analyzer.py](file:///d:/github/prd-refiner/.agents/skills/product-manager-toolkit/scripts/customer_interview_analyzer.py) | ✅ Safe — offline text analysis |

**Details:** Pure Python scripts using `re`, `json`, `csv`, `argparse`, `collections` stdlib. RICE prioritizer can write a sample CSV file (explicit user action only). No network calls, no dangerous operations.

---

## Scan Results Summary

### ✅ No Issues Found

| Check | Result |
|---|---|
| `eval()` / `exec()` calls | **None found** |
| `subprocess` / `os.system()` calls | **None found** (only mentioned in docs) |
| Prompt injection patterns | **None found** |
| Base64/encoding obfuscation | **None found** |
| Hardcoded credentials | **None found** |
| `.exe` files | **None found** |
| PowerShell scripts (`.ps1`) | **None found** |

### Auth/credential keyword matches

All 50+ matches for `auth`/`token`/`credential` are **benign code examples** in `vercel-react-best-practices` (React authentication patterns) and documentation references in `context-engine`, not actual credentials.

---

## Recommendations

| Priority | Action | Skill |
|---|---|---|
| 🟡 Medium | Review whether `deploy-to-vercel` endpoint is trusted before use | `deploy-to-vercel` |
| 🟡 Medium | Inspect `Archive.zip` contents before extracting | `deploy-to-vercel` |
| 🟢 Low | No action needed — safe offline scripts | `content-creator` |
| 🟢 Low | No action needed — safe offline scripts | `product-manager-toolkit` |

---

## Conclusion

The 153 skills are **predominantly safe markdown instruction files** that guide AI agent behavior. Only 3 skills contain executable code, and of those, only `deploy-to-vercel` makes external network calls. No prompt injection, malicious code, or data exfiltration patterns were detected.

**The main risk is `deploy-to-vercel`** which uploads project source to a Vercel deployment service — this is expected behavior for a deploy skill but should be used intentionally.
