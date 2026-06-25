# Living Villages

> AI-enhanced villagers that think, remember, build, and govern themselves — for Minecraft 26.2 (Fabric).

Living Villages turns Minecraft's static villager hubs into communities that hold real conversations, remember what you've done together, manage their own resources, raise houses and streets, fortify against raids, form trade routes with other villages, and pass family lineages down through generations.

The mod's design rule is **"villagers think; Minecraft acts"** — the AI picks what to do and why, the mod handles the pathfinding, block placement, and inventory work.


(Being the first iteration of this mod, there is a rather unpolished feel to it. I will work to develop a user interface, maybe a city hall with books that log each historic village event. Possibly a store center.)
---

## Features

| Category | What it does |
|---|---|
| 💬 Natural conversation | `/talk <message>` talks to the nearest villager via a local or cloud AI |
| 🧠 Persistent memory | Villagers remember durable facts ("call me Will") and their last conversation |
| 🧩 Dynamic personalities | UUID-seeded archetype + traits + name (no two villagers feel the same) |
| 🏘️ Villages as communities | Proximity-clustered villages with shared chronicles, leaders, reputation, raid history |
| 🌾 Specialized cultures | Geography-derived: **Agricultural, Forestry, Mining, Martial** — each over-produces its specialty |
| 👁️ World awareness | Villagers perceive time, weather, biome, terrain, and threats (with coordinates) |
| 🛡️ Adaptive defense | Open-air mob detection; raid alarms; **Guards** (and martial villages) actively fight, **including creepers**; civilians muster |
| ⛏️ Autonomous work | Villagers gather wood, mine stone & ore, harvest & replant crops — all on their own when reserves run low |
| 🏗️ Construction | Warehouses, **houses** (with functional doors + beds, oriented toward streets), **streets**, and a **team-built defensive wall** with watchtowers and gates |
| ⛰️ Mining | Persistent staircase mine that deepens each run, branches around obstacles, lights itself, and reaches the diamond band |
| 🛒 Regional trade | Caravans haul surpluses from specialty villages to villages in need; trades warm relations |
| 🤝 Diplomacy | Villages develop alliances (from trade) and rivalries (from shared scarcity); rivals refuse to trade |
| 🗺️ Exploration parties | Scouts venture out, discover terrain and other villages, and report back |
| 🧬 Family lineages | Every villager has a parent (or founds a line); when they die, descendants inherit and remember their deeds |
| 📋 Reports | `/village` prints a full status: name, culture, population, leader, your standing, resources, threats, upgrades, relations, recent events |

---

## Requirements

| | Minimum |
|---|---|
| Minecraft | **26.2** |
| Mod loader | **Fabric Loader ≥ 0.19.3** |
| Mod API | **Fabric API** (latest for 26.2) |
| Java | **25 or newer** (project uses Java 26 features) |
| AI backend (one of) | **Ollama** (local, free, recommended), Anthropic API key, or any OpenAI-compatible endpoint |

The mod itself is small (~150 KB JAR). The AI backend is what does the heavy lifting.

---

## Installation

### Singleplayer / Client

1. Install [Fabric Loader 0.19.3+](https://fabricmc.net/use/installer/) for Minecraft 26.2.
2. Download the latest **Fabric API** for 26.2 and drop it into `.minecraft/mods/`.
3. Drop **`livingvillages-1.0.0.jar`** into `.minecraft/mods/`.
5. Pick an AI backend (see below) and launch the game once to generate the config file.
6. Edit the config, then launch again.

### Dedicated server

Same as above, but use the Fabric server installer and drop both JARs into the server's `mods/` folder. The mod is server-side authoritative — clients only need Fabric API.

---

## AI backend

The first launch creates `config/livingvillages.json`. Pick one of:

### Ollama (recommended — local, free, private)

1. Install [Ollama](https://ollama.com).
2. Pull a model: `ollama pull llama3.1:8b` (recommended) or `ollama pull llama3.2:3b` (lighter).
3. Confirm `ollama serve` is running on `http://localhost:11434`.
4. Edit `config/livingvillages.json`:

LLama3.1:8b is approximately 8gb of data. It is strongly recommended for less dropped commands. 

```json
{
  "backend": "OLLAMA",
  "model": "llama3.1:8b",
  "baseUrl": "http://localhost:11434",
  "villagerCooldownSeconds": 5,
  "maxConcurrentRequests": 2,
  "timeoutSeconds": 30
}
```

### Anthropic (cloud, paid)

```json
{
  "backend": "ANTHROPIC",
  "model": "claude-haiku-4-5-20251001",
  "apiKey": "sk-ant-...",
  "timeoutSeconds": 30
}
```

### OpenAI-compatible

```json
{
  "backend": "OPENAI_COMPAT",
  "model": "gpt-4o-mini",
  "apiKey": "sk-...",
  "baseUrl": "https://api.openai.com",
  "timeoutSeconds": 30
}
```

---

## In-game commands

- `/talk <message>` — speak to the nearest villager (range ≈ 40 blocks horizontally)
- `/village` — print the status report of the nearest village

Most behavior is **autonomous** — you don't have to give commands. Stand near a village and let it run; you'll see villagers gathering, building houses, paving streets, defending against raids, and trading with neighbors on their own. Use `/talk` for conversation, role-play, and to issue specific requests; use `/village` to see what's happening.

---

## Building from source

```bash
./gradlew build
```

Produces `build/libs/livingvillages-<version>.jar`.

To run a dev client:

```bash
./gradlew runClient
```

---

## License

[CC0 1.0](LICENSE) — public domain dedication. Use it however you like. Attribution is appreciated but not required.

---

## Project structure

```
src/main/java/com/livingvillages/
├── ai/                # LLM client + JSON-tolerant response parsing
├── config/            # Config file (livingvillages.json)
├── entity/            # Villager name tags
├── interfaces/        # MemoryHolder marker interface
├── job/               # Behavior jobs (gather, mine, build, wall, guard, caravan, …)
├── lineage/           # Family lineage tracking
├── memory/            # Per-villager memory
├── mixin/             # The brain-suppression + lineage mixins
├── network/           # /talk command and prompt assembly
├── personality/       # UUID-seeded personalities
├── village/           # Village abstraction, culture, defense, trade, diplomacy
└── world/             # World perception (sky, terrain, threats)
```

---

## Acknowledgements

Built on [Fabric](https://fabricmc.net) and [Fabric API](https://github.com/FabricMC/fabric).
AI inference via [Ollama](https://ollama.com), the [Anthropic API](https://docs.anthropic.com), or any OpenAI-compatible endpoint.
