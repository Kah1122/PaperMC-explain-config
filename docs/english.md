# 📄 PaperMC Annotated Configuration Reference

A fully commented reference for PaperMC's `paper-global.yml` and `paper-world.yml` configuration files. Designed for server administrators of all experience levels.

> **Paper version:** Latest (1.21+)  
> **Applies to:** All server types — Survival, Farms, Minigame, PvP

---

## 📁 File Structure

```
server/
├── config/
│   ├── paper-global.yml     ← Affects the entire server
│   └── paper-world-defaults.yml  ← Affects all worlds (can be overridden per world)
└── world/
    └── paper-world.yml      ← Per-world overrides
```

---

## 🌐 paper-global.yml

### `anticheat.obfuscation.items`

| Option | Default | Description |
|--------|---------|-------------|
| `enable-item-obfuscation` | `false` | Hides item data (enchants, lore, etc.) from other players' clients. Prevents cheat clients from scanning nearby players' items. **May break resource packs.** |
| `sanitize-count` | `true` | Hides the exact item stack count from other players. |
| `also-obfuscate` | `[]` | Extra data components to hide. Not recommended unless you know what you're doing. |
| `dont-obfuscate` | `[minecraft:lodestone_tracker]` | Components to always reveal. Lodestone trackers are excluded by default because hiding them causes compasses to shake. |

**Elytra override** — Elytras with 1 durability have a cracked texture, so `minecraft:damage` is excluded from obfuscation by default to preserve the visual.

---

### `block-updates`

| Option | Default | Description |
|--------|---------|-------------|
| `disable-chorus-plant-updates` | `false` | Stops chorus plants from updating block state. Useful for map makers who want custom configurations. |
| `disable-mushroom-block-updates` | `false` | Same as above, for mushroom blocks. |
| `disable-noteblock-updates` | `false` | Stops note blocks from updating. For map makers only. |
| `disable-tripwire-updates` | `false` | Stops tripwires from updating. For map makers only. |

> ⚠️ These are only useful for adventure/map servers. Leave `false` on survival servers.

---

### `chunk-loading-advanced`

| Option | Default | Description |
|--------|---------|-------------|
| `auto-config-send-distance` | `true` | Matches chunk send radius to each client's view distance setting if lower than server's. Saves bandwidth. |
| `player-max-concurrent-chunk-generates` | `0` | Max parallel chunk generations per player. `0` = auto, `-1` = unlimited. |
| `player-max-concurrent-chunk-loads` | `0` | Max parallel chunk loads per player. `0` = auto, `-1` = unlimited. |

---

### `chunk-loading-basic`

| Option | Default | Description |
|--------|---------|-------------|
| `player-max-chunk-generate-rate` | `-1` | Max new chunks generated per second per player. `-1` = unlimited. Set to `8–15` on weaker servers or when players use Elytra frequently. |
| `player-max-chunk-load-rate` | `100` | Max chunks loaded from disk per second per player. Reduce to `50` on servers with 20+ players to avoid I/O spikes. |
| `player-max-chunk-send-rate` | `75` | Max chunks sent to a client per second. Reduce to `40–50` if bandwidth is limited or many players join at once. |

---

### `chunk-system`

| Option | Default | Description |
|--------|---------|-------------|
| `io-threads` | `-1` | Threads for reading/writing chunk files to disk. `-1` = 1 thread. Increase to `2–3` if using a slow HDD. |
| `worker-threads` | `-1` | Threads for generating new chunk terrain. `-1` = auto (½ of physical CPU cores, minimum 1). Increase if many players are exploring simultaneously. |

> **Note:** These run independently from the main server thread. They handle chunk I/O and terrain generation without blocking gameplay.

---

### `collisions`

| Option | Default | Description |
|--------|---------|-------------|
| `enable-player-collisions` | `true` | Whether players physically collide with each other. May conflict with some scoreboard plugins. |
| `send-full-pos-for-hard-colliding-entities` | `true` | Sends precise positions for boats/minecarts to reduce client-server desync. Uses slightly more bandwidth but prevents glitchy collisions. |

