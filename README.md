<p align="center">
  <h1 align="center">claude-dream</h1>
</p>

<p align="center">
  Memory consolidation for Claude Code — the <code>/dream</code> command that should exist.
</p>

<p align="center">
  <a href="https://github.com/JonEcho/claude-dream/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License" /></a>
  <a href="https://code.claude.com/docs/en/plugins"><img src="https://img.shields.io/badge/Claude_Code-plugin-blueviolet" alt="Claude Code Plugin" /></a>
</p>

---

## Why

Claude Code shows `/dream` in the `/memory` UI, but the command doesn't exist yet ([#39135](https://github.com/anthropics/claude-code/issues/39135), [#38426](https://github.com/anthropics/claude-code/issues/38426), [#38461](https://github.com/anthropics/claude-code/issues/38461)). Auto Dream is in gradual rollout — this plugin gives you the `/dream` command now.

## What It Does

`/dream` audits and consolidates your auto memory files:

1. **Audit** — reads every file in your memory directory, checks frontmatter, flags format violations
2. **Propose** — presents a structured report of what to fix (stale content, duplicates, format drift)
3. **Execute** — after your approval, extracts inline content, deduplicates, renames session-dated files, rebuilds MEMORY.md as a clean index
4. **Verify** — confirms every index entry links to a real file with valid frontmatter

No changes are made without your approval.

## Install

### Option A: Plugin marketplace (recommended)

```
/plugin marketplace add JonEcho/claude-dream
/plugin install dream@claude-dream
```

### Option B: Local plugin directory

```bash
git clone https://github.com/JonEcho/claude-dream.git
claude --plugin-dir ./claude-dream
```

## Usage

```
/dream
```

Or Claude may invoke it automatically when you say "consolidate memory", "clean up memory", or "dream".

## Plugin Structure

```
claude-dream/
├── .claude-plugin/
│   ├── plugin.json          # Plugin manifest
│   └── marketplace.json     # Self-contained marketplace
├── skills/
│   └── dream/
│       └── SKILL.md         # The dream skill
├── README.md
├── LICENSE
└── .gitignore
```

## How It Works

The skill enforces the memory format contract from Claude Code's system prompt:

| Rule | What Dream Checks |
|------|-------------------|
| MEMORY.md is an index | No inline content, tables, or multi-line facts |
| Topic files need frontmatter | `name`, `description`, `type` fields present |
| Valid types only | `user`, `feedback`, `project`, `reference` |
| Semantic naming | Session-dated files flagged for rename |
| Under 200 lines | MEMORY.md line count check |
| No duplicates | Facts appearing in multiple files flagged |
| Stale content | TODOs/Next sections older than 14 days flagged |

## Background

Built from Claude Code's own system prompt instructions for auto memory, which define the format contract, memory types, and organization rules. The skill codifies these into an auditable, repeatable process.

### Sources

- **Claude Code auto memory system prompt** -- the format contract injected into every session, which defines memory types (`user`, `feedback`, `project`, `reference`), the MEMORY.md index structure (one-line entries, 200-line limit), frontmatter requirements, and organization rules. This is not a public docs page -- it's the client system prompt itself.
- [Plugin creation docs](https://code.claude.com/docs/en/plugins) -- plugin structure and marketplace distribution
- [Skills authoring docs](https://code.claude.com/docs/en/skills) -- skill file format and frontmatter

## License

[MIT](LICENSE)
