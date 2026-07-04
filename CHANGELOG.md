# Changelog

All notable changes to the Density game are documented here.

## v0.13.2 — 2026-07-04

- Fixed 3D view flattening while waiting for the opponent (multiplayer) or during the computer's turn: the "not your turn" fade set cell opacity below 1, which forces browsers to flatten 3D content. Cells no longer fade in 3D mode — the turn indicator carries that signal.

## v0.13.1 — 2026-07-04

- Fixed silent multiplayer breakage waiting to happen: `createRoom` sent a `created` timestamp that the v0.13 Firebase security rules reject (rooms with unexpected fields are refused). Removed the field — nothing ever read it — so the client is valid under both the old open rules and the new strict rules, whenever they're deployed.

## v0.13 — 2026-07-04

- **New mechanic: the Bulldozer (prototype).** Each player gets 2 charges; spending one is your whole turn and demolishes a qualifying building. No cascade runs — densities are sticky, so demolishing never retroactively downgrades neighbors. Parks and rivers can't be bulldozed.
- Built behind variant flags (`BULLDOZER` constant) for playtesting: `target` ('own' buildings below tower — default — or 'opponent-lowrise'), `charges` (1–3), and `rubble` (off by default; when on, the demolished tile becomes rubble for 2 turns — unbuildable, counts as nothing for upgrade checks, then clears).
- In-game rule text is generated from the flags, so playtest builds never show stale rules.
- Firebase security rules updated for the new inventory field (`dozers`) and rubble tile type — **rules must be deployed before this client version is published**, or multiplayer writes are silently rejected.
- Firebase rules now live in the repo (`database.rules.json`) with `firebase.json`/`.firebaserc`, deployable via `firebase deploy --only database`.
- The AI does not use its bulldozers yet; it handles rubble tiles correctly. (Wiring the bulldozer into the difficulty-level brains from v0.10–0.11 is follow-up work.)
- Fixed stale color names in the RULES.md density table (missed in v0.9.1).
- Prototype rules to be reconciled with Adrian's full proposal before v1.0.

## v0.12 — 2026-07-04

- Added an isometric 3D city view with a 2D/3D toggle button in the game toolbar
- Buildings rise with density — low-rises are slabs, towers loom; rivers and parks stay flat
- Walls are directionally shaded (consistent "sunlight"), built with pure CSS 3D — no new dependencies
- Rotate buttons (⟲ ⟳) in 3D mode spin the city in animated 90° steps so every part of the map is reachable; rooftop density numbers counter-rotate to stay readable
- Purely visual: rules, AI, multiplayer, and the reasoning panel are unaffected; 2D remains the default view

## v0.11 — 2026-07-04

- Added an always-on "Computer's reasoning" panel below the board in vs-computer games: after every AI move, the computer explains what it played and why
- Explanations are built from the AI's actual decision data recorded during the turn (options weighed, moves discarded for gifting a tower, threat check results, whether it picked its #1 choice) — never invented after the fact
- The panel also reveals difficulty behavior honestly: Medium admits when it skipped its threat check or played its #2/#3 choice; Easy admits it has no plan
- Special cases covered: winning moves, desperation moves (every option loses), error fallback, and passing

## v0.10 — 2026-07-04

- Added AI difficulty levels to "Play vs computer": Easy, Medium (default), and Hard
- **Easy** plays random sensible moves (always adjacent to the existing city) — never blocks, never hunts a win; towers only happen by accident
- **Medium** uses the full strategic brain with two human-shaped flaws: it picks among its top 3 moves (50/30/20) instead of always the best, and its "is the opponent about to win?" check only runs 75% of turns — sneaky setups can get past it
- **Hard** is the unchanged classic AI (the only difficulty that existed before)
- The opponent's panel now shows the chosen difficulty (e.g. "Computer · Medium")
- Note: Medium is the new default, which is a step down from the old button's behavior — pick Hard for the classic experience

## v0.9.1 — 2026-07-04

