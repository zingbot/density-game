# Density — Game Rules (v0.6)

## Overview

Density is a two-player competitive city-building game on a 20×20 grid. Two developers race to upgrade a building to **Tower** (density level 4) by strategically placing tiles and surrounding existing buildings.

## Setup

- The board is a 20×20 grid.
- Two rivers are procedurally generated across the board at game start, creating unique terrain each game.
- Each player starts with **3 parks** in their inventory.
- Player 1 (warm colors) goes first.

## Tile Types

### Building
- Placed by either player on any empty non-river cell.
- Starts at density 1 (low-rise).
- Can upgrade through the density levels via the surround mechanic.
- Belongs to the player who placed it. Ownership never changes.

### Park
- Placed by either player. Limited to **3 per developer**.
- When placed, all **your own** adjacent buildings immediately receive +1 density.
- This boost can trigger cascading upgrades.
- Parks count as occupied for surround checks and satisfy any density level (wild).
- Parks cannot be upgraded and have no density level of their own.

### River
- Predetermined terrain generated at game start.
- Cannot be built on.
- Counts as occupied for surround checks and satisfies any density level (wild).

## Density Levels

| Level | Name      | Player 1 Color | Player 2 Color |
|-------|-----------|----------------|----------------|
| 1     | Low-rise  | Yellow         | Mint           |
| 2     | Mid-rise  | Orange         | Teal           |
| 3     | High-rise | Red            | Blue           |
| 4     | Tower ★   | Purple         | Indigo         |

## Turn Structure

1. Players alternate turns. One placement per turn.
2. On your turn, place either a **Building** or a **Park** on any empty, non-river cell.
3. After placement, the upgrade cascade resolves automatically.
4. Play passes to the other player.

## Upgrade Rule

A building upgrades +1 density when **3 of its 4 orthogonal neighbors** are occupied by tiles at or above its current density level.

- Only orthogonal neighbors count (up, down, left, right). Diagonals do not count.
- Rivers and parks count as occupied and satisfy any density level (wild) for upgrade checks.
- Any player's buildings count — you can be upgraded by your opponent's tiles surrounding yours.
- Upgrades cascade: if an upgrade causes another tile to meet its upgrade condition, that tile upgrades too. This continues until no more upgrades are possible.

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
- Parks are powerful tempo plays — save them for moments where the +1 boost can trigger a cascade toward tower.
- Rivers create natural chokepoints and districts. Building along a river means one side is already "occupied" for surround checks.
- Edge tiles only need 3 qualifying neighbors (which is all they have), so river-adjacent edges can be strong positions.
