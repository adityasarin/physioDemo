# Cloudflare Pages — Deployment Guide

## How It Works

This site is hosted on **Cloudflare Pages** and connected to the GitHub repo
`adityasarin/physioDemo`. Every push to the `main` branch triggers an automatic
deployment to `https://drwasimphysio.com/`.

---

## Step-by-Step: Push Changes to Production

### 1. Edit the website files

Make changes to any of the core files:

| File | What it controls |
|---|---|
| `index.html` | All page content, FAQ, structured data (JSON-LD) |
| `css/style.css` | All styles — colors, layout, typography |
| `js/main.js` | All interactivity — nav, accordion, scroll effects |
| `images/` | Photos and icons |

### 2. Stage the changed files

Only stage website files — never stage `.claude/`, `.playwright-mcp/`, `.zip` files, or PDFs.

```bash
# Stage specific files (preferred)
git add index.html
git add css/style.css
git add js/main.js

# Or stage everything (safe because .gitignore excludes tooling files)
git add .
```

### 3. Commit with a clear message

```bash
git commit -m "Brief description of what changed"
```

Examples of good commit messages:
- `Update review count to 750`
- `Add new FAQ entry about home visits`
- `Fix mobile nav close behavior`

### 4. Push to GitHub

```bash
git push origin main
```

### 5. Wait for Cloudflare Pages to deploy

- Cloudflare detects the push and starts a build (usually completes in **30–60 seconds**).
- Monitor progress at: **Cloudflare Dashboard → Pages → physioDemo → Deployments**
- Once the deployment shows "Success", visit `https://drwasimphysio.com/` and hard-refresh (`Ctrl+Shift+R`) to see the changes.

---

## Common Content Updates

### Update the review count

The review count appears in **three places** — keep all three in sync:

1. `index.html` — hero badge (search for `reviews`)
2. `index.html` — testimonials section heading
3. `index.html` — JSON-LD `"reviewCount"` field inside the `<script type="application/ld+json">` block

### Update opening hours

Hours appear in **two places**:

1. `index.html` — the visible schedule grid
2. `index.html` — JSON-LD `openingHoursSpecification` blocks

### Add / edit FAQ entries

FAQ entries must be updated in **two places**:

1. `index.html` — the visible accordion HTML
2. `index.html` — the JSON-LD `FAQPage` block (same `<script type="application/ld+json">`)

---

## Rollback a Bad Deployment

If a push breaks the live site, revert to the previous commit:

```bash
# View recent commits to find the good one
git log --oneline -10

# Revert the most recent commit (creates a new undo-commit, safe)
git revert HEAD

# Push the revert — Cloudflare redeploys automatically
git push origin main
```

---

## Check What Will Be Pushed

Before pushing, confirm you're only sending website files:

```bash
# See all changed files
git status

# See exact line-level changes
git diff

# See commits that haven't been pushed yet
git log origin/main..HEAD --oneline
```

---

## Quick Reference

| Task | Command |
|---|---|
| Check status | `git status` |
| Stage all changes | `git add .` |
| Commit | `git commit -m "your message"` |
| Push to Cloudflare | `git push origin main` |
| View deployment log | Cloudflare Dashboard → Pages |
| Hard-refresh browser | `Ctrl + Shift + R` |
