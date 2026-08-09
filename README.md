# PoE Mercenary Warrant Evaluator

Instantly evaluate Path of Exile 1 Mercenary Warrants and search the trade platform for matching mercenaries.

## Features

- **Warrant Parser** — Hover over a Mercenary Warrant in-game, press `Ctrl + C`, paste the text, and get an instant breakdown of skill links with trade search URLs
- **Popular Presets** — Pre-configured mercenary builds from Cpt Lance's Luminary guide with one-click trade search

## How It Works

1. Hover over your Mercenary Warrant in-game
2. Press `Ctrl + C` to copy the warrant text
3. Paste into the text area — skill links are parsed instantly
4. Click **Search Trade** to open PoE's trade site with your mercenary's skills pre-filtered
5. Or click **Copy Trade Link** to grab the URL

## Presets

Sourced from [Cpt Lance's Luminary build](https://mobalytics.gg/poe/builds/captainlance9-luminary-merc-bot):

- **Kineticist** — Best-in-slot map clearing merc
- **Manyshot** — Hybrid clear / single target
- **Blade Ambusher** — Trap-based boss killer
- **Combatant** — Versatile starter merc

## Trade Search

Generated links search for "Instant Buyout and In Person" listings, with each skill group (active + linked supports) as its own filter group. Sorted by price ascending.

## Tech

Single-file static app (`index.html`). No build step, no server, no dependencies beyond Tailwind CDN. Hosted on GitHub Pages.

## Disclaimer

This project is an independent fan-made tool and is not affiliated with, authorized, maintained, sponsored, or endorsed by Grinding Gear Games or Path of Exile.

Path of Exile and Grinding Gear Games are trademarks or registered trademarks of Grinding Gear Games in New Zealand and/or other countries. All game assets, imagery, and related materials are property of Grinding Gear Games.
