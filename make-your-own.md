# How to Build a Personal Site Like This One

A step-by-step SOP for spinning up your own portfolio site using the [Starfolio](https://github.com/webrating/starfolio) Astro template and deploying it free on Cloudflare Pages.

**Time required:** ~30 minutes for a basic site, plus however long you want to spend writing your bio and picking photos.

---

## Before you start

Make sure you have:
- A **GitHub account**
- A **Cloudflare account** (free tier is plenty, no credit card needed)
- **Node.js 22+** installed locally ([download](https://nodejs.org/))
- **pnpm** installed: `npm install -g pnpm`
- A code editor (VS Code, Cursor, or whatever you like)
- Optional but useful: **Claude Code** or another AI coding assistant if you want help editing files

---

## Step 1. Get the code

Open a terminal and clone the template into a folder on your Desktop:

```bash
cd ~/Desktop
git clone https://github.com/webrating/starfolio my-site
cd my-site
pnpm install
pnpm dev
```

A dev server starts at `http://localhost:4321`. Open it in your browser. You'll see the template with a fake person named Alex Mercer. That's what you'll replace.

---

## Step 2. Customize three files

Almost everything you'll change lives in these three files. **You never need to edit components.**

### a) `src/data/resume.tsx`
Your name, bio, work history, education, skills, photos array, social links, projects.

Open it. Find the `DATA` object at the top. Replace each field with your info:
- `name`, `initials`, `description`, `summary`
- `work: [...]` (your jobs)
- `education: [...]` (your degrees)
- `skills: [...]` (your skills, with optional icons)
- `contact: { email, social }` (your links)
- `projects: [...]` (your projects)
- `photos: [...]` (paths to your photos)

Set `enabled: true` or `false` on any section under `sections: { ... }` to show or hide it.

### b) `src/data/config.ts`
Site URL, theme colors, font size, SEO meta.

The most fun part is theme colors:
1. Go to [tweakcn.com](https://tweakcn.com)
2. Pick or design a theme you like
3. Click **Copy** in the Theme Code panel (use **Tailwind v3** or **v4** format, either works)
4. Paste the values into the `light` and `dark` blocks in `config.ts`, stripping `--` and camelCasing names (e.g. `--card-foreground` becomes `cardForeground`)

### c) `src/styles/global.css`
Fonts. The default is Outfit + Geist Mono. To change:
1. Pick a variable font at [fontsource.org](https://fontsource.org/?variable=true)
2. Install it: `pnpm add @fontsource-variable/<font-name>`
3. Update the `@import` line and `--font-sans` value in `global.css`

---

## Step 3. Replace the placeholder assets

Drop your own files into the `public/` folder:

| Replace | With |
|---|---|
| `public/picofme.png` | Your headshot. Save as `public/avatar.jpg`, then update `avatarUrl: "/avatar.jpg"` in `resume.tsx` |
| `public/photos/photo1.jpg` to `photo9.jpg` | Your travel or hobby photos |
| `public/favicon.svg` | Edit the SVG to show your initials or a custom logo |
| `public/og_image.png` | The image that shows when your site is shared on social media (1200x630 px is ideal) |

Anything in `public/` is served at the matching URL: `public/avatar.jpg` becomes `/avatar.jpg` on your site.

---

## Step 4. Test locally

With `pnpm dev` running, refresh your browser. Click around, read everything, fix typos. When it looks right, do a production build to catch errors:

```bash
pnpm build
```

If that finishes without errors, you're ready to deploy.

---

## Step 5. Push to GitHub

### Create a new GitHub repo
1. Go to [github.com/new](https://github.com/new)
2. Pick a name. **Tip:** the GitHub repo name does not affect your site URL, so just pick something descriptive (e.g. `personal-site`, `my-portfolio`)
3. Public, no README, no .gitignore, no license
4. Click **Create repository**

### Connect your local repo to GitHub
The template clone is pointing at the original Starfolio repo. Switch it to yours:

```bash
git remote set-url origin https://github.com/<your-username>/<your-repo-name>
git add .
git commit -m "Personalise template"
git push -u origin master
```

---

## Step 6. Deploy to Cloudflare Pages

### Create the project
1. Sign up at [pages.cloudflare.com](https://pages.cloudflare.com) (free, no credit card)
2. Connect your GitHub account
3. **Important:** if Cloudflare drops you into the Workers UI, scroll to the bottom and click **"Looking to deploy Pages? Get started"**. Then click **Get started** next to **"Import an existing Git repository"**
4. Select your repo

### Configure the build
Fill in these values exactly:

| Field | Value |
|---|---|
| **Project name** | This becomes your URL: `<project-name>.pages.dev`. Pick wisely, it can't be changed without recreating the project. |
| **Production branch** | `master` |
| **Framework preset** | **Astro** |
| **Build command** | `pnpm build` |
| **Build output directory** | `dist` |
| **Root directory** | (leave empty) |

### Add the environment variable
This is the one thing that almost everyone misses and it will make your build fail:

Click **Environment variables (advanced)** and add:

| Variable | Value |
|---|---|
| `NODE_VERSION` | `22` |

Without this, Cloudflare uses an older Node version and pnpm 11 will fail.

### Deploy
Click **Save and Deploy**. Cloudflare runs `pnpm install` and `pnpm build`. Takes ~2 minutes. Your site goes live at `https://<project-name>.pages.dev`.

---

## Step 7. Future updates

From now on, updating your site is just three commands:

```bash
git add .
git commit -m "what I changed"
git push
```

Cloudflare detects the push within ~30 seconds and redeploys automatically.

---

## Gotchas worth knowing

**File size limit.** Cloudflare Pages rejects any file over 25MB. If you add a video, compress it first:
```bash
brew install ffmpeg
ffmpeg -i input.mp4 -vf "scale='min(720,iw)':-2" -crf 28 -preset slow -movflags +faststart output.mp4
```
This usually shrinks a phone screen recording from ~30MB down to under 2MB with no visible quality loss.

**Cloudflare URL is locked at project creation.** If you want a different `.pages.dev` subdomain, you have to delete the Cloudflare project and recreate it with a new name. The GitHub repo name is independent.

**Don't commit secrets.** Don't put API keys, passwords, or personal documents (resume PDFs, etc.) in the repo. Use a `.gitignore` to keep them out.

**Custom domain.** When you're ready to use a real domain instead of `.pages.dev`, buy one (Cloudflare sells them at cost) and add it under your project's **Custom domains** tab. Cloudflare handles SSL automatically.

---

## Troubleshooting

**Build fails with `pnpm: command not found`** → make sure your project name uses pnpm and `NODE_VERSION` env var is set to 22.

**Build fails with `ERR_PNPM_IGNORED_BUILDS`** → this is a pnpm security feature. The Cloudflare build environment usually handles it fine; if it doesn't, add `onlyBuiltDependencies: ["esbuild", "sharp"]` to the `pnpm` block in your `package.json`.

**Video doesn't autoplay** → make sure it's muted. Browsers block autoplay with sound. The project card component already handles this.

**Theme looks wrong** → the `oklch()` color values need to be valid. If you copied a Tailwind v4 theme, the values should drop into the `light` / `dark` blocks unchanged. Don't include the `--` prefix.
