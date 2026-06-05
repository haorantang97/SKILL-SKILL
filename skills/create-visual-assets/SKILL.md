---
name: create-visual-assets
description: "Use this skill when the user needs visual assets for their AI skill or rule repository: banner images and badge code. This includes generating a light-mode SVG banner, a dark-mode SVG banner, the HTML embed snippet with automatic dark/light switching, and the badge markdown row for license, PRs Welcome, and last-updated. Trigger when the user says 'make a banner', 'I need a logo for my repo', 'create assets', 'design the repo header', 'give me the badges', or 'my README has a banner placeholder that needs filling'. Also trigger when the README has already been written with a placeholder but the actual SVG files haven't been created yet. Do NOT trigger for complex logo design, brand identity systems, or any asset that requires raster images, photos, or design software — only SVG banners and badge code are in scope."
license: MIT
---

## Quick Start

Generate `assets/banner-dark.svg` and `assets/banner.svg` using the default color scheme. Substitute `{REPO_NAME}` and `{TAGLINE}` in the templates below, leave everything else as-is. Then output the README embed snippet from Phase 3.

---

## Phase 1: Gather inputs

| Input | Default |
|---|---|
| Repo / skill name | Required — ask if missing |
| One-line tagline | Required — ask if missing |
| Primary color (dark banner bg) | `#0f172a` |
| Accent color (title text) | `#f8fafc` |
| Subtitle color | `#94a3b8` |

If the user has no color preference, use the defaults.

---

## Phase 2: Generate banner SVGs

Write these as actual files to `assets/banner.svg` and `assets/banner-dark.svg`.

**Dark banner** (`assets/banner-dark.svg`):
```svg
<svg width="700" height="175" xmlns="http://www.w3.org/2000/svg">
  <rect width="700" height="175" fill="{DARK_BG}" rx="8"/>
  <text x="350" y="82"
        font-family="system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif"
        font-size="38" font-weight="700" fill="{TITLE_COLOR}"
        text-anchor="middle" dominant-baseline="middle">{REPO_NAME}</text>
  <text x="350" y="128"
        font-family="system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif"
        font-size="15" fill="{SUBTITLE_COLOR}"
        text-anchor="middle">{TAGLINE}</text>
</svg>
```

**Light banner** (`assets/banner.svg`):
```svg
<svg width="700" height="175" xmlns="http://www.w3.org/2000/svg">
  <rect width="700" height="175" fill="#ffffff" rx="8"/>
  <text x="350" y="82"
        font-family="system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif"
        font-size="38" font-weight="700" fill="#0f172a"
        text-anchor="middle" dominant-baseline="middle">{REPO_NAME}</text>
  <text x="350" y="128"
        font-family="system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif"
        font-size="15" fill="#475569"
        text-anchor="middle">{TAGLINE}</text>
</svg>
```

Substitutions:
- `{DARK_BG}` → dark background color (default `#0f172a`)
- `{TITLE_COLOR}` → title text on dark banner (default `#f8fafc`)
- `{SUBTITLE_COLOR}` → tagline on dark banner (default `#94a3b8`)
- `{REPO_NAME}` → repo or skill name, title-cased
- `{TAGLINE}` → one-line tagline; keep under 60 characters or it overflows at 700px width

---

## Phase 3: Generate the README embed snippet

Place this before the `# Title` line in README.md:

```html
<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/banner-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="assets/banner.svg">
    <img width="700" alt="{REPO_NAME}" src="assets/banner.svg">
  </picture>
</p>
```

The `<picture>` tag serves the dark or light version depending on the viewer's theme. `src="assets/banner.svg"` is the fallback for environments that don't support `<picture>`.

---

## Phase 4: Generate badge markdown

Place directly after the `# Title` heading:

```markdown
[![License](https://img.shields.io/badge/license-{LICENSE}-blue.svg?style=flat-square)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](CONTRIBUTING.md)
[![Last Updated](https://img.shields.io/badge/updated-{MON%20YYYY}-blue.svg?style=flat-square)]()
```

Fill:
- `{LICENSE}` → `MIT` or `CC0` (no spaces)
- `{MON%20YYYY}` → current month and year, URL-encoded, e.g., `Jun%202026`

**Optional badges** (add only if applicable):

```markdown
<!-- npm package -->
[![npm](https://img.shields.io/npm/v/{PACKAGE_NAME}.svg?style=flat-square)](https://npmjs.com/package/{PACKAGE_NAME})

<!-- Awesome list certification — only if accepted into sindresorhus/awesome -->
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

<!-- Custom content count -->
[![Skills](https://img.shields.io/badge/skills-{N}-blue.svg?style=flat-square)]()
```

Never exceed 6 badges total.

---

## Color palette reference

| Theme | Dark BG | Title | Subtitle | Light BG | Light Title |
|---|---|---|---|---|---|
| Default (slate) | `#0f172a` | `#f8fafc` | `#94a3b8` | `#ffffff` | `#0f172a` |
| Neutral gray | `#18181b` | `#fafafa` | `#a1a1aa` | `#fafafa` | `#18181b` |
| Deep blue | `#0c1a3a` | `#e2e8f0` | `#7ea5d4` | `#f0f4ff` | `#0c1a3a` |
| Forest | `#0f1f14` | `#d1fae5` | `#6ee7b7` | `#f0fdf4` | `#0f1f14` |
| Warm dark | `#1c1410` | `#fef3c7` | `#d97706` | `#fffbeb` | `#1c1410` |

---

## Common Mistakes

**Using an image hosting service instead of assets/ in the repo.**
External links break when the hosting service changes URLs. Keep banner files inside the repo under `assets/`.

**Omitting the light-mode banner.**
Many users are in light mode. Serving only a dark banner makes the repo look broken for them.

**Forgetting to URL-encode spaces in badge text.**
`https://img.shields.io/badge/updated-Jun 2026-blue` will break. Use `Jun%202026`.

**Making the tagline too long.**
SVG text does not wrap. A tagline over 60 characters overflows the banner width at 700px. Shorten it or split across two `<text>` elements at different `y` positions.

**Using PNG for banners that contain only text and shapes.**
SVG is resolution-independent, renders sharply on all screens, and produces files under 2KB. Use PNG only for banners that contain photos or complex illustrations.
