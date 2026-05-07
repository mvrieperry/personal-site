# Marie's Personal Site — How It Works

Quick reference for editing and deploying this site.

**Live site:** https://marie-perry.pages.dev

## Stack
- Built on the [Starfolio](https://github.com/webrating/starfolio) Astro template
- Hosted on **Cloudflare Pages** (auto-deploys from this repo)
- Source on GitHub: `mvrieperry/marie-perry-site`

---

## Editing workflow

All my content lives in **three files** — I never need to touch components.

| File | What lives here |
|---|---|
| `src/data/resume.tsx` | Name, bio, work, education, skills, social links, photos array |
| `src/data/config.ts` | Site URL, theme colors, font size, SEO |
| `src/styles/global.css` | Font imports |

### Steps for any change
```bash
# 1. Make sure I'm on master and up to date
git pull

# 2. Edit files locally (use any editor, or just claude code)

# 3. Preview locally before pushing
pnpm dev
# → opens http://localhost:4321

# 4. Commit and push
git add .
git commit -m "describe what I changed"
git push
```

That's it — Cloudflare picks up the push within ~30 seconds and rebuilds.

---

## Where to update common things

| To change... | Edit |
|---|---|
| My name, bio, summary | `src/data/resume.tsx` (top of the `DATA` object) |
| Add a new job | `src/data/resume.tsx` → `work: [...]` array |
| Add education / certs | `src/data/resume.tsx` → `education: [...]` array |
| Add / remove skills | `src/data/resume.tsx` → `skills: [...]` array |
| Avatar photo | Replace `public/avatar.jpg` |
| Travel photos | Replace files in `public/photos/photoN.jpg` |
| Theme colors | `src/data/config.ts` → `theme.light` / `theme.dark` (use [tweakcn.com](https://tweakcn.com)) |
| Favicon | Edit `public/favicon.svg` |
| Social links | `src/data/resume.tsx` → `contact.social` |

---

## Cloudflare deployment workflow

### How it works
1. I push to `master` on GitHub
2. Cloudflare Pages detects the push
3. It runs `pnpm install` then `pnpm build` in their CI
4. The built `dist/` folder is deployed
5. Live within ~2 minutes at `marie-perry.pages.dev`

### Build settings (already configured)
- **Framework preset:** Astro
- **Build command:** `pnpm build`
- **Build output directory:** `dist`
- **Environment variable:** `NODE_VERSION = 22`

### If a deploy fails
1. Cloudflare dashboard → my project → **Deployments** tab
2. Click the failed deployment → **View build log**
3. Read the error in the build log
4. Fix locally → commit → push → it'll retry automatically

### Rolling back
If a deploy goes live and something looks broken:
1. Cloudflare dashboard → **Deployments**
2. Find the last working deployment → **⋯** menu → **Rollback to this deployment**

---

## Local dev commands

| Command | What it does |
|---|---|
| `pnpm install` | Install dependencies (run once after cloning, or after pulling changes) |
| `pnpm dev` | Run the dev server with hot reload at localhost:4321 |
| `pnpm build` | Build for production (output to `dist/`) — useful for testing the build before pushing |
| `pnpm preview` | Serve the production build locally |

---

## Gotchas / things I've hit
- **Node version matters** — both locally and on Cloudflare, need Node 22+ for pnpm 11
- **Photos go in `public/photos/`** — anything in `public/` is served as-is at the matching URL (e.g. `public/avatar.jpg` → `/avatar.jpg`)
- **Don't commit the resume PDF** — it's in `src/data/` but stays untracked on purpose
- **Cloudflare URL is locked at project creation** — to change `.pages.dev` subdomain, have to delete and recreate the Pages project