---

### `misc`

| Option | Default | Description |
|--------|---------|-------------|
| `chat-executor-core-size` | `-1` | Core thread count for async chat processing. `-1` = 0 threads (uses caller thread). |
| `chat-executor-max-size` | `-1` | Max threads for chat. `-1` = unlimited. |
| `compression-level` | `default` | Network packet compression level. `default` = no compression (`-1`). Set `1–6` to save bandwidth at the cost of CPU. |
| `max-joins-per-tick` | `5` | Max players allowed to join in a single tick. Extra players are queued, not kicked. Prevents join spikes. |
| `region-file-cache-size` | `256` | How many region files stay open in memory. Increase to `512` on servers with lots of explored land and enough RAM. |
| `send-full-pos-for-item-entities` | `false` | Sends precise positions for dropped items. Enable if item positions appear desynced. |
| `xp-orb-groups-per-area` | `default` | Max XP orb groups (of same value) per area before merging. Default is 40. |

---

### `packet-limiter`

| Option | Default | Description |
|--------|---------|-------------|
| `max-packet-rate` | `500` | Max packets per player within the interval. Protects against packet flood attacks. |
| `interval` | `7.0` | Time window (seconds) for rate limiting. |
| `action` | `KICK` | What to do when limit is exceeded. `DROP` = silently ignore, `KICK` = disconnect player. |

> The `minecraft:place_recipe` packet has its own override — recipe spammers can be caught separately from general packet floods.

---

### `player-auto-save`

| Option | Default | Description |
|--------|---------|-------------|
| `rate` | `-1` | How often (in ticks) player data is saved. `-1` uses `bukkit.yml`'s `ticks-per.autosave`. |
| `max-per-tick` | `-1` | Max players saved per tick. `-1` = auto (10 or 20 based on rate). Reduce to `5` on large servers to spread out save load. |

---

### `spam-limiter`

| Option | Default | Description |
|--------|---------|-------------|
| `incoming-packet-threshold` | `300` | Packets per interval before they are considered spam and ignored. |
| `tab-spam-limit` | `500` | How many tab-completions before a player is kicked for spamming. |
| `recipe-spam-limit` | `20` | How many recipe clicks before a player is kicked for spamming. |

---

### `watchdog`

| Option | Default | Description |
|--------|---------|-------------|
| `early-warning-delay` | `10000` | Milliseconds of server hang before the watchdog starts printing thread dumps. |
| `early-warning-every` | `5000` | Interval (ms) between thread dump prints while the server is hanging. |

> The watchdog monitors the main thread. If it stops responding, it logs what's happening and eventually force-crashes the server to prevent a silent freeze.

---

### `console`

| Option | Default | Description |
|--------|---------|-------------|
| `enable-brigadier-completions` | `true` | Enables advanced command tab-completion in the server console. |
| `enable-brigadier-highlighting` | `true` | Highlights command syntax in the server console. |
| `has-all-permissions` | `false` | If `true`, the console bypasses all permission checks. Leave `false` unless you have a specific reason. |

---

### `item-validation`

| Option | Default | Description |
|--------|---------|-------------|
| `book.author` | `8192` | Max character length for a book's author field. |
| `book.title` | `8192` | Max character length for a book's title. |
| `book.page` | `16384` | Max character length per book page. |
| `book-size.page-max` | `2560` | Max bytes a single page can contribute to the total book size. Set to `disabled` to remove the restriction. |
| `book-size.total-multiplier` | `0.98` | Each page's byte budget is this fraction of the previous page's. Prevents exponential book size growth. |
| `display-name` | `8192` | Max character length for an item's display name. |
| `lore-line` | `8192` | Max character length per lore line. |
| `resolve-selectors-in-books` | `false` | If `true`, `@a`, `@p` etc. are resolved in book text. **Do not enable** — creative mode players can use this to crash the server. |

