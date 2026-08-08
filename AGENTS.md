# Project 10 — PoE 1 Mercenary Evaluator & Preset Finder

## Status
MVP implemented as single-file static app. See `plannedfeatures.md` for backlog.

## What's Being Built
A client-side static web app hosted on GitHub Pages. Two features:
1. **Warrant Price Evaluator** — Paste in-game Mercenary Warrant text, parse skills, generate PoE trade search URLs (`?q=` encoded JSON).
2. **CptLance Presets** — Catalog of recommended mercenary builds with pre-built trade links.

## File Structure
- `index.html` — Entire app: inline Tailwind (CDN), CSS config, parser, stat map, presets. Single file, no build step.
- `plannedfeatures.md` — Backlog of future features.
- `designdoc.md` — Full spec: features, UI/UX, Tailwind palette, edge cases.
- `ressources/linkGeneration.md` — Trade URL pipeline spec.
- `ressources/exampleMercs.md` — Raw clipboard samples for parser test cases.
- `ressources/idealMercs.md` — CptLance's recommended builds (source: mobalytics.gg).

## Key Constraints
- **Client-side only.** No server, no API calls, no build step.
- **Single file.** Everything lives in `index.html`. Tailwind via CDN.
- **Dark theme.** Palette in `designdoc.md` section 5 (`#09090b` bg, `#d97706` gold accent).
- **Keyboard-first.** Auto-focus on paste area, instant parse on `input`/`paste` events, no submit button.
- **Note the directory is spelled `ressources/`** (with double s) — match this in any paths.

## Critical: Stat ID Mapping
The `SKILL_STAT_MAP` object in `index.html` uses **placeholder stat IDs** (`explicit.stat_3653701493`). These must be replaced with real stat IDs from `https://www.pathofexile.com/api/trade/data/stats` before the trade links will work correctly. Each skill name needs its actual PoE trade stat ID.

## League
Hardcoded to `Settlers` in `index.html`. League selector is a planned feature.

## Trade URL Pipeline
Parse warrant text → extract name/level/skills → map skill strings to PoE stat IDs → build JSON query → `JSON.stringify` → `encodeURIComponent` → append to `https://www.pathofexile.com/trade/search/{League}?q=...`
