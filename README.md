# Discourse Character Sheet

A Claude Code plugin that generates RPG character sheets from Discourse forum activity.

Analyzes a user's posts to derive stats, abilities, debuffs, inventory, quests, and party dynamics — then outputs a markdown character sheet and an interactive HTML character page.

## Install

```
claude plugin marketplace add nolosb/discourse-character-sheet
claude plugin install discourse-character-sheet@discourse-character-sheet --scope user
```

## Usage

```
/discourse-character-sheet <username> <discourse-site-url> [timeframe]
```


## What it does

1. **Gathers data** — reads the user's posts via Discourse MCP tools
2. **Analyzes patterns** — output volume, signature moves, strongest contributions, gaps, collaboration style, evolution over time
3. **Maps traits collaboratively** — proposes Family / Trade / Main Trait, stats (STR, DEX, CON, INT, WIS, CHA), abilities, debuffs, inventory, quests, and allies. You pick and adjust.
4. **Generates output** — a markdown character sheet and a self-contained interactive HTML page with tabs for Overview, Abilities, Debuffs, Inventory, Quests, and Party

## Requirements

- [Claude Code](https://claude.ai/claude-code)
- A Discourse MCP server configured for the target site

## Example

![Character sheet screenshot](https://github.com/nolosb/discourse-character-sheet/raw/main/example.png)
