# Grand Prix — Referral Raffle Horse Race

A pixel arcade-style horse race raffle game built as a single standalone `index.html` file. Used by a company to run referral raffles in a fun, interactive way.

---

## How It Works

Players are entered into a raffle system. Each player can have **multiple entries**, represented as multiple lanes in the race. The more entries a player has, the higher their chance of winning. After each round, the winner and all their entries are removed, and the race continues with the remaining players.

---

## Screens

### 1. Setup Screen (`setup-screen`)
- Players enter their name, choose an emoji, and set their number of entries (1–20)
- 3 empty rows appear by default on page load
- Clicking an emoji button opens an inline emoji picker grid below that player's row
- Keyboard shortcuts: `↓/↑` navigate rows, `←/→` cycle emojis, `Backspace` on empty field deletes row, `Enter` advances or starts race
- `+ ADD PLAYER` adds a new row
- `START RACE >` validates and begins the race

### 2. Race Screen (`race-screen`)
- Each player's entries are expanded into individual lanes
- All entries for the same player share the same emoji and text color
- `START RACE >` begins the animation
- Horses gallop with phase-based velocity (surge / cruise / tired) — winner is pre-selected randomly but gets a speed boost in the final 25%
- Name tags beside emojis fade out at race start
- Checkered black/white finish line on the right
- Speed slider (1–5)
- `NEW RACE` returns to setup with a blank form

### 3. Winner Screen (`winner-screen`)
- Replaces the track when a winner crosses the finish line
- Shows winner name with confetti
- `NEXT RACE >` removes the winner's entries and rebuilds the track for remaining players (hidden if fewer than 2 remain)
- `NEW RACE` returns to the setup screen with blank form

---

## Key Data Model

```
originalPlayers  — set once at startSetup(), never mutated
                   [{name, emoji, entries, color}]

remainingPlayers — copy filtered after each round (winner removed)
                   [{name, emoji, entries, color}]

lanes            — expanded for current round
                   [{name, emoji, color}]
                   (one entry per lane — a player with 3 entries = 3 lanes)
```

Colors are assigned once per unique player name at `startSetup()` time and persist across all rounds.

---

## File Structure

```
index.html   — entire app (HTML + CSS + JS, self-contained)
README.md    — this file
CHANGELOG.md — history of changes
```

---

## Hosting

Hosted on **GitHub Pages**. To update:
1. Go to the repo on github.com
2. Click `index.html` → pencil icon to edit
3. Select all, paste new code, commit

---

## Tech Stack

- Vanilla HTML/CSS/JS — no frameworks, no dependencies
- Google Fonts: `Press Start 2P` (pixel font)
- No localStorage, no backend, no build step

---

## Known Constraints

- Runs inside a sandboxed iframe (Claude.ai artifact environment)
- `position:fixed` and `position:absolute` overlays do not work reliably inside the iframe — use normal document flow for any UI that needs to appear on top
- The emoji picker is implemented as an inline `<tr>` inserted after the clicked player row (not a floating dropdown) for this reason
