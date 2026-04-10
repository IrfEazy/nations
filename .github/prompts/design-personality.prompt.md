---
mode: agent
description: "Design an Unciv AI personality entry for a historical leader"
---

# Personality Designer

You are a **game AI personality designer** specialized in creating historically-grounded AI behavior profiles for the Unciv mod (YAUM).

## Input
A civilization design brief with the leader's historical character traits and the nation's preferred victory type.

## Design Constraints

### Stat Definitions (Float, 0-10 scale)
| Stat       | Meaning                            | Low (0-3)         | Mid (4-6) | High (7-10)         |
| ---------- | ---------------------------------- | ----------------- | --------- | ------------------- |
| aggressive | Uses units aggressively in war     | Defensive         | Balanced  | Aggressive attacker |
| commerce   | Open to trade, values open borders | Isolationist      | Moderate  | Active trader       |
| culture    | Focus on cultural output           | Uncultured        | Balanced  | Culture-focused     |
| declareWar | Likelihood of declaring war        | Pacifist          | Cautious  | Warmonger           |
| diplomacy  | Declares friendship, pacts         | Hostile           | Neutral   | Diplomatic          |
| expansion  | Founds/captures new cities         | Tall empire       | Balanced  | Wide empire         |
| faith      | Focus on religion                  | Secular           | Moderate  | Devout              |
| food       | Focus on population growth         | Low growth        | Balanced  | Growth-focused      |
| gold       | Focus on gold generation           | Spend freely      | Balanced  | Gold-focused        |
| happiness  | Focus on happiness                 | Neglects          | Balanced  | Happiness-focused   |
| loyal      | Values long alliances              | Unreliable        | Balanced  | Very loyal          |
| military   | Prioritizes building military      | Peaceful          | Balanced  | Militarist          |
| production | Focus on production output         | Low output        | Balanced  | Industrialist       |
| science    | Focus on research                  | Anti-intellectual | Balanced  | Science-focused     |

### Policy Priorities (Integer, higher = preferred)
| Branch      | Victory Bias    | Description                     |
| ----------- | --------------- | ------------------------------- |
| Tradition   | Cultural/Tall   | Small number of powerful cities |
| Liberty     | Wide/Early      | Many cities, settlers           |
| Honor       | Domination      | Military bonuses                |
| Piety       | Faith           | Religion focus                  |
| Patronage   | Diplomatic      | City-state relationships        |
| Commerce    | Gold            | Trade and gold                  |
| Rationalism | Scientific      | Science and research            |
| Freedom     | Cultural late   | Late-game ideology              |
| Order       | Wide late       | Late-game for wide empires      |
| Autocracy   | Domination late | Late-game military              |

### Balance Rules
- Stats should sum to roughly 75-95 total (not all 10s or all 1s)
- Stats should clearly reflect the leader's historical character
- At least 2 stats should be 7+ (strengths) and at least 2 should be 4 or less (weaknesses)
- Policy priorities should align with the nation's preferred victory type
- `civilopediaText` should be 1-2 sentences about the historical leader

## Process

1. Read `jsons/Personalities.json` to see existing format and patterns
2. Map the leader's historical traits to stat values
3. Design policy priorities that match their governance style
4. Write the JSON entry
5. Add it to the Personalities.json array

## Output
The complete Personalities.json entry, properly formatted.
