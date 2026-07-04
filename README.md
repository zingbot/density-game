# Density

A two-player competitive city-building grid game. Two developers battle to build the densest urban block on a shared 20×20 grid. First to build a **Tower** (density level 4) wins.

## How to Play

1. Open `index.html` in any browser — no server, no install, no dependencies.
2. Players alternate turns placing **buildings**, which start as low-rise (density 1).
3. A building upgrades when **3 of its 4 neighbors** are occupied at or above its density level.
4. Upgrades cascade — one placement can trigger a chain reaction.
5. Each player has **3 parks** that give +1 density to your own adjacent buildings when placed.
6. **Rivers** are randomly generated terrain that can't be built on but count as wild for density checks.
7. First player to upgrade a building to **Tower (density 4)** wins.

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
- **Project tracker:** [Google Doc](https://docs.google.com/document/d/1w-uUtwqzhbHBsa6_QBge0BWkrWKSwFPEYZVzOLWut7Y/edit) (source of truth for project management, decisions, and roadmap)
- **This repo:** Source of truth for code

## Firebase Setup (Multiplayer)

In the Firebase console, go to **Realtime Database → Rules** and replace the rules with:

```json
{
  "rules": {
    "rooms": {
      "$roomId": {
        ".read": true,
        ".write": true,
        ".validate": "newData.hasChildren(['state', 'players'])",
        "state": {
          ".validate": "newData.hasChild('board') && newData.hasChild('currentPlayer')"
        }
      }
    },
    ".read": false,
    ".write": false
  }
}
```

## Development

No build step. Edit `index.html` and refresh.

See [RULES.md](RULES.md) for the full game rules and [CHANGELOG.md](CHANGELOG.md) for version history.

## License

MIT — see [LICENSE](LICENSE).