---

### `messages`

| Option | Default | Description |
|--------|---------|-------------|
| `kick.authentication-servers-down` | `<lang:...>` | Message shown when Mojang auth servers are unreachable. Supports MiniMessage formatting. |
| `kick.connection-throttle` | `Connection throttled!...` | Message shown when a player reconnects too quickly. |
| `kick.flying-player` | `<lang:...>` | Message shown when a player is kicked for flying. |
| `kick.flying-vehicle` | `<lang:...>` | Message shown when a player is kicked for riding a flying vehicle. |
| `no-permission` | `<red>I'm sorry...` | Default no-permission message. Plugins may override this per command. Supports MiniMessage. |
| `use-display-name-in-quit-message` | `false` | Use plugin-set display names instead of real usernames in quit messages. |

---

### `proxies`

| Option | Default | Description |
|--------|---------|-------------|
| `bungee-cord.online-mode` | `true` | Set to match your BungeeCord proxy's `online-mode`. Handles UUID/data when behind a proxy. |
| `proxy-protocol` | `false` | Enable only if using HAProxy or similar. Unrelated to BungeeCord/Velocity. |
| `velocity.enabled` | `false` | Enable Velocity Modern Forwarding. Required if using Velocity as your proxy. |
| `velocity.online-mode` | `true` | Set to match your Velocity proxy's `online-mode`. |
| `velocity.secret` | `""` | Must match the secret in Velocity's `forwarding.secret` file. |

> ⚠️ Misconfiguring proxies can allow players to join with fake UUIDs (bypass authentication). Always match `online-mode` to your proxy setting.

---

### `scoreboards`

| Option | Default | Description |
|--------|---------|-------------|
| `save-empty-scoreboard-teams` | `true` | Automatically removes empty scoreboard teams left behind by plugins. Keeps login times fast. Keep `true`. |
| `track-plugin-scoreboards` | `false` | Track scoreboards with only dummy objectives. Enabling with heavy scoreboard plugins will degrade performance. |

---

### `spark`

| Option | Default | Description |
|--------|---------|-------------|
| `enabled` | `true` | Whether the bundled Spark profiler is active. Useful for diagnosing lag. Keep `true`. |
| `enable-immediately` | `false` | Start Spark profiling during server startup instead of after. Enable when diagnosing startup lag. |

---

### `time`

| Option | Default | Description |
|--------|---------|-------------|
| `affects-all-worlds` | `false` | If `true`, the `/time` command affects all worlds sharing a dimension type. If `false`, only affects the sender's current world. |

---

## 🌍 paper-world.yml

### `anticheat.anti-xray`

| Option | Default | Description |
|--------|---------|-------------|
| `enabled` | `false` | Enables the Anti-Xray system. |
| `engine-mode` | `1` | `1` = Replace ores with fake stone (low CPU). `2` = Randomize hidden blocks (high CPU, more secure). `3` = Per-layer randomization. |
| `max-block-height` | `64` | Y level up to which anti-xray is active. Set to `320` to cover the full world height. |
| `lava-obscures` | `false` | Also hide blocks touching lava. Works poorly with non-default ore textures. |

> **Performance note:** Engine mode `2` significantly increases CPU usage. Use `1` on servers without dedicated anticheat needs.

---

### `chunks`

| Option | Default | Description |
|--------|---------|-------------|
| `auto-save-interval` | `default` | World save interval in ticks. `default` uses `bukkit.yml`. |
| `delay-chunk-unloads-by` | `10s` | How long to keep chunks loaded after players leave the area. Prevents constant load/unload when players move near chunk borders. |
| `max-auto-save-chunks-per-tick` | `24` | Max chunks saved per tick during autosave. Reduce to `8–12` to prevent TPS spikes during saves. |
| `flush-regions-on-save` | `false` | Force-flush region files to disk on every save. Very safe but slow. Only enable if you experience data corruption. |
| `prevent-moving-into-unloaded-chunks` | `false` | Stops players from entering unloaded chunks. Prevents some edge-case crashes. |
| `entity-per-chunk-save-limit` | `-1` | Limits how many of a given entity type are saved per chunk. Useful to prevent chunks becoming corrupted by mob farms gone wrong. |

