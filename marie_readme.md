# About This Site

This is the source code for my personal site, [marie-perry.pages.dev](https://marie-perry.pages.dev).

## What's on it

A short bio, my work history, a few side projects (a garden planner and an AI chatbot called The Know-It-All Postdoc), some travel photos shot on film, and a note about my DJ habit.

## How it was built

The site started from the open-source [Starfolio](https://github.com/webrating/starfolio) Astro template. Almost everything I changed lives in two files:

- `src/data/resume.tsx` for the actual content (name, bio, work, projects, etc.)
- `src/data/config.ts` for theme colors, fonts, and SEO

Theme colors were picked using [tweakcn.com](https://tweakcn.com). Font is Geist from [Fontsource](https://fontsource.org).

The build is plain Astro, plus React for a few interactive bits, Tailwind CSS v4, and shadcn/ui components. Output is a fully static site, so it loads fast and ships almost no JavaScript.

## How it deploys

Hosted on **Cloudflare Pages** (free tier). Every time I push code to the `master` branch on GitHub, Cloudflare:

1. Detects the push
2. Runs `pnpm install` and `pnpm build`
3. Publishes the result to `marie-perry.pages.dev` within ~2 minutes

No manual deploy step, no servers to manage.

## Tools I used to build it

- **Claude Code** for editing files, debugging the deploy, compressing the demo video, and writing this README
- **GitHub** to host the source
- **Cloudflare Pages** to serve the site
- **Finder** to drag photos around

## Want to build your own?

I wrote a beginner-friendly guide at [`make-your-own.md`](./make-your-own.md). It assumes no terminal experience and walks through the whole flow using Claude Code.

## Stack

| Layer | Tool |
|---|---|
| Static site generator | [Astro](https://astro.build) |
| UI library | [React](https://react.dev) |
| Styling | [Tailwind CSS v4](https://tailwindcss.com) |
| Components | [shadcn/ui](https://ui.shadcn.com) |
| Fonts | [Fontsource](https://fontsource.org) (Geist) |
| Theme | Custom palette via [tweakcn.com](https://tweakcn.com) |
| Hosting | [Cloudflare Pages](https://pages.cloudflare.com) |
| Package manager | [pnpm](https://pnpm.io) |
