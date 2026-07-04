# CLAUDE.md — Density Game Project Context

## What This Is

Density is a two-player competitive city-building grid game. Two developers battle to build the densest urban block on a 20×20 grid. First to build a Tower (density level 4) wins.

This is a single-file HTML/CSS/JS game (`index.html`). No build tools, no dependencies, no server.

## Project Tracker (READ FIRST)

The project tracker lives in **Notion** (it moved there from a Google Doc in June 2026 — ignore any stale Google Doc references). Ask the user for the Notion page if you can't access it.

The tracker is the source of truth for: game rules, task board, decision log, roadmap, known bugs, and open questions. The repo is the source of truth for code.

## After Every Change

1. **Update the Notion tracker** with any rule changes, new decisions, version bumps, task status changes, and bug fixes.
2. **Audit the tracker against the code** — check that documented methods, rules, constants, and mechanics match what the code actually does. Flag discrepancies.
3. **Bump the version** in CHANGELOG.md, in the `VERSION` constant in index.html, and in the Notion tracker's version history.
4. Skip these steps only if the user explicitly says not to.

## Current Game Rules (v0.13)

- Grid: 20×20
- Players alternate turns, one placement per turn
- Buildings start at density 1 (low-rise), upgrade through 4 levels: low-rise → mid-rise → high-rise → tower
- **Upgrade rule:** A building upgrades +1 when 3 of its 4 orthogonal neighbors are occupied at or above its current density level
- Upgrades cascade until no more are possible, but each tile can upgrade at most +1 per turn (v0.8.1), and a park-boosted tile can't also cascade-upgrade the same turn (v0.8.2)
- **Rivers:** Predetermined terrain, generated randomly at game start (2 rivers). Can't build on them. Count as low-rise (level 1) for upgrade checks (v0.9; was wild before), so they only help the low-rise → mid-rise upgrade.
- **Parks:** 3 per player. When placed, give +1 density to ALL adjacent buildings — both players' (changed in v0.7.2). Count as wild for upgrade checks.
- **Bulldozer (v0.13 prototype, variant flags in `BULLDOZER` constant):** 2 charges per player; using one is your whole turn and demolishes a qualifying building (default since v0.13.2: any building below tower, either player's; variants: own-only, opponent low-rises). No cascade; densities are sticky. Optional rubble variant blocks the tile for 2 turns. Reconcile with Adrian's proposal before v1.0.
- **Win condition:** First player to reach Tower (density 4) wins immediately.
- Edge tiles (3 neighbors) can upgrade if all 3 qualify. Corner tiles (2 neighbors) cannot upgrade.

## Code Architecture

Single file: `index.html`. Key globals and functions:

### Constants
- `GRID = 20`
- `MAX_PARKS = 3`
- `P1_COLORS = ['#FAC775','#EF9F27','#E24B4A','#7F77DD']`
- `P2_COLORS = ['#9FE1CB','#1D9E75','#378ADD','#534AB7']`
- `DENSITY_NAMES = ['low-rise','mid-rise','high-rise','tower']`

### State
- `board[20][20]` — null or `{type: 'settle'|'park'|'river', player: 1|2, density: 1-4}`
- `inventory` — `{1: {parks, buildings, dozers}, 2: {parks, buildings, dozers}}`
- `currentPlayer` — 1 or 2
- `gameOver` — boolean

### Key Functions
- `initGame()` — reset everything, generate rivers, render
- `generateRivers()` — 2 procedural rivers using random walk with meander
- `handleClick(r, c)` — main game loop: validate, place, resolve park boosts, cascade, switch player
- `canUpgrade(r, c)` — true if 3+ neighbors occupied and meet density threshold; parks return -1 from getEffectiveDensity() (wild), rivers return 1 (low-rise only, v0.9)
- `runUpgradeCascade()` — loop board until stable (safety cap: 50 iterations)
- `getEffectiveDensity(r, c)` — returns density for settle tiles, -1 for parks/empty (wild), 1 for rivers
- `isOccupiedForSurround(r, c)` — returns true if cell is not null
- `renderGrid()` — full DOM rebuild of grid
- `endGame(winner, r, c)` — set gameOver, show banner

## Coding Standards

- Keep everything in one file (`index.html`) unless there's a strong reason to split
- No external dependencies — the game should work by opening the HTML file in a browser
- Support dark mode via `prefers-color-scheme` media query
- Use CSS custom properties for theming
- Commit messages should reference the version: `v0.7: Add undo/redo system`
- Test in browser after every change

## File Layout

```
density-game/
├── CLAUDE.md        ← you are here
├── README.md        ← project overview, how to play
├── index.html       ← the game (single file)
├── RULES.md         ← full game rules
├── CHANGELOG.md     ← version history
├── database.rules.json ← Firebase RTDB security rules (deploy BEFORE pushing client changes that alter state shape)
├── firebase.json    ← Firebase CLI config (deploy rules: firebase deploy --only database)
├── .firebaserc      ← Firebase project binding (density-game)
└── LICENSE          ← MIT
```

## Two-Environment Workflow

This project is developed across two Claude environments:

- **Claude.ai (Projects)** — game design, rule discussions, live prototyping with show_widget, playtesting, Google Doc updates
- **Claude Code** — git operations, code refactoring, file editing, deployment, testing

The Google Doc bridges both. Either environment should read it before starting work to get current context.
