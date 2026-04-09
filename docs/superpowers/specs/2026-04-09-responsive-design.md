# Responsive Web Design — Mobile Pass
**Date:** 2026-04-09
**Status:** Approved

## Overview

Add a single `@media (max-width: 640px)` CSS block to make the horse racing game look and feel good on mobile devices. All changes are CSS-only, isolated to one media query. Zero touches to game logic, JS, or desktop styles.

## Approach

Option A — Single mobile breakpoint. All responsive rules live in one `@media (max-width: 640px)` block appended after the existing `<style>` sections in `index.html`.

## Breakpoint

`640px` — covers phones in portrait and landscape up to small tablets. Desktop experience (≥641px) is completely untouched.

## Design Decisions

### 1. CRT Bezel — Flatten on Mobile
- `#crt-bezel`: remove gradient background, border-radius, box-shadow, large padding. Reduce to a transparent zero-padding wrapper.
- `#crt-bezel::before` (brand label "PICCOLLAGE COMPANY"): `display: none` — relies on absolute positioning inside the bezel shape.
- `#crt-bezel::after` (power LED): `display: none` — same reason.
- `#crt-screen`: retain background color, scanline overlay (`::before`), and vignette (`::after`) — these still look good on mobile. Reduce padding to `1rem 0.75rem 1.5rem`.

### 2. Score Bar — Hide Counters on Mobile
- Hide the RACES, PLAYERS, and CREDIT counter divs (`display: none`).
- The bar simplifies to just the 🔊 and EDIT buttons, right-aligned.
- The score bar element itself stays visible as a thin strip.

### 3. Audio Panel — No Change Needed
- `min-width: 280px` fits within ~351px of available content width on a 375px phone with 12px padding each side.
- YouTube URL input row already uses `flex: 1` so it shrinks naturally.

### 4. Player Grid — 2 Columns on Mobile
- Change `grid-template-columns` from `repeat(3, 1fr)` to `repeat(2, 1fr)` at ≤640px.
- Player cards (~160px each) fit comfortably at 375px with a 2-column layout.

### 5. Panel Padding — Tighten on Mobile
- `.panel`: reduce padding from `1.4rem` to `1rem 0.9rem` to recover horizontal space.

### 6. Game Logic — No Changes
- Horse positioning uses `%`-based `left`/`top` on the track container — already responsive.
- SVG track uses `viewBox` + `width: 100%` — already responsive.
- Standings, slow-mo zoom, confetti, modals — all untouched.

## Files Changed

- `index.html` — append one `@media (max-width: 640px)` block to the existing `<style>` section.

## Out of Scope

- Tablet-specific styles (641px–900px already looks good)
- Mobile-first CSS rewrite
- Any JS changes
- Touch-specific interactions (tap targets are already acceptable)