---

### `collisions`

| Option | Default | Description |
|--------|---------|-------------|
| `max-entity-collisions` | `8` | Server stops processing collisions after this many per entity per tick. Reduce to `2` on mob farm servers to improve performance. |
| `only-players-collide` | `false` | Only run collision checks when a player is involved. Reduces CPU on mob-heavy servers. |
| `allow-player-cramming-damage` | `false` | Players take cramming damage from entity stacking (like in vanilla). |

---

### `entities`

| Option | Default | Description |
|--------|---------|-------------|
| `armor-stands.tick` | `true` | Whether armor stands tick each game tick. Disable for a big performance boost if you have many decorative armor stands. |
| `zombies-target-turtle-eggs` | `true` | Zombies constantly scan for nearby turtle eggs. Set to `false` for a small performance gain. |
| `baby-zombie-movement-modifier` | `0.5` | Speed multiplier for baby zombies. `0.5` = 50% faster than normal. |
| `phantoms-do-not-spawn-on-creative-players` | `true` | Phantoms won't target creative mode players. |
| `nerf-pigmen-from-nether-portals` | `false` | Pigmen spawned from portals have no AI. Useful if portals spawn too many pigmen. |
| `experience-merge-max-value` | `-1` | Sets a cap on XP orb merge value. `-1` = no cap (all merge into one). Set a value like `50` to keep orbs more spread out visually. |

---

### `environment`

| Option | Default | Description |
|--------|---------|-------------|
| `optimize-explosions` | `false` | Caches entity lookups during explosions instead of recalculating. **Enable this** — it significantly speeds up TNT/Creeper explosions. |
| `max-block-ticks` | `65536` | Safety cap on block updates per tick. Prevents server freeze from runaway update chains. |
| `max-fluid-ticks` | `65536` | Same as above, for fluid (water/lava) updates. |
| `disable-ice-and-snow` | `false` | Stops ice and snow formation. Also prevents cauldrons from filling with rain. |
| `nether-ceiling-void-damage-height` | `disabled` | Players above this Y level in the Nether take void damage. Useful for blocking the nether roof without mods. |

---

### `hopper`

| Option | Default | Description |
|--------|---------|-------------|
| `cooldown-when-full` | `true` | Applies a brief cooldown when hopper is full instead of constantly retrying. Keep `true`. |
| `disable-move-event` | `false` | Disables the `InventoryMoveItemEvent` for hoppers entirely. **Massively improves hopper performance** but will break protection plugins (like chest protection). Only enable if you don't use such plugins. |
| `ignore-occluding-blocks` | `false` | Hoppers ignore items inside solid blocks (e.g. hopper minecart in sand). Small performance improvement. |

---

### `misc`

| Option | Default | Description |
|--------|---------|-------------|
| `redstone-implementation` | `VANILLA` | The redstone engine used. `ALTERNATE_CURRENT` or `EIGENCRAFT` dramatically reduce redstone lag. **Recommended for servers with farms.** Note: behavior may differ slightly from vanilla. |
| `update-pathfinding-on-block-update` | `true` | Recalculates mob pathfinding whenever a block changes. Set to `false` on servers with many mobs or active redstone — nearly no gameplay impact. |
| `disable-end-credits` | `false` | Skips the end credits screen when leaving the End dimension. |

---

### `spawning`

