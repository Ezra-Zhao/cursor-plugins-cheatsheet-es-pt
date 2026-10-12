# 🔌 Cursor Plugins — Quick Guide (English)

> Verified against the [cursor/plugins](https://github.com/cursor/plugins) README and the official manifest schema (2026-10-12).
> What it is: a plugin packs skills, rules, commands, agents, hooks and MCP servers into one installable unit for Cursor, declared by a single JSON manifest.

---

## 1. The building blocks

A plugin is a folder. Each block lives in its own place:

| Block | Location | Format |
|---|---|---|
| Skills | `skills/<name>/SKILL.md` | Markdown with `name` + `description` frontmatter |
| Rules | `rules/*.mdc` | `.mdc` files, frontmatter: `description`, `alwaysApply`, optional `globs` |
| Commands | `commands/*.md` or `*.txt` | Slash commands, `name` + `description` frontmatter |
| Agents | `agents/*.md` | Subagent definitions, `name` + `description` frontmatter |
| Hooks | `hooks/hooks.json` | Lifecycle hook configuration |
| MCP servers | `mcp.json` at plugin root | MCP server definitions |

Cursor discovers components by folder. You only declare paths in the manifest when you move something away from its default spot.

---

## 2. The manifest: `.cursor-plugin/plugin.json`

One file at the plugin root: `.cursor-plugin/plugin.json`. The official schema requires **exactly one field**: `name`.

Minimal valid manifest:

```json
{
  "name": "my-plugin"
}
```

- `name`: lowercase kebab-case — letters, digits, hyphens (periods allowed), must start and end with an alphanumeric character.
- Recommended fields: `version` (semver), `description`, `author` (`{ "name": ..., "email": ... }`), `license`, `keywords`.
- Optional: `displayName`, `homepage`, `repository`, `logo`, `category`, `tags`, `minClientVersions`.
- Component fields (`skills`, `rules`, `agents`, `commands`, `hooks`, `mcpServers`): path or glob, only needed for non-default locations.
- Every path in the manifest must be **relative** — no absolute paths, no `..`.

A realistic manifest (modeled on the official `teaching` plugin):

```json
{
  "name": "teaching",
  "displayName": "Teaching",
  "version": "1.0.0",
  "description": "Skill mapping, practice plans, and learning retrospectives.",
  "author": { "name": "Cursor", "email": "plugins@cursor.com" },
  "license": "MIT",
  "keywords": ["teaching", "study-plan", "practice"],
  "category": "utilities",
  "skills": "./skills/"
}
```

---

## 3. Your first plugin in 3 steps

**1. Write the manifest.** Create the folder, drop `.cursor-plugin/plugin.json` inside, add a `README.md` and a `LICENSE`, then add only the component folders you actually use.

**2. Load it locally.** Put the folder in your local plugin directory — no install step, Cursor picks it up directly:

```
~/.cursor/plugins/local/<plugin-name>/
```

**3. Debug and iterate.** Before calling it done, check: the JSON parses, `name` is valid kebab-case, every declared path exists, and each skill/rule/agent/command carries its `name` + `description` frontmatter. Shortcut: the official `create-plugin` plugin scaffolds the folder and runs these quality checks for you.

Selling it later? A multi-plugin repo adds one entry per plugin (`name` + `source`) to a root `.cursor-plugin/marketplace.json`.

---

## 4. Official plugins worth studying

From the upstream repo root (author: Cursor, unless noted):

| Plugin | What it does |
|---|---|
| `create-plugin` | Scaffolds and validates new plugins — start here |
| `cursor-sdk` | Build apps and automations with the TypeScript SDK |
| `advisor` | Consults a stronger model before big decisions |
| `orchestrate` | Fans large tasks out across parallel cloud agents |
| `thermos` | Deep branch review: security and correctness audits |
| `ralph-loop` | Iterative self-referential AI loops |
| `agent-compatibility` | Scans a repo for agent compatibility, audits startup and docs |
| `teaching` | Skill mapping, practice plans, learning retrospectives |

Dozens more integrations live under `third_party/` — GitHub, Gmail, Google Drive, Playwright, Salesforce, HubSpot, Zoom, X, and many others. Read one that resembles yours before writing your own.

---

## Links

- Upstream repo: https://github.com/cursor/plugins
- Manifest schema: https://github.com/cursor/plugins/blob/main/schemas/plugin.schema.json
- Marketplace schema: https://github.com/cursor/plugins/blob/main/schemas/marketplace.schema.json

---

*Found an error? Open an issue — spec details change as Cursor ships.*
