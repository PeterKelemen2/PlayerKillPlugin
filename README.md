# KillRatio

A Paper plugin that tracks per-player PvP kill and death statistics and exposes them through [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/).

## Requirements

- Paper 1.18.2+
- [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) (optional — placeholders are disabled without it)

## Installation

1. Drop `KillRatio.jar` into your `plugins/` folder.
2. Restart the server.
3. Stats are saved automatically every 5 minutes and on server shutdown.

## Configuration

`plugins/KillRatio/config.yml`:

```yaml
# If true (default), only deaths caused directly by another player count toward %personal_deaths%.
# Set to false to count all deaths (fall damage, mobs, etc.).
pvp-deaths-only: true
```

---

## Placeholders

### Personal stats

These are per-player and resolve to the stats of the player requesting the placeholder.

| Placeholder              | Description                                                                   |
| ------------------------ | ----------------------------------------------------------------------------- |
| `%personal_kills%`       | Total player kills                                                            |
| `%personal_deaths%`      | Total deaths (PvP-only by default, see config)                                |
| `%personal_kd%`          | K/D ratio formatted to 2 decimal places; shows raw kill count when deaths = 0 |
| `%personal_kills_rank%`  | The player's own 1-indexed rank on the kills leaderboard                      |
| `%personal_deaths_rank%` | The player's own 1-indexed rank on the deaths leaderboard                     |
| `%personal_kd_rank%`     | The player's own 1-indexed rank on the K/D leaderboard                        |

Players tied on a stat share the same rank (e.g. two players tied for the most kills are both rank 1).

---

### Leaderboards

Each leaderboard placeholder follows the pattern `%<identifier>_<query>%`.

**Identifiers**

| Identifier         | Ranks by          |
| ------------------ | ----------------- |
| `kills` / `kill`   | Most kills        |
| `deaths` / `death` | Most deaths       |
| `kd`               | Highest K/D ratio |

**Queries**

| Query                    | Returns                                                                           |
| ------------------------ | --------------------------------------------------------------------------------- |
| `top_<n>`                | A **list**: all of the top `n` players and their stat value, one per line         |
| `top_<n>_name`           | A **single value**: just the name of whichever player is ranked exactly `n`       |
| `top_<n>_value`          | A **single value**: just the stat value of whichever player is ranked exactly `n` |
| `top_name` / `top_value` | Aliases for `top_1_name` / `top_1_value` (the #1 player)                          |

`n` is whatever number you put in the placeholder — `top_2_name` means rank #2, `top_7_name` means rank #7, and so on.

The important difference: `top_<n>` returns everyone _from #1 down to #n_ as one multi-line block, while `top_<n>_name` / `top_<n>_value` return _only the player at that one rank_ — no list, no other ranks. If fewer than `n` players are tracked (e.g. you ask for `top_5_name` but only 2 players have kills), it resolves to an empty string.

**Worked example** — say the kills leaderboard currently looks like this:

| Rank | Player | Kills |
| ---- | ------ | ----- |
| #1   | Alice  | 42    |
| #2   | Bob    | 17    |
| #3   | Carol  | 9     |

| Placeholder           | Resolves to                                                           |
| --------------------- | --------------------------------------------------------------------- |
| `%kills_top_3%`       | `Alice - 42`<br>`Bob - 17`<br>`Carol - 9` (all 3 lines, as one block) |
| `%kills_top_2_name%`  | `Bob` (just Bob — rank #2's name, nothing else)                       |
| `%kills_top_2_value%` | `17` (just Bob's kill count)                                          |
| `%kills_top_1_name%`  | `Alice` (same as `%kills_top_name%`)                                  |
| `%kills_top_5_name%`  | _(empty string — there's no #5 player yet)_                           |

Use `top_<n>_name` / `top_<n>_value` when you want to build your own leaderboard layout out of separate placeholders — for example, three sign lines each showing one rank:

```text
Line 1: #1 %kills_top_1_name% - %kills_top_1_value%
Line 2: #2 %kills_top_2_name% - %kills_top_2_value%
Line 3: #3 %kills_top_3_name% - %kills_top_3_value%
```

which would render as:

```text
#1 Alice - 42
#2 Bob - 17
#3 Carol - 9
```

— the same data as `%kills_top_3%`, just split across separately positioned placeholders instead of one combined block.

**Examples**

| Placeholder           | Description                             |
| --------------------- | --------------------------------------- |
| `%kills_top_10%`      | Top 10 players by kill count            |
| `%kills_top_5%`       | Top 5 players by kill count             |
| `%kills_top_2_name%`  | Name of the player ranked #2 by kills   |
| `%kills_top_2_value%` | Kill count of the player ranked #2      |
| `%kd_top_10%`         | Top 10 players by K/D ratio             |
| `%kd_top_name%`       | Name of the player with the highest K/D |
| `%kd_top_value%`      | K/D ratio of the #1 player              |
| `%deaths_top_10%`     | Top 10 players by death count           |
| `%deaths_top_name%`   | Name of the player with the most deaths |
| `%deaths_top_value%`  | Death count of the #1 player            |

`kill` and `death` are aliases for `kills` and `deaths` respectively and return identical values.

## Building

```text
./gradlew build
```

The JAR is output to `build/libs/`.
