# Unciv Mod: YAUM — Workspace Knowledge Base

## Project Overview

This is an **Unciv extension mod** (Yet Another Unciv Mod). Unciv is an open-source reimplementation of Civilization V.
Extension mods add new nations, units, buildings, and improvements to the base game ruleset without replacing it.

Repository: `IrfEazy/yaum` — GitHub topic: `unciv-mod`

## Current Nations in This Mod

| Nation             | Leader                   | Victory    | Unique Name                | Key Uniques                                                         |
| ------------------ | ------------------------ | ---------- | -------------------------- | ------------------------------------------------------------------- |
| Sumer              | Gilgamesh                | Scientific | Epic Quests                | Gold from barbarian kills, barbarian camp notifications             |
| League of Lezhë    | Skanderbeg               | Domination | League of Lezhë            | +20% strength in friendly land, double Great General bonus          |
| Florence           | Lorenzo de' Medici       | Cultural   | Magnificence               | +25% Great Person generation, +1 Gold from specialists              |
| Kingdom of Naples  | Charles III              | Cultural   | Enlightenment of the South | +20% culture building production, +1 culture from culture buildings |
| Republic of Venice | Enrico Dandolo           | Diplomatic | Serenissima                | +2 Gold from coast tiles, -25% purchase cost                        |
| Ottoman Empire     | Suleiman the Magnificent | Domination | Kanuni                     | +1 Happiness from Courthouses, -20% unit maintenance                |
| Crown of Castile   | Isabella I               | Domination | Reconquista                | +20% strength in enemy land, +1 Faith in all cities                 |
| Kingdom of Portugal | Manuel I                 | Diplomatic | Age of Discovery           | +1 Science from coast tiles, +2 Gold from Harbors                   |

## Mod File Structure

```
yaum/
├── jsons/
│   ├── Nations.json          # Nation definitions (name, leader, colors, cities, uniques)
│   ├── Buildings.json        # Unique buildings (replaces base buildings)
│   ├── Units.json            # Unique units (replaces base units)
│   ├── Personalities.json    # AI personality for each leader
│   └── TileImprovements.json # Unique tile improvements
├── Images/
│   ├── NationIcons/          # 100x100px, white on transparent
│   ├── UnitIcons/            # 200x200px, white on transparent
│   ├── BuildingIcons/        # 200x200px, white on transparent
│   ├── ImprovementIcons/     # 200x200px, white on transparent
│   ├── ImprovementPortraits/ # Improvement portrait images
│   └── TileSets/HexaRealm/  # Tileset graphics
├── Atlases.json              # Texture atlas config: [game]
├── credits.md                # Icon/sound attribution (Creative Commons)
├── game.atlas                # Texture atlas
└── README.md                 # Mod description
```

## JSON Schemas

### Nations.json Entry
```json
{
  "name": "Nation Name",
  "leaderName": "Leader Name",
  "adjective": ["Adjectival Form"],
  "outerColor": [R, G, B],        // RGB 0-255, outer ring of icon
  "innerColor": [R, G, B],        // RGB 0-255, inner circle of icon
  "startBias": ["Terrain"],        // e.g. Plains, Hill, Coast, River, Grassland
  "preferredVictoryType": "Type",  // Scientific, Cultural, Domination, Diplomatic
  "favoredReligion": "Religion",
  "personality": "PersonalityName", // Must match Personalities.json
  "uniqueName": "Ability Name",
  "uniques": ["unique1", "unique2"],
  "cities": ["City1", "City2", ...], // 15-25 historical city names
  "spyNames": ["Spy1", ...],         // 10 historical figure names
  "startIntroPart1": "Historical introduction paragraph 1",
  "startIntroPart2": "Call to action paragraph 2",
  "introduction": "Leader's greeting when first met",
  "declaringWar": "War declaration dialogue",
  "attacked": "Response when attacked",
  "defeated": "Defeat dialogue",
  "neutralHello": "Neutral greeting",
  "hateHello": "Hostile greeting",
  "tradeRequest": "Trade proposal dialogue"
}
```

### Buildings.json Entry (Unique Building)
```json
{
  "name": "Unique Building Name",
  "replaces": "Base Building",     // e.g. Castle, Garden, Harbor, Opera House
  "uniqueTo": "Nation Name",
  "cost": 120,
  "maintenance": 1,
  "requiredTech": "Tech Name",
  "requiredBuilding": "Prerequisite",
  "uniques": ["unique1", "unique2"],
  // Optional stat fields: culture, happiness, cityStrength, cityHealth
  // Optional: "specialistSlots": {"Artist": 1}
  // Optional: "hurryCostModifier": 25
}
```

### Units.json Entry (Unique Unit)
```json
{
  "name": "Unique Unit Name",
  "replaces": "Base Unit",        // e.g. Knight, Musketman, Frigate
  "uniqueTo": "Nation Name",
  "unitType": "Type",             // Mounted, Gunpowder, Ranged Water, Melee, etc.
  "cost": 100,
  "movement": 4,
  "strength": 20,
  "requiredTech": "Tech Name",
  "obsoleteTech": "Tech Name",
  "upgradesTo": "Next Unit",
  "attackSound": "sound",         // horse, shot, shipCannonVolley
  "uniques": ["unique1", "unique2"],
  // Optional: "rangedStrength", "range", "requiredResource", "promotions"
}
```

