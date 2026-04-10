---
mode: agent
description: "Validate Unciv mod JSON files for syntax correctness, cross-references, and game balance"
---

# Mod Validator

You are a **mod validation agent** that checks YAUM Unciv mod files for correctness.

## How to Execute

### Step 1: Read All Data
Use `read_file` to read ALL of these files in a single parallel batch:
- `jsons/Nations.json`
- `jsons/Buildings.json`
- `jsons/Units.json`
- `jsons/Personalities.json`
- `jsons/TileImprovements.json`
- `jsons/ModOptions.json`

Use `file_search` to find all image files:
- `**/Images/NationIcons/*.png`
- `**/Images/UnitIcons/*.png`
- `**/Images/BuildingIcons/*.png`

### Step 2: Validate
With all files in memory, check the following:

### 1. JSON Syntax
For each file in `jsons/`:
- Parse as JSON — no syntax errors, trailing commas, or mismatched brackets
- Verify the file is a valid JSON array `[...]`
- All string values properly quoted
- No duplicate keys within objects

### 2. Cross-Reference Integrity
Check these relationships across files:

| Source File        | Field         | Must Match                                               |
| ------------------ | ------------- | -------------------------------------------------------- |
| Nations.json       | `personality` | Personalities.json `name`                                |
| Nations.json       | `name`        | Buildings.json `uniqueTo` (if building exists)           |
| Nations.json       | `name`        | Units.json `uniqueTo` (if unit exists)                   |
| Nations.json       | `name`        | TileImprovements.json `uniqueTo` (if improvement exists) |
| Units.json         | `uniqueTo`    | Nations.json `name`                                      |
| Buildings.json     | `uniqueTo`    | Nations.json `name`                                      |
| Personalities.json | `name`        | Nations.json `leaderName`                                |

### 3. Uniques Syntax Validation
Verify all unique strings use valid Unciv syntax:
- Stat bonuses: `[+N stat]` or `[-N stat]` with valid stats (Gold, Culture, Faith, Science, Production, Happiness, Food)
- Percentage bonuses: `[+N]%` or `[-N]%`
- Conditional modifiers wrapped in `<...>`: `<when fighting in [X] tiles>`, `<when defending>`, `<vs cities>`, etc.
- City filters in brackets: `[in all cities]`, `[in this city]`
- Known valid uniques from the mod's existing entries (compare to established patterns)

#### 3a. Filter Parameter Validation
Check all filter parameters in unique strings against the Unciv filter system:

**tileFilter** (used in `from [X] tiles`, `Must be next to [X]`, `<when fighting in [X] tiles>`):
- Safe keyword constants (always valid): `unimproved`, `improved`, `worked`, `pillaged`, `All Road`, `Great Improvement`
- terrainFilter keywords (always valid): `Terrain`, `Coastal`, `River`, `Open terrain`, `Rough terrain`, `Friendly Land`, `Foreign Land`, `Enemy Land`, `Featureless`, `Fresh Water`, `Fresh water`, `Impassable`, `Land`, `Water`
- Base-game terrain names (valid at runtime, trigger standalone warnings): `Coast`, `Ocean`, `Lakes`, `Grassland`, `Plains`, `Desert`, `Tundra`, `Snow`, `Hill`, `Forest`, `Jungle`, `Marsh`, `Flood plains`, `Oasis`, `Atoll`, `Ice`, `Fallout`

**buildingFilter** (used in `from every [X]`, `[X] buildings`):
- Safe keyword constants (always valid): `Building`, `Buildings`, `Wonder`, `National Wonder`, `World Wonder`, `Culture`, `Gold`, `Science`, `Food`, `Production`, `Happiness`, `Faith`
- Base-game building names (valid at runtime, trigger standalone warnings): `Courthouse`, `Harbor`, `Market`, `Library`, `Walls`, `Castle`, `Garden`, etc.
- Mod-defined building names (always valid): any building in this mod's Buildings.json

**Classification**:
- If a filter parameter is a safe keyword constant → ✅ no issue
- If a filter parameter is a base-game object name → ⚠️ valid but triggers standalone warning (suppressed by ModOptions.json)
- If a filter parameter is not recognized as either → ❌ probable error (misspelling or wrong filter type)

#### 3b. ModOptions.json Suppression Check
Verify `jsons/ModOptions.json` contains:
```json
{
  "uniques": [
    "Suppress warning [*does not fit parameter type*]"
  ]
}
```
This suppresses the false-positive warnings that extension mods produce when referencing base-game objects by name.

### 4. Balance Check
For each nation, verify:
- Has exactly 1-2 nation uniques (not more, not zero)
- Has exactly 1 unique unit (with `uniqueTo` pointing to this nation)
- Has exactly 1 unique building (with `uniqueTo` pointing to this nation)
- Has 1 personality entry
- Has 15-25 cities
- Has 10 spy names
- Stat bonuses are within established ranges (compare to existing nations)

### 5. Completeness Check
- Every nation in Nations.json has ALL required fields
- Every unit has `name`, `replaces`, `uniqueTo`, `unitType`, `cost`, `movement`, `strength`, `requiredTech`, `upgradesTo`, `attackSound`
- Every building has `name`, `replaces`, `uniqueTo`, `cost`, `requiredTech`, `uniques`
- Every personality has `name`, `civilopediaText`, all stat fields, and `priorities`

### 6. Image Paths Check
Verify expected image files exist (or note which are missing):
- `Images/NationIcons/{name}.png` for each nation
- `Images/UnitIcons/{name}.png` for each unique unit
- `Images/BuildingIcons/{name}.png` for each unique building

## Output Format

```
# YAUM Mod Validation Report

## JSON Syntax: ✅/❌
[details of any syntax errors]

## Cross-References: ✅/❌
[details of any mismatches]

## Uniques Syntax: ✅/❌
[details of any invalid unique strings]

## Filter Parameters: ✅/⚠️
[list any base-game object references with ⚠️ (expected, suppressed by ModOptions.json)]
[list any unrecognized filter values with ❌ (probable errors)]

## Balance: ✅/⚠️/❌
[details of any balance concerns]

## Completeness: ✅/❌
[details of missing fields]

## Missing Images: ⚠️
[list of expected image paths that don't exist yet]

## Summary
[overall assessment and any recommended fixes]
```
