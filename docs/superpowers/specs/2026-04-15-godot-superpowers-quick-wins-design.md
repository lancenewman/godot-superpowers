# godot-superpowers: Quick Wins Design

**Date:** 2026-04-15
**Status:** Approved

## Problem

This is a fork of obra/superpowers aimed at Godot game development. The upstream plugin is oriented toward general software development (TypeScript/JavaScript examples, `npm test`, no game-engine-specific design guidance). Two gaps stand out:

1. **Scene/node architecture** — Claude lacks a mental model of idiomatic Godot design. It tends to propose monolithic scenes or flat scene structures instead of composed, instanced sub-scenes.
2. **GDScript conventions** — Claude writes code that works but doesn't feel Godot-native: missing typed annotations, wrong signal patterns, raw Dictionaries instead of Resources.

The goal is a reusable plugin usable across multiple Godot projects, with the design conversation and code output both feeling Godot-native.

## Scope

Quick wins to establish the fork's identity and address the primary pain points. TDD is out of scope (user already handles this by pointing Claude to GUT documentation). Deep platform-specific cleanup (Codex, OpenCode, Cursor) is out of scope for this pass.

---

## New Skills

### `skills/godot-scene-design/SKILL.md`

**Type:** Discipline skill (like TDD — enforces a way of thinking before action)

**Trigger:** Before designing or implementing any Godot feature; before creating new scenes or nodes

**Core content:**

- **Scene-as-prefab mental model** — every scene must be independently instancable with no assumptions about its parent. A scene that reaches upward to find its parent breaks this.
- **Composition over inheritance** — prefer a scene made of specialized child nodes over a single script doing everything. If a script is growing large, it's a signal the scene should be split.
- **Scene split heuristics:**
  - Split when a node subtree has its own internal state
  - Split when a subtree could be reused elsewhere
  - Split when a subtree has a clearly separate responsibility
- **Signal topology** — scenes expose signals upward, call methods on children downward, never reach sideways via `get_node("../../SomeOtherThing")` or `get_tree().get_nodes_in_group()` as a substitute for proper architecture
- **Ownership rules** — a scene owns its children; it does not own its parent or siblings. Cross-scene communication goes through signals or an autoload event bus, not direct node paths.

### `skills/gdscript-idioms/SKILL.md`

**Type:** Reference skill

**Trigger:** When writing or reviewing GDScript

**Core content:**

- **`@export` and `@onready`** — use `@onready` instead of assigning node references in `_ready()` via `get_node()`; use `@export` for designer-tunable values instead of hardcoded constants
- **Typed GDScript throughout** — `var speed: float`, typed function parameters and return types, typed arrays (`Array[Node]`), typed dictionaries where applicable
- **Signal patterns:**
  - Declare with types: `signal health_changed(new_health: int)`
  - Connect with `+=` syntax: `health_changed.connect(_on_health_changed)` or `health_changed += _on_health_changed`
  - Emit with named parameters for clarity
- **Autoload singletons** — appropriate for: global game state, event bus, settings. Not appropriate for: scene-specific logic, anything that should be reset per-scene
- **Resource-based data** — define custom `Resource` subclasses instead of raw `Dictionary` for structured data (item definitions, level configs, character stats)
- **Godot 4 / GDScript 2.0 specifics:**
  - `super()` for calling parent methods
  - Lambdas: `var fn = func(x): return x * 2`
  - `await` for async operations and signals: `await get_tree().create_timer(1.0).timeout`
  - `%NodeName` shorthand for unique-named nodes instead of long `get_node()` paths

---

## Modifications to Existing Files

### `plugin.json`
Rebrand: update `name`, `description`, `keywords`, `author`, `homepage`, `repository` to reflect this fork and its Godot focus.

### `README.md`
Full rewrite. Cover: what the fork is, how it differs from upstream, installation, the skill workflow as it applies to Godot projects, and the new Godot-specific skills.

### `CLAUDE.md`
Strip the upstream contributor guidance (PR rejection rates, slop warnings — these are obra/superpowers-specific). Replace with:
- Godot project context for contributors
- This fork's contributor norms
- Any Godot-specific coding standards to enforce

### `skills/test-driven-development/SKILL.md`
Targeted change only: replace `npm test path/to/test.test.ts` command examples with GUT/gdUnit4 runner equivalents. Leave all principles and structure intact.

### `RELEASE-NOTES.md`
Clear upstream entries. Start fresh with this fork's release history.

### `.github/PULL_REQUEST_TEMPLATE.md` and `.github/ISSUE_TEMPLATE/`
Rewrite for Godot context. Remove obra/superpowers-specific language. Keep the structure (it's good) but update the content to reflect this project.

---

## Removals

| Path | Reason |
|------|--------|
| `docs/plans/` | obra's upstream design history; not relevant to this fork |
| `docs/superpowers/plans/` | Same |
| `docs/superpowers/specs/2026-*.md`, `docs/superpowers/specs/2025-*.md` (upstream entries only) | obra's upstream design history; delete the upstream spec files but keep the directory — this fork's own specs live here |
| `scripts/sync-to-codex-plugin.sh` | Upstream-specific Codex publishing tooling; not useful until/unless this fork pursues Codex support |

**Keep:**
- `scripts/bump-version.sh` + `.version-bump.json` — reusable release workflow
- `CODE_OF_CONDUCT.md` — keep as-is or lightly adapt
- `.github/FUNDING.yml` — keep (owner's decision whether to use it)
- All platform configs (`.codex/`, `.cursor-plugin/`, `GEMINI.md`) — keep unless explicitly decided otherwise

---

## Success Criteria

- A new Godot project session: Claude proposes a scene breakdown before writing code
- GDScript output uses typed annotations, proper signal syntax, and `@onready`/`@export` without being prompted
- The fork is clearly identifiable as Godot-focused from `plugin.json`, `README.md`, and `CLAUDE.md`
- Release workflow and open source infrastructure remain intact and functional