- **New color system.** Terrain owns nature hues (river blue #005fcc, park green #4f9400); developers own artificial hues — Developer 1 amber (#ffd79a → #8a5a18), Developer 2 violet (#db8fff → #7000fa). Darker = denser, following sequential-map convention.
- Why: the old palette had four collisions — both towers were purple (the win state was the most ambiguous pair on the board), P2 high-rise vs river were both blue, P2 mid-rise vs park were both green. Verified against deuteranopia/protanopia simulation; known accepted trade-off: violet tower vs river converge slightly under protanopia, mitigated by glyphs (buildings show numbers/★, rivers don't). Full rationale in the Notion Decision Log.
- Refactor: all player/terrain colors now flow from P1_COLORS / P2_COLORS / RIVER_COLOR / PARK_COLOR constants via applyColorSystem() — legend swatches, build buttons, player dots, panel accents, win message, and river CSS var included. Colors turned out to live in eleven places, not four; now they live in one (same single-source pattern as the VERSION fix).
- Bonus fix: error-message red was previously identical to P1's high-rise color; no longer a player color.

## v0.9 — 2026-07-03

- **Rule change:** rivers now count as low-rise (level 1) neighbors for upgrade checks, instead of matching any density level (wild). Riverbanks still help early growth but no longer count toward high-rise or tower upgrades.
- Why: wild rivers made riverbank tiles objectively the best land on the board — a free permanent qualifying neighbor at every level, and river-adjacent edge tiles needed only 2 real neighbors to reach tower. The AI exploited this relentlessly. Parks keep their wild status since they're limited to 3 per player.

## v0.8.3 — 2026-07-03

- Fixed AI simulation bug: the AI's what-if simulations were handed the wrong inventory object, so simulated moves never spent parks (and the park-legality check inside simulations never fired)
- Room code entry now requires exactly 5 characters, with a clearer error message (was accepting 4–6 and reporting "Room not found" on typos)
- Version label now comes from a single VERSION constant in the code, so it can't fall out of date again
- Fixed stale on-screen rules text: parks boost ALL adjacent buildings (both players'), as changed in v0.7.2
- Synced RULES.md and CLAUDE.md with current rules (park boost, v0.8.1 cascade cap, v0.8.2 park-boost interaction)
- Removed an unused leftover variable
- Backfilled missing v0.8.1 and v0.8.2 changelog entries below

## v0.8.2 — 2026-07-03

- Fixed park boost double-dip: a building boosted by a park no longer also cascade-upgrades in the same turn

## v0.8.1 — 2026-07-03

- Fixed cascade bug: each building can now upgrade at most +1 per turn

## v0.8 — 2026-06-14

- Added single-player mode vs an AI opponent ("Play vs computer" in the lobby)
- AI plays strategically: takes winning moves, blocks the opponent's wins, avoids handing the opponent a tower, develops density, and spends parks judiciously
- Built on a reusable foundation (board-agnostic rule helpers, a side-effect-free move resolver, and a swappable move-picker) so additional difficulty levels can be added without touching game plumbing

## v0.7.3 — 2026-06-14

- Added a subtle version label to the bottom of the UI

## v0.7.2 — 2026-06-14

- Parks boost both players' adjacent buildings (removed owner-only restriction)

## v0.6 — 2026-06-14

- Expanded grid to 20×20 (from 15×15)
- Switched to predetermined river terrain (2 procedurally generated rivers per game)
- Changed upgrade rule from 4-of-4 to 3-of-4 neighbors
- Parks now give +1 density boost to YOUR adjacent buildings when placed
- Removed player-placed rivers
- Limited parks to 3 per developer
- Edge tiles can now upgrade (all 3 neighbors must qualify)

## v0.5 — 2026-06-14

- Rethemed as "Developer 1 vs Developer 2" city-building battle
- Limited parks to 3 per player and rivers to 3 per player
- Parks and rivers act as wild for density checks

## v0.4 — 2026-06-14

- Introduced layered density mechanic: neighbors must be at or above your density level to trigger upgrade
- Cascading upgrades: upgrades chain until no more are possible

## v0.3 — 2026-06-14

- Switched to +1 density per adjacent placement (no surround needed)
- Result: game moved too fast, reverted concept in v0.4

## v0.2 — 2026-06-14

- Fixed upgrade loop bug that caused tiles to jump straight from yellow to purple
- Added upgradeState tracking to limit upgrades to one per surround event
- Result: tiles could never upgrade past level 2 (too restrictive), addressed in v0.4

## v0.1 — 2026-06-14

- Initial prototype
- 15×15 grid
- 4-side surround upgrade rule
- Player-placed rivers and parks (unlimited)
- Two-player hot-seat with alternating turns
