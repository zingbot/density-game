# Density

A two-player competitive city-building grid game. Two developers battle to build the densest urban block on a shared 20×20 grid. First to build a **Tower** (density level 4) wins.

## How to Play

1. Open `index.html` in any browser — no server, no install, no dependencies.
2. Players alternate turns placing **buildings**, which start as low-rise (density 1).
3. A building upgrades when **3 of its 4 neighbors** are occupied at or above its density level.
4. Upgrades cascade — one placement can trigger a chain reaction.
5. Each player has **3 parks** that give +1 density to ALL adjacent buildings — yours and your opponent's — when placed.
6. Each player has **2 bulldozers** — spend your turn to demolish one of your own buildings (below tower). Demolishing never downgrades neighbors.
7. **Rivers** are randomly generated terrain that can't be built on; they count as low-rise (level 1) neighbors for density checks.
8. First player to upgrade a building to **Tower (density 4)** wins.

## Density Levels

| Level | Name | Player 1 | Player 2 |
|-------|------|----------|----------|
| 1 | Low-rise | 🟡 Pale amber | 🩷 Pale violet |
| 2 | Mid-rise | 🟠 Orange | 🟣 Bright violet |
| 3 | High-rise | 🟤 Bronze | 🟣 Deep violet |
| 4 | Tower ★ | 🟤 Dark umber | 🟣 Electric violet |

Terrain owns the nature hues — river is blue, parks are green — so ownership colors never collide with the map.

## Project

- **Game concept:** Adrian
- **Prototype:** Built collaboratively with Claude
- **Project tracker:** Notion (source of truth for project management, decisions, and roadmap)
- **This repo:** Source of truth for code

## Firebase Setup (Multiplayer)

The database security rules live in [database.rules.json](database.rules.json). Deploy them either by:

- **Firebase CLI:** `firebase deploy --only database` (config in `firebase.json` / `.firebaserc`), or
- **Console:** paste the contents of `database.rules.json` into **Realtime Database → Rules** and publish.

**Important:** when a release changes the synced state shape (board cell types, inventory fields), deploy the rules *before* publishing the client — the rules reject unknown fields, so an old ruleset silently breaks multiplayer for the new client.

## Development

No build step. Edit `index.html` and refresh.

See [RULES.md](RULES.md) for the full game rules and [CHANGELOG.md](CHANGELOG.md) for version history.

## License

MIT — see [LICENSE](LICENSE).
