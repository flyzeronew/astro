# CLAUDE.md


This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```sh
pnpm dev          # dev server at localhost:4321
pnpm build        # build to ./dist/
pnpm preview      # preview the built site
pnpm astro check  # TypeScript / type-check
pnpm astro add <integration>  # add an official integration (e.g. react, tailwind)
```

## Architecture

**Static site** built with Astro 6.x, output mode `static` (default). No JS framework integration is configured yet. Package manager is **pnpm**.

### Routing

File-based routing under `src/pages/`. Each `.astro` file maps directly to a URL:

- `src/pages/index.astro` → `/`
- `src/pages/blog/index.astro` → `/blog`
- `src/pages/blog/[slug].astro` → `/blog/:slug` (dynamic, if added)

### Layout & component hierarchy

Every page wraps its content in `BaseLayout.astro`, which:
- Imports `global.css`, `Header.astro`, and `Footer.astro`
- Accepts OG/meta props: `title` (required), `description`, `url`, `ogTitle`, `ogDescription`, `ogImage`, `ogUrl`
- Constrains `<main>` to `max-width: 900px`

`Header.astro` computes the active nav link via `navActive()`, using `Astro.url.pathname` with `class:list={{ act: navActive(href) }}`.

### Styling

- Global resets and font stack (`Roboto`, `Noto Sans TC`) live in `src/styles/global.css`
- Component-scoped `<style>` blocks are used everywhere else (Astro scopes them automatically)
- Brand colour: `#7c3aed` (purple)

### Assets

- `public/` — static files served as-is (e.g. `favicon.svg`)
- `src/assets/` — assets processed by Astro/Vite (import in `.astro` files for optimisation)

### TypeScript

Extends `astro/tsconfigs/strict`. Types for Astro-specific globals (e.g. `Astro.props`, `Astro.url`) are generated into `.astro/types.d.ts` automatically on `dev`/`build`.
