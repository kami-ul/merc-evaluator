# Project 10 — PoE 1 Mercenary Evaluator & Preset Finder

## Status
MVP implemented as single-file static app. See `plannedfeatures.md` for backlog.

## What's Being Built
A client-side static web app hosted on GitHub Pages. Two features:
1. **Warrant Parser** — Paste in-game Mercenary Warrant text (Ctrl+C), parse skill links, generate PoE trade search URLs (`?q=` encoded JSON).
2. **Popular Presets** — Catalog of recommended mercenary builds from Cpt Lance's Luminary guide with pre-built trade links.

## File Structure
- `index.html` — Entire app: inline Tailwind (CDN), CSS config, parser, stat map, presets. Single file, no build step.
- `plannedfeatures.md` — Backlog of future features.
- `designdoc.md` — Full spec: features, UI/UX, Tailwind palette, edge cases.
- `ressources/linkGeneration.md` — Trade URL pipeline spec.
- `ressources/exampleMercs.md` — Raw clipboard samples for parser test cases.
- `ressources/idealMercs.md` — CptLance's recommended builds (source: mobalytics.gg).
- `testlinks.md` — Trade URL test cases for validation.

## Key Constraints
- **Client-side only.** No server, no API calls, no build step.
- **Single file.** Everything lives in `index.html`. Tailwind via CDN.
- **Dark theme.** Palette in `designdoc.md` section 5 (`#09090b` bg, `#d97706` gold accent).
- **Keyboard-first.** Auto-focus on paste area, instant parse on `input`/`paste` events, no submit button.
- **Note the directory is spelled `ressources/`** (with double s) — match this in any paths.

## League
Hardcoded to `Allflame` in `index.html`. League selector is a planned feature.

## Stat ID Mapping
`SKILL_STAT_MAP` in `index.html` has 541 entries with real `mercenary.skill_*` and `mercenary.support_*` stat IDs sourced from `ressources/mercenaries.json`. One remaining placeholder:
- `'Lesser Multiple Projectiles'` — not found in API data, marked as `TODO_`

## Parser Behavior
- Splits warrant text by `--------` sections
- Each section = one skill group: first recognized active skill + subsequent supports
- Supports with tier notation `Name (Tier: X)` are stripped to base name, tier stored for display
- Returns `{ name, build, level, actives[], supports[], groups: [{ active, linked: [{ name, tier }] }] }`
- `"Return"` maps to `mercenary.support_5293` (in-game name for "Returning Projectiles")
- `"Haste"` is an active skill (`mercenary.skill_52155`), not a support

## Trade URL Format
- Each skill group (active + linked supports) becomes its own `{"type": "and", filters: [...]}` stat group
- No `status` filter — defaults to "Instant Buyout and In Person"
- Structure: `{ query: { stats: [group1, group2, ...] }, sort: { price: 'asc' } }`

## Presets Data
- Stored as `groups: [{ active, linked: [supportNames] }]` (linked are plain strings, no tier info)
- 4 presets: Kineticist, Manyshot, Blade Ambusher, Combatant
- Trade URLs built inline in `renderPresets()` using same per-group "and" structure

## UI Details
- Skill badges: gold border for actives (`rgba(251,191,36,0.35)`), purple for supports (`rgba(139,92,246,0.3)`)
- Supports display tier as `(TX)` suffix (e.g., "Lesser Spell Cascade (T1)")
- Preset badges use `text-[10px]` for smaller font
- Header links to Cluster Resolver project
