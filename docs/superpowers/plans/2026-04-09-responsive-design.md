# Responsive Design — Mobile Pass Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a single `@media (max-width: 640px)` CSS block to `index.html` that flattens the CRT bezel, hides score bar counters, and tightens layout for mobile — with zero changes to game logic or desktop styles.

**Architecture:** All changes are a single new `<style>` block appended just before `</head>` in `index.html`. Each task adds rules to this block incrementally. No JS is touched.

**Tech Stack:** Plain HTML/CSS. No build step. Open `index.html` directly in a browser (or via `node server.js`) to verify.

---

## File Map

| File | Action | Responsibility |
|---|---|---|
| `index.html` | Modify — insert new `<style>` block before `</head>` (after line 390) | All responsive CSS lives here |

---

### Task 1: Create the mobile media query block + flatten CRT bezel

**Files:**
- Modify: `index.html` — insert before `</head>` (line 391)

The CRT bezel (`#crt-bezel`) has `padding: 44px 52px 72px`, a gradient background, border-radius, and a large box-shadow. On mobile these waste screen space. The `::before` (brand label) and `::after` (power LED) pseudo-elements are positioned inside the bezel shape — meaningless without it. `#crt-screen` keeps its dark background and effects but gets tighter padding.

- [ ] **Step 1: Open `index.html`. Find line 390 (`</style>`) and line 391 (`</head>`). Insert the following new `<style>` block between them:**

```html
<style>
/* ── Mobile Responsive (≤640px) ──────────────────────────── */
@media (max-width: 640px) {

  body {
    padding: 0;
  }

  /* Flatten CRT bezel — remove all decorative chrome */
  #crt-bezel {
    background: transparent;
    border-radius: 0;
    box-shadow: none;
    padding: 0;
  }
  #crt-bezel::before,
  #crt-bezel::after {
    display: none;
  }

  /* Screen — keep effects, tighten padding */
  #crt-screen {
    border-radius: 0;
    padding: 1rem 0.75rem 1.5rem;
  }

}
</style>
```

- [ ] **Step 2: Open the site in a browser. Resize to ≤640px width (or use DevTools device emulation — iPhone SE is 375px).**

Expected: The grey rounded CRT monitor frame is gone. The game content fills the screen edge-to-edge with a dark background. The scanline and vignette overlays still appear. Desktop (>640px) is completely unchanged.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: flatten CRT bezel on mobile (≤640px)"
```

---

### Task 2: Hide score bar counters on mobile

**Files:**
- Modify: `index.html` — add rules inside the `@media (max-width: 640px)` block created in Task 1

The score bar has four flex children:
1. `RACES 00` — hide
2. `PLAYERS 00` — hide
3. `CREDIT 01` — hide
4. `div` containing 🔊 and EDIT buttons — keep, right-align

- [ ] **Step 1: Inside the `@media (max-width: 640px)` block (before the closing `}`), append:**

```css
  /* Score bar — hide counters, keep controls right-aligned */
  .score-bar > div:nth-child(-n+3) {
    display: none;
  }
  .score-bar {
    justify-content: flex-end;
  }
```

- [ ] **Step 2: Verify in browser at ≤640px.**

Expected: Score bar shows only the 🔊 audio button and EDIT button, right-aligned. No RACES / PLAYERS / CREDIT text visible. Desktop unchanged.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: hide score bar counters on mobile"
```

---

### Task 3: Player grid — 2 columns on mobile

**Files:**
- Modify: `index.html` — add rules inside the `@media (max-width: 640px)` block

The existing code has `@media (max-width:700px) { .player-grid { grid-template-columns:repeat(3,1fr); } }`. On a 375px phone, 3 columns with emoji + name + entries is too cramped. Adding a `max-width: 640px` rule for 2 columns overrides the 700px rule at small sizes (more-specific breakpoint wins because it appears later in the cascade).

- [ ] **Step 1: Inside the `@media (max-width: 640px)` block, append:**

```css
  /* Player grid — 2 columns (overrides the 700px → 3-col rule) */
  .player-grid {
    grid-template-columns: repeat(2, 1fr);
  }
```

- [ ] **Step 2: Verify in browser at ≤640px on the Setup screen.**

Expected: Player cards display in 2 columns. Each card has enough horizontal space for the emoji, name input, and entries controls without overflow. At 641px+ the grid stays at 3 or 5 columns as before.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: 2-column player grid on mobile"
```

---

### Task 4: Tighten panel padding on mobile

**Files:**
- Modify: `index.html` — add rules inside the `@media (max-width: 640px)` block

`.panel` has `padding: 1.4rem` on desktop. On mobile this burns horizontal space on the left and right sides of each panel. Reducing it recovers usable width for content inside.

- [ ] **Step 1: Inside the `@media (max-width: 640px)` block, append:**

```css
  /* Tighten panel padding */
  .panel {
    padding: 1rem 0.9rem;
  }
```

- [ ] **Step 2: Verify in browser at ≤640px across all screens (Setup, Track Select, Race, Winner).**

Expected: Panels sit flush with slightly less internal breathing room but content is no longer clipped or unnecessarily cramped. Desktop panels are unchanged.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: tighten panel padding on mobile"
```

---

### Task 5: Full cross-screen verification

No code changes in this task — just a thorough walkthrough to confirm nothing is broken.

- [ ] **Step 1: Open the site at ≤640px (iPhone SE = 375px is a good target). Run through the full game flow:**

  - Setup screen: add 4+ players, adjust entries
  - Track Select screen: select/deselect tracks, adjust speed
  - Race screen: start race, watch slow-mo zoom, check standings panel stacks below track
  - Winner screen: confirm trophy card, winner name, NEXT RACE / NEW RACE buttons

  Expected at each screen:
  - No horizontal scroll
  - No text or UI elements clipped off-screen
  - Buttons are tappable (no overlap)
  - Slow-mo zoom (`scale(2.2)`) still fires and resolves correctly
  - Confetti spawns correctly

- [ ] **Step 2: Switch to desktop width (>640px) and confirm the CRT bezel, score bar counters, and all desktop styles are fully intact.**

- [ ] **Step 3: If any issues found, fix them in the `@media (max-width: 640px)` block and re-verify. Commit fixes with descriptive messages.**

- [ ] **Step 4: Final commit if any fixes were made during verification:**

```bash
git add index.html
git commit -m "fix: mobile responsive tweaks from cross-screen verification"
```
