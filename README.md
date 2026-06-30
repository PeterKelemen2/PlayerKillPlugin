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

| Placeholder | Description |
|---|---|
| `%personal_kills%` | Total player kills |
| `%personal_deaths%` | Total deaths (PvP-only by default, see config) |
| `%personal_kd%` | K/D ratio formatted to 2 decimal places; shows raw kill count when deaths = 0 |

---

### Leaderboards

Each leaderboard placeholder follows the pattern `%<identifier>_<query>%`.

**Identifiers**

| Identifier | Ranks by |
|---|---|
| `kills` / `kill` | Most kills |
| `deaths` / `death` | Most deaths |
| `kd` | Highest K/D ratio |

**Queries**

| Query | Returns |
|---|---|
| `top_10` | Newline-separated list of the top 10 players and their stat value |
| `top_name` | Display name of the #1 player |
| `top_value` | Stat value of the #1 player |

**Examples**

| Placeholder | Description |
|---|---|
| `%kills_top_10%` | Top 10 players by kill count |
| `%kills_top_name%` | Name of the player with the most kills |
| `%kills_top_value%` | Kill count of the #1 killer |
| `%kd_top_10%` | Top 10 players by K/D ratio |
| `%kd_top_name%` | Name of the player with the highest K/D |
| `%kd_top_value%` | K/D ratio of the #1 player |
| `%deaths_top_10%` | Top 10 players by death count |
| `%deaths_top_name%` | Name of the player with the most deaths |
| `%deaths_top_value%` | Death count of the #1 player |

`kill` and `death` are aliases for `kills` and `deaths` respectively and return identical values.

## Building

```text
./gradlew build
```

The JAR is output to `build/libs/`.
