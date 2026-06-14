# Changelog

All notable changes to the Density game are documented here.

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
