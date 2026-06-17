# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-file static promotional landing page for **The Sweet Life Society**, an Italian party band (booking/press microsite). There is no build system, no package manager, no dependencies, and no tests — everything lives in [index.html](index.html) (HTML + inline `<style>` + one inline `<script>`), served alongside the `assets/` folder. The site is deployed on Vercel at the custom domain `sweetlifesociety.com`; every push to `main` triggers a redeploy.

## Running / previewing

Open the file directly, or serve the folder so relative `assets/` paths and the audio player resolve correctly:

```bash
python3 -m http.server 8000   # then open http://localhost:8000/
```

Use a server (not `file://`) when testing audio playback, the IntersectionObserver reveal, or anything that can be blocked by the `file://` origin.

## Architecture & conventions

The page is built as one document with three parts, all inside `index.html`:

- **`:root` CSS custom properties** (top of `<style>`) are the single source of design truth — `--yellow`, `--ink`, `--blue`, `--paper`, `--night`, `--max` (content width), `--shadow`. Change the palette/layout here, not in individual rules. The aesthetic is a press-kit / album-cover print style: hard offset shadows (`6px 6px 0`), halftone dot texture (`.halftone::after`), stepped text-shadows, slight rotations, `Archivo Black` display type.
- **Sections** are ordered top-to-bottom and keyed by `id` (`#festivals`, `#live`, `#photos`, `#instagram`, `#show`, `#music`, `#booking`); the fixed `<nav>` anchors link to these ids. Each section follows the same skeleton: `<section id><div class="inner"><span class="label">…<h2>…`. The `.inner` wrapper is what gets the scroll-reveal animation.
- **Two inline scripts** at the bottom: (1) a scroll-reveal that adds `.reveal` to every `section .inner` and toggles `.in` via IntersectionObserver, with a 1500ms failsafe so content is never left hidden; (2) a self-contained fixed top audio bar (`#audiobar`) that plays `assets/audio/why-not.mp3` with a clickable progress/seek bar.
- **Accessibility/motion**: every animation has a `@media (prefers-reduced-motion: reduce)` override — preserve this when adding motion. Focus styles use `:focus-visible` with a blue outline.
- **Responsive**: mobile breakpoints are mainly `max-width:820px` (collapses nav links, single-column grids, shows the `.sticky-book` bottom CTA) and `980px`/`1200px` for the hero photo.

## Editing notes

- Italian `<!-- … -->` comments mark intentional placeholder/swap points — e.g. the album art is text and can be replaced with an `<img>`; the `.hero-photo` supports a `cutout` class for pre-cut PNGs; the Spotify/Instagram embeds and social links are flagged to verify/replace. Honor these markers.
- External embeds: YouTube uses `youtube-nocookie.com`; Instagram profile embeds are often blocked by Meta, so a static fallback panel sits behind the iframe by design — don't "fix" the blank iframe by removing the fallback.
- Booking contact is a `mailto:` to `thesweetlifesociety@gmail.com`; there is no backend/form.
- `assets/` contains the three band images (`band-lineup.png`, `band-crowd-1.png`, `band-crowd-2.png`) and the audio track; image paths are relative, so keep the folder structure intact.