| Option | Default | Description |
|--------|---------|-------------|
| `per-player-mob-spawns` | `true` | Mob cap is calculated per player instead of globally. More fair distribution across players. Keep `true`. |
| `alt-item-despawn-rate` | `false` | Allows different despawn timers per item type. Enable to make junk items (cobblestone, dirt, gravel) despawn faster. |
| `count-all-mobs-for-spawning` | `false` | If `true`, spawner mobs count toward the global mob cap. Keep `false` unless you have runaway spawner issues. |

**Example alt despawn config:**
```yaml
spawning:
  alt-item-despawn-rate:
    enabled: true
    items:
      cobblestone: 300
      dirt: 300
      gravel: 300
      netherrack: 300
```

---

### `tick-rates`

| Option | Default | Description |
|--------|---------|-------------|
| `mob-spawner` | `1` | How often spawners attempt to spawn. `1` = every tick. Increase to `2–4` to reduce spawner CPU usage. |
| `grass-spread` | `1` | Delay between grass spreading to adjacent dirt. Increase to `4` for a minor performance gain with no real gameplay impact. |
| `container-update` | `1` | How often inventories are synced to clients. Do **not** increase — causes item ghosting and visual desyncs. |
| `dry-farmland` | `1` | How often dry farmland checks for moisture. `-1` disables. |
| `wet-farmland` | `1` | How often wet farmland checks for moisture. `-1` disables. |

---

### `unsupported-settings`

> ⚠️ These are **not officially supported** by PaperMC. Use at your own risk. They may be removed in future versions.

| Option | Default | Description |
|--------|---------|-------------|
| `disable-world-ticking-when-empty` | `false` | Stops ticking a world entirely when no players are present. Good for multi-world servers with rarely visited worlds. |
| `fix-invulnerable-end-crystal-exploit` | `true` | Prevents players from creating unkillable end crystals. Keep `true`. |

---

### `fixes`

| Option | Default | Description |
|--------|---------|-------------|
| `disable-unloaded-chunk-enderpearl-exploit` | `false` | Prevents ender pearls from saving the thrower when thrown into unloaded chunks. Enable to close this exploit. |
| `falling-block-height-nerf` | `disabled` | Removes falling blocks above this Y level. Useful for preventing sand/gravel cannons. |
| `fix-items-merging-through-walls` | `false` | Prevents items from merging through walls. Small performance cost — only needed if `merge-radius` in `spigot.yml` is large. |
| `prevent-tnt-from-moving-in-water` | `false` | Stops primed TNT from drifting in water currents. |
| `tnt-entity-height-nerf` | `disabled` | Removes primed TNT entities above this Y level. |
| `split-overstacked-loot` | `true` | Splits overstacked items from loot tables. Keep `true` to prevent oversized packets from corrupting chunks. |

---

### `lootables`

| Option | Default | Description |
|--------|---------|-------------|
| `auto-replenish` | `false` | Automatically refills loot containers (chests, barrels) over time. Useful for long-term survival worlds where new chunks aren't being explored. |
| `max-refills` | `-1` | How many times a container can be refilled. `-1` = infinite. |
| `refresh-min` | `12h` | Minimum time before a container can be refilled. |
| `refresh-max` | `2d` | Maximum time before a container is refilled. |
| `reset-seed-on-fill` | `true` | Randomizes loot on each refill instead of repeating the same items. |
| `restrict-player-reloot` | `true` | Prevents the same player from looting the same container again after a refill. |
| `restrict-player-reloot-time` | `disabled` | Per-player cooldown between reloots. |

---

### `maps`

| Option | Default | Description |
|--------|---------|-------------|
| `item-frame-cursor-limit` | `128` | Max map markers (cursors) per map. A very high number can lag clients rendering the map. |
| `item-frame-cursor-update-interval` | `10` | How often (ticks) map cursors update for maps in item frames. Set to `0` or less to disable updates. |

---

### `max-growth-height`