### Personalities.json Entry
```json
{
  "name": "Leader Name",          // Must match leaderName in Nations.json
  "civilopediaText": [{"text": "1-2 sentence historical bio"}],
  // Stats (Float, 0-10 scale):
  "aggressive": 5, "commerce": 5, "culture": 5, "declareWar": 5,
  "diplomacy": 5, "expansion": 5, "faith": 5, "food": 5,
  "gold": 5, "happiness": 5, "loyal": 5, "military": 5,
  "production": 5, "science": 5,
  // Policy priorities (Integer, higher = preferred):
  "priorities": {
    "Tradition": 5, "Liberty": 5, "Honor": 5, "Piety": 5,
    "Patronage": 5, "Commerce": 5, "Rationalism": 5,
    "Freedom": 5, "Order": 5, "Autocracy": 5
  }
}
```

### TileImprovements.json Entry
```json
{
  "name": "Improvement Name",
  "uniqueTo": "Nation Name",
  "techRequired": "Tech Name",
  "terrainsCanBeBuiltOn": ["Plains", "Grassland"],
  "turnsToBuild": 6,
  "uniques": ["unique1"],
  // Optional stat fields: science, culture, gold, food, production, faith
  // Optional: "shortcutKey": "Z"
}
```

## Common Uniques Reference

### Nation Uniques (Global)
- `[+N]% Strength <when fighting in [Friendly Land/Enemy Land] tiles>`
- `[+N]% Great Person generation [in all cities]`
- `[+N Gold/Culture/Faith/Science/Production] [in all cities]`
- `[+N stat] from every [specialist/buildingFilter] [in all cities]`
- `[-N]% maintenance costs <for [All] units>`
- `[+N Happiness] from every [buildingFilter]`
- `Earn [N]% of killed [unitFilter] unit's [Strength] as [Gold]`
- `Great General provides double combat bonus`
- `Notified of new Barbarian encampments`
- `[Gold] cost of purchasing items in cities [-N]%`
- `[+N stat] from [tileFilter] tiles [in all cities]`

### Unit Uniques
- `Can move after attacking`
- `No defensive terrain bonus`
- `[+N]% Strength <when defending>`
- `[+N]% Strength <vs cities>`
- `[+N]% Strength <when fighting in [terrain] tiles>`
- `Heals [N] damage if it kills a unit`
- `Withdraws before melee combat <with [N]% chance>`
- `[-N] Range`

### Building Uniques
- `[+N Production/Culture/Faith/Gold/Science]`
- `[+N]% Great Person generation [in this city]`
- `Destroyed when the city is captured`
- `Connects trade routes over water`
- `Must be next to [Coast]`
- `[+N]% Production when constructing [filter] units [in this city]`
- `[+N stat] from [tileFilter] tiles [in this city]`

### Improvement Uniques
- `[+N Culture/Science/Gold] <in [Fresh Water] tiles>`
- `Pillaging this improvement yields approximately [+N Gold]`

### Conditional Modifiers
- `<when fighting in [terrainFilter] tiles>`
- `<when defending>`
- `<vs cities>`
- `<with [N]% chance>`
- `<in [Fresh Water] tiles>`
- `<for [All/Military] units>`

## Design Conventions in This Mod

1. **Each nation gets**: 1 unique unit, 1 unique building, 1 personality, optionally 1 improvement
2. **Unique units** replace existing base-game units (Knight, Musketman, Frigate, etc.)
3. **Unique buildings** replace existing base-game buildings (Castle, Garden, Harbor, etc.)
4. **Colors**: outerColor is the primary, innerColor is the accent — choose historically appropriate colors
5. **Cities**: 15-25 real historical cities, capital first
6. **Spy names**: 10 famous historical figures from that civilization
7. **Flavor text**: In-character dialogue reflecting the leader's personality and historical context
8. **Balance**: Uniques should be comparable in power to existing nations — not overpowered
9. **Icon attribution**: All icons must be open-source (Noun Project, etc.) and credited in credits.md

## Image Requirements

- **NationIcons**: 100×100px, white silhouette on transparent background
- **UnitIcons**: 200×200px, white silhouette on transparent background
- **BuildingIcons**: 200×200px, white silhouette on transparent background
- **ImprovementIcons**: white silhouette on transparent background
- Source: [The Noun Project](https://thenounproject.com/) or similar open-source

## Development Workflow Prompts

| Prompt                    | Purpose                                                                                                                       |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `pre-commit-orchestrator` | Validates JSON, cross-references, images, then organizes gitmoji commits and updates docs — run before every commit/push      |
| `gitmoji-commit`          | Organizes staged changes into atomic gitmoji commits following [gitmoji.dev/specification](https://gitmoji.dev/specification) |
| `update-docs`             | Incrementally updates README, copilot-instructions, and credits with new content — targeted additions only                    |
| `validate-mod`            | Validates mod JSON files for syntax, cross-references, uniques, balance, and completeness                                     |

### Gitmoji Commit Convention

Commits follow the [gitmoji specification](https://gitmoji.dev/specification): `<emoji> [scope?][:?] <message>`

Key emojis: ✨ new feature, 🐛 bug fix, 📝 docs, 🎨 art/icons, ⚡️ balance, ♻️ refactor, 🔧 config, 👷 CI
