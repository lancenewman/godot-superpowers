# godot-superpowers Release Notes

## v5.0.7 (2026-04-15)

### Initial Fork Release

godot-superpowers is a fork of [obra/superpowers v5.0.7](https://github.com/obra/superpowers).

**Added:**
- `skills/godot-scene-design/` — discipline skill for Godot scene architecture: scene-as-prefab mental model, composition over inheritance, signal topology, ownership rules, and self-check before building
- `skills/gdscript-idioms/` — reference skill for GDScript conventions: typed annotations, `@export`/`@onready`, Godot 4 signal patterns, `Resource`-based data, GDScript 2.0 specifics (`super()`, lambdas, `await`, `%NodeName`)

**Updated:**
- `skills/test-driven-development/` — replaced `npm test path/to/test.test.ts` command examples with GUT/gdUnit4 equivalents; all TDD principles and structure intact
- `.claude-plugin/plugin.json` and `.cursor-plugin/plugin.json` — rebranded to reflect Godot fork (name, author, description, URLs, keywords)
- `README.md` — rewritten for Godot development context; covers fork differences, skill workflow for Godot, and the new Godot-specific skills
- `CLAUDE.md` — updated contributor guidelines for Godot fork; removed obra/superpowers-specific PR rejection warnings; added Godot coding standards

**Removed:**
- Upstream obra/superpowers design history (`docs/plans/`, `docs/superpowers/plans/`, upstream spec files in `docs/superpowers/specs/`)
- `scripts/sync-to-codex-plugin.sh` — upstream-specific Codex publishing tooling
