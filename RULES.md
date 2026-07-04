# Density — Game Rules (v0.13)

## Overview

Density is a two-player competitive city-building game on a 20×20 grid. Two developers race to upgrade a building to **Tower** (density level 4) by strategically placing tiles and surrounding existing buildings.

## Setup

- The board is a 20×20 grid.
- Two rivers are procedurally generated across the board at game start, creating unique terrain each game.
- Each player starts with **3 parks** and **2 bulldozers** in their inventory.
- Player 1 (amber) goes first.

## Tile Types

### Building
- Placed by either player on any empty non-river cell.
- Starts at density 1 (low-rise).
- Can upgrade through the density levels via the surround mechanic.
- Belongs to the player who placed it. Ownership never changes.

### Park
- Placed by either player. Limited to **3 per developer**.
- When placed, **all adjacent buildings — yours and your opponent's** — immediately receive +1 density (changed in v0.7.2; was owner-only before).
- This boost can trigger cascading upgrades in other tiles, but a building boosted by the park cannot also cascade-upgrade in the same turn (v0.8.2).
- Parks count as occupied for surround checks and satisfy any density level (wild).
- Parks cannot be upgraded and have no density level of their own.

### River
- Predetermined terrain generated at game start.
- Cannot be built on.
- Counts as occupied for surround checks, but only as a **low-rise (level 1)** neighbor (changed in v0.9; was wild before). Rivers help buildings reach mid-rise but contribute nothing toward high-rise or tower upgrades.

### Rubble (v0.13, rubble variant only)
- Created when a building is bulldozed with the `rubble` variant flag enabled (off by default — bulldozed tiles normally return to empty).
- Cannot be built on while present.
- Counts as **nothing** for upgrade checks — not occupied, satisfies no density level.
- Clears automatically after **2 completed turns**, returning the tile to empty.

## Density Levels

| Level | Name      | Player 1 Color | Player 2 Color |
|-------|-----------|----------------|----------------|
| 1     | Low-rise  | Pale amber     | Pale violet    |
| 2     | Mid-rise  | Orange         | Bright violet  |
| 3     | High-rise | Bronze         | Deep violet    |
| 4     | Tower ★   | Dark umber     | Electric violet |

Darker = denser (v0.9.1). Terrain owns the nature hues: river is blue, parks are green, rubble is neutral gray.

## Turn Structure

1. Players alternate turns. One action per turn.
2. On your turn, either place a **Building** or a **Park** on any empty, non-river cell — or spend a **Bulldozer** charge to demolish a building (see below).
3. After a placement, the upgrade cascade resolves automatically. Bulldozing triggers no cascade.
4. Play passes to the other player.

## Bulldozer (v0.13 prototype)

- Each player has **2 bulldozer charges** per game.
- Using one is your **entire turn**: choose a qualifying building and demolish it. The tile returns to empty (or becomes rubble for 2 turns, under the rubble variant).
- **Densities are sticky:** demolishing a building never retroactively downgrades neighbors it helped upgrade.
- Parks and rivers cannot be bulldozed.
- The demolished building comes off its owner's building count.

The bulldozer is being playtested under variant flags (the `BULLDOZER` constant in `index.html`):

| Flag | Default | Options |
|------|---------|---------|
| `charges` | 2 | 1–3 per player |
| `target` | `'own'` — your own buildings below tower | `'opponent-lowrise'` — opponent density-1 buildings only |
| `rubble` | `false` — tile returns to empty | `true` — tile becomes rubble for 2 turns |

These are prototype rules; final rules to be reconciled with Adrian's proposal before v1.0.

## Upgrade Rule

A building upgrades +1 density when **3 of its 4 orthogonal neighbors** are occupied by tiles at or above its current density level.

- Only orthogonal neighbors count (up, down, left, right). Diagonals do not count.
- Parks count as occupied and satisfy any density level (wild) for upgrade checks.
- Rivers count as occupied but only at **low-rise level (1)** — they qualify only for the low-rise → mid-rise upgrade (v0.9).
- Any player's buildings count — you can be upgraded by your opponent's tiles surrounding yours.
- Upgrades cascade: if an upgrade causes another tile to meet its upgrade condition, that tile upgrades too. This continues until no more upgrades are possible.
- Each building can upgrade at most **+1 per turn** (v0.8.1) — a tile that upgraded this turn can't upgrade again until a later turn.

### Upgrade Thresholds

| Current Level | Upgrade To | Requirement |
|---------------|------------|-------------|
| 1 (Low-rise)  | 2 (Mid-rise) | 3 neighbors occupied by anything |
| 2 (Mid-rise)  | 3 (High-rise) | 3 neighbors at mid-rise (2) or higher |
| 3 (High-rise) | 4 (Tower) | 3 neighbors at high-rise (3) or higher |

### Edge and Corner Rules

- **Edge tiles** (3 orthogonal neighbors): Can upgrade if all 3 neighbors qualify.
- **Corner tiles** (2 orthogonal neighbors): Cannot upgrade — they can never reach the 3-neighbor threshold.

## Win Condition

The first player to have any of their buildings reach **Tower (density 4)** wins immediately. The game ends as soon as a tower is built, even if it happens mid-cascade.

## Strategy Notes

- Building near your own tiles creates density clusters, but building near your opponent's tiles can inadvertently upgrade theirs.
- Parks are powerful tempo plays — save them for moments where the +1 boost can trigger a cascade toward tower. But place them carefully: they boost your opponent's adjacent buildings too.
- Rivers create natural chokepoints and districts. Building along a river gives you a free neighbor toward your first upgrade — but rivers don't help beyond mid-rise, so riverbank buildings need real density behind them to reach tower.
- Edge tiles only need 3 qualifying neighbors (which is all they have), so river-adjacent edges are strong early positions — but no longer a shortcut to tower (since v0.9).