| Option | Default | Description |
|--------|---------|-------------|
| `bamboo.max` | `16` | Max height bamboo grows naturally. |
| `bamboo.min` | `11` | Min height bamboo grows naturally. |
| `cactus` | `3` | Max height cactus grows naturally. |
| `reeds` | `3` | Max height sugar cane grows naturally. |

---

### `spawning.despawn-ranges`

Controls how far from a player mobs are despawned. Uses two thresholds:

| Term | Meaning |
|------|---------|
| `soft` | Beyond this range, mobs have a random chance to despawn each tick |
| `hard` | Beyond this range, mobs are immediately force-despawned |

```yaml
despawn-ranges:
  monster:
    soft:
      horizontal: 32
      vertical: 16
    hard:
      horizontal: 128
      vertical: 64
```

> Reducing `hard` range on mob-heavy servers reduces entity count and improves TPS.

---

### `spawning.despawn-time`

Forces an entity to despawn after a fixed time regardless of proximity to players.

```yaml
despawn-time:
  villager: 1200   # Despawn villagers after 60 seconds if not near a player
```

> Useful for cleaning up stray mobs from farms or events.

---

### `tracking-range-y`

Controls how far **vertically** entities are tracked (sent to players). Disabled by default — all entities use horizontal range only.

| Option | Default | Description |
|--------|---------|-------------|
| `enabled` | `false` | Enable separate vertical tracking ranges. Useful for tall builds or deep underground farms. |
| `monster` | `default` | Vertical tracking range for monsters. |
| `animal` | `default` | Vertical tracking range for animals. |
| `player` | `default` | Vertical tracking range for players. |
| `display` | `default` | Vertical tracking range for display entities. |
| `misc` | `default` | Vertical tracking range for miscellaneous entities. |

---

### `fishing-time-range`

| Option | Default | Description |
|--------|---------|-------------|
| `minimum` | `100` | Minimum RNG ticks before a fish bites. Lower = faster fishing. |
| `maximum` | `600` | Maximum RNG ticks before a fish bites. |

---

### `feature-seeds`

Allows pinning specific world generation features to a fixed seed for reproducibility or farm optimization.

```yaml
feature-seeds:
  generate-random-seeds-for-all: false
  minecraft:ore_diamond: 12345
```

> `generate-random-seeds-for-all: true` auto-fills all features with random seeds — useful for seeing what options are available.

---

## 📌 Quick Reference — Recommended Changes by Server Type

### All Servers
```yaml
# paper-world.yml
environment:
  optimize-explosions: true
misc:
  update-pathfinding-on-block-update: false
entities:
  armor-stands:
    tick: false
  behavior:
    zombies-target-turtle-eggs: false
collisions:
  max-entity-collisions: 2
chunks:
  max-auto-save-chunks-per-tick: 10
```

### Farm / Redstone Servers
```yaml
misc:
  redstone-implementation: ALTERNATE_CURRENT
hopper:
  disable-move-event: true  # Only if no chest-protection plugin
tick-rates:
  mob-spawner: 2
spawning:
  alt-item-despawn-rate:
    enabled: true
    items:
      cobblestone: 300
      dirt: 300
```

### 20+ Player Servers
```yaml
# paper-global.yml
chunk-loading-basic:
  player-max-chunk-load-rate: 50
  player-max-chunk-send-rate: 45
  player-max-chunk-generate-rate: 12
chunks:
  max-auto-save-chunks-per-tick: 10
```

---

## 🔗 Resources

- [PaperMC Official Docs](https://docs.papermc.io/paper/reference/configuration/)
- [Paper Global Config Reference](https://docs.papermc.io/paper/reference/global-configuration/)
- [Paper World Config Reference](https://docs.papermc.io/paper/reference/world-configuration/)
- [Alternate Current (Redstone)](https://github.com/SpaceWalkerRS/alternate-current)
- [Spark Profiler](https://spark.lucko.me/) — identify what's causing lag

---

*Generated with reference to PaperMC documentation. Last updated: June 2026.*
