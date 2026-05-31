# APX Global — Deployment Notes

## Site Overview
Single-page website for APX Global (a trade name of Africa ProjectX Ltd).
File: `index.html` — self-contained, no build step, no dependencies to install.
Live URL: https://apx-global.net (custom domain)
Fallback URL: https://dwehsidi.github.io/apx/

---

## Repository
- GitHub repo: https://github.com/Dwehsidi/apx.git
- Branch: `main`
- GitHub Pages source: `main` branch, root `/`
- The `.nojekyll` file at root tells GitHub Pages to skip Jekyll processing.

---

## How to Push Changes

1. Edit `index.html` (the working file is `apx-global-v4.html` — copy it to `index.html` after edits).
2. Stage and commit:
   ```
   git add index.html
   git commit -m "your message"
   ```
3. Push using a GitHub Personal Access Token for the **Dwehsidi** account:
   ```
   git push https://Dwehsidi:<TOKEN>@github.com/Dwehsidi/apx.git main
   ```
   Get a token at: https://github.com/settings/tokens (needs `repo` scope).

> Note: The machine's default git credentials are for a different account (Alkebuleum).
> Always push with an explicit token for Dwehsidi or the push will be rejected with 403.

---

## First-Time Setup (if repo is missing .git)

```
git init
git checkout -b main
git remote add origin https://github.com/Dwehsidi/apx.git
git add index.html .gitignore .nojekyll
git commit -m "Launch APX Global website"
git push https://Dwehsidi:<TOKEN>@github.com/Dwehsidi/apx.git main
```

---

## Enabling GitHub Pages (if ever disabled or on a new repo)

Use the GitHub API — no gh CLI needed:

```powershell
$headers = @{ Authorization = "token <TOKEN>"; Accept = "application/vnd.github+json" }
$body = @{ source = @{ branch = "main"; path = "/" } } | ConvertTo-Json
Invoke-RestMethod -Uri "https://api.github.com/repos/Dwehsidi/apx/pages" -Method POST -Headers $headers -Body $body -ContentType "application/json"
```

---

## Custom Domain (apx-global.net via Namecheap)

The `CNAME` file in the repo contains `apx-global.net`. GitHub Pages reads this automatically.

DNS records to set in **Namecheap → Advanced DNS**:

### A Records (Host: `@`)
| Type | Host | Value |
|------|------|-------|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

### CNAME Record
| Type | Host | Value |
|------|------|-------|
| CNAME | www | dwehsidi.github.io. |

After DNS propagates (10–30 min, up to 48h), GitHub auto-provisions HTTPS.
Check status: https://github.com/Dwehsidi/apx/settings/pages

---

## Files in This Repo

| File | Purpose |
|------|---------|
| `index.html` | The website (deploy this) |
| `CNAME` | Custom domain — contains `apx-global.net` |
| `apx-global-v4.html` | Working/source copy — gitignored |
| `.nojekyll` | Prevents Jekyll processing on GitHub Pages |
| `.gitignore` | Excludes `*.pdf` and `apx-global-v4.html` from git |
| `Note.md` | This file |

---

## Important
- Do NOT commit `ARTICLES OF AMENDMENT.pdf` or any `.pdf` — it's a private legal document and is gitignored.
- Do NOT commit `token.md` or any file containing a GitHub token.
- After pushing with a token, delete any local file that held it.
