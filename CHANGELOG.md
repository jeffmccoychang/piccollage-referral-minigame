# Changelog

## Latest State

### Setup Screen
- 3 empty player rows shown by default on page load
- Each row has: emoji picker button | name input | entries counter (−/+/type) | remove (✕)
- Emoji picker opens as an inline grid row directly below the clicked player row; closes on selection or outside click
- Auto-assigns a unique unused emoji for each new row
- Keyboard shortcuts: `↓/↑` navigate between name fields, `←/→` cycles emoji at cursor edges, `Backspace` on empty deletes row, `Enter` advances or starts race
- Minimum 2 players enforced
- Maximum 30 players, maximum 20 entries per player

### Race Screen
- Players expanded into lanes by entry count (e.g. 3 entries = 3 lanes)
- Each player has one consistent color and emoji across all their lanes
- Colors assigned once at race start from a fixed 20-color palette, keyed by player name
- Lane label (left) and name tag (right of emoji) both use player's assigned color
- Name tags fade out in 0.5s when race starts
- Emoji bounces (gallop animation) when race starts — requires `display:inline-block` on `.horse-emoji`
- Checkered finish line (18px wide, repeating-conic-gradient black/white)
- Winner pre-selected randomly; gets speed boost after 72% progress; non-winners slowed after 85%
- Speed slider default: 1

### Winner Screen
- Replaces track in-place (no overlay/fixed positioning)
- Winner's name shown in red pixel font
- All entries for the winner removed from `remainingPlayers` on declaration
- `NEXT RACE >` hidden when fewer than 2 players remain
- `NEW RACE` resets to blank 3-row setup screen

### Visual Design
- Color scheme: dark green (`#0a1a0d`) background, lime green accents, red highlights
- Font: Press Start 2P (Google Fonts)
- Scanline overlay via CSS repeating-linear-gradient
- Pixel-style buttons with 3D box-shadow offset effect
- HUD bar: 1UP (round count) | PLAYERS | CREDIT

---

## Change History

| # | Change |
|---|--------|
| 1 | Initial horse race mini-game with wheel-of-names replacement |
| 2 | Added dramatic race mechanics (surge/cruise/tired phases, inertia) |
| 3 | Added checkered finish line; winner declared on visual crossing |
| 4 | Added "Next Race" button; winner removed each round |
| 5 | Retro Swiss racing poster aesthetic (lime/teal/brown) |
| 6 | Full pixel arcade redesign (dark green, Press Start 2P font, scanlines) |
| 7 | Replaced emoji horses with pixel art SVG sprites (later reverted) |
| 8 | Reverted to emoji horses with gallop bounce animation |
| 9 | Player setup form: emoji picker, name input, entries counter per row |
| 10 | Auto-assign unique emoji per new player row |
| 11 | Consistent color per player across all their entries |
| 12 | Multiple entries per player expand into multiple lanes |
| 13 | Winner's ALL entries removed after winning a round |
| 14 | Winner screen replaces track (fixed overlay approach abandoned due to iframe constraints) |
| 15 | Name tags fade out on race start; gallop bounce fixed with `display:inline-block` |
| 16 | "New Raffle" renamed to "New Race" |
| 17 | 3 empty rows on initial page load |
| 18 | "New Race" preserves original player details |
| 19 | Emoji picker changed to inline row below clicked player (iframe constraint workaround) |

---

## Known Issues / Gotchas

- **`position:fixed` doesn't work** inside the Claude.ai artifact iframe sandbox — all "popup" UI must use normal document flow
- **`transform` on inline `<span>`** doesn't animate — `.horse-emoji` must be `display:inline-block` for the gallop bounce to work
- **Inline `<script>` inside `<table>`** is rejected by browsers — initialization calls must be at the end of the main `<script>` block
- **`DOMContentLoaded` fires too early** in the iframe streaming environment — direct calls at the end of the script are more reliable
