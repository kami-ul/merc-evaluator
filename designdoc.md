# Software Design Document: PoE 1 Mercenary Evaluator & Preset Finder

## 1. Executive Summary & Goals

* **Target Audience:** Luminary (Scion) players in *Path of Exile 1: Curse of the Allflame* (3.29) evaluating or trading Mercenary Warrants.
* **Core Function:** A fast, client-side static web tool hosted on GitHub Pages that instantly converts pasted in-game warrant data into encoded PoE trade URLs, while offering 1-click trade searches for CptLance's ideal Mercenary setups.
* **Design Pillars:**
1. **Zero Fluff:** No splash screens, no useless clicks, no background fluff.
2. **Keyboard-First Ergonomics:** Auto-focus, paste-and-go detection, instant URL generation.
3. **Eye Protection:** Deep dark theme designed to fit seamlessly next to Path of Exile on a second monitor.



---

## 2. Resource Dependency Map

The application relies strictly on client-side data and local markdown assets:

* `resources/linkGeneration.md`: Defines the precise JSON schema required for PoE trade searches and details the client-side URI encoding process (`?q=` payload generation) to bypass API calls and CORS restrictions.
* `resources/exampleMercs.md`: Contains raw clipboard text samples of Mercenary Warrants (Name, Level, Active Skills, Support Skills) used as test cases for the regex parsing engine.
* `resources/idealMercs.md`: Contains suggestions from the Luminary guide by cpt lance. also link to the guide (should appear somewhere on the site).

---

## 3. Core Feature Specifications

### Feature 1: Warrant Price Evaluator

* **Input Zone:** Large monospaced textarea that auto-captures clipboard content on paste (`Ctrl+V`).
* **Parser Logic:** Strips irrelevant item stats, flavor text, and section dividers. Extracts solely:
* Mercenary archetype / Name
* Mercenary Level
* Primary Active Skills
* Linked Support Skills


* **Parsed State Preview:** Displays extracted metadata as visual badges/chips so the user can verify accuracy before searching.
* **Action:** Direct CTA button ("Open in PoE Trade") that opens the generated URL in a new tab, plus a secondary "Copy Trade Link" utility button.

### Feature 2: CptLance Mercenary Presets

* **Structure:** Clean visual catalog categorized by role (e.g., *Aurabot Support*, *Curse/Debuffer*, *Single-Target DPS*).
* **Card Details:**
* Preset Name & Role Tag
* Brief note on why CptLance recommends it
* Core skills searched in the trade link


* **Action:** Hardcoded/Pre-built trade links using static queries configured according to `resources/linkGeneration.md`.

---

## 4. UI/UX & Aesthetic Guidelines

### Ergonomics & Flow

* **Single-Page Dashboard:** Split view on desktop (Left: Evaluator, Right: Presets); stacked view on mobile.
* **Instant Processing:** No "Submit" button required for parsing—listening to `input`/`paste` events triggers instant parsing and link generation.
* **Auto-Focus:** On page load, cursor auto-focuses on the paste input box.
* **One-Click Reset:** Clear button inside the textarea to reset state in a single click.

### Theme & Dark Aesthetics

* **Base Palette:** Deep graphite and obsidian tones (`#09090b` / `#121215`) to eliminate screen glare.
* **Accent Palette:** Warm PoE-inspired Amber/Gold for primary trade actions and Crimson for skill tags/highlights.
* **Text Contrast:** High-readability off-white text (`#f4f4f5`) with subtle zinc secondary text (`#a1a1aa`).

---

## 5. Tailwind CSS Implementation Suggestions

### Palette Configuration

```javascript
// Suggested Tailwind CSS color extensions
colors: {
  poe: {
    bg: '#09090b',       // bg-zinc-950 equivalent for deep background
    card: '#121215',     // Elevated card background
    border: '#27272a',   // Muted border (border-zinc-800)
    gold: '#d97706',     // Primary action CTA (amber-600)
    goldHover: '#b45309',// Hover state (amber-700)
    crimson: '#dc2626',  // Skill highlight accent (red-600)
  }
}

```

### Component Styling Classes

* **App Shell / Container:**
`min-h-screen bg-zinc-950 text-zinc-100 font-sans p-4 md:p-8 space-y-6`
* **Dark Card Surfaces:**
`bg-zinc-900/80 border border-zinc-800 rounded-lg p-5 shadow-xl backdrop-blur-sm`
* **Paste Textarea:**
`w-full bg-zinc-950 border border-zinc-800 rounded-md p-3 font-mono text-sm text-zinc-200 placeholder-zinc-600 focus:outline-none focus:ring-2 focus:ring-amber-500/50 focus:border-amber-500 transition-all`
* **Parsed Skill Badges / Chips:**
`inline-flex items-center gap-1.5 bg-zinc-800/90 text-amber-300 border border-zinc-700/60 text-xs font-mono px-2.5 py-1 rounded-md`
* **Primary Trade Button (CTA):**
`inline-flex items-center justify-center gap-2 bg-amber-600 hover:bg-amber-500 text-zinc-950 font-semibold px-4 py-2.5 rounded-md transition-colors shadow-lg shadow-amber-950/20 active:scale-[0.98]`
* **Secondary Utility Button:**
`inline-flex items-center justify-center bg-zinc-800 hover:bg-zinc-700 text-zinc-200 border border-zinc-700 font-medium px-3 py-2 rounded-md transition-colors text-sm`

---



## 6. Edge Case Handling

* **Empty / Invalid Paste:** If the regex fails to match any valid mercenary skills from `resources/exampleMercs.md`, gracefully display a subtle error (`"No valid mercenary skills detected"`) without breaking the UI.
* **Browser Clipboard Restrictions:** Provide a clear fallback paste box if browser permission blocks programmatic clipboard access.
* **URL Character Caps:** Query payload stringification must strip unnecessary white spaces before URI encoding to ensure trade links stay well under safe browser URL length boundaries.