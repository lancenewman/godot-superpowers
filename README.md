# godot-superpowers

A [Superpowers](https://github.com/obra/superpowers) fork tuned for Godot game development.

The upstream plugin is a general-purpose development workflow plugin for Claude Code and other AI coding assistants. This fork keeps everything from upstream and adds two Godot-specific skills:

- **godot-scene-design** — enforces scene-as-prefab architecture, signal topology, and scene split heuristics before any implementation begins
- **gdscript-idioms** — typed GDScript, `@export`/`@onready`, Godot 4 signal patterns, `Resource`-based data

## How It Differs From Upstream

| | [obra/superpowers](https://github.com/obra/superpowers) | godot-superpowers |
|---|---|---|
| Scene architecture guidance | None | `godot-scene-design` discipline skill |
| GDScript conventions | None | `gdscript-idioms` reference skill |
| TDD test runner examples | TypeScript / `npm test` | GUT / gdUnit4 |
| Contributor audience | General OSS contributors | Godot developers |

Everything else — brainstorming, writing-plans, subagent-driven-development, systematic-debugging, TDD, code review — is inherited from upstream unchanged.

## Installation

### Claude Code

```bash
claude plugin add https://github.com/lancenewman/godot-superpowers
```

Or manually add to `~/.claude/settings.json`:

```json
{
  "plugins": [
    "https://github.com/lancenewman/godot-superpowers"
  ]
}
```

### Cursor

Clone the repo and add the path to your Cursor settings as a plugin directory.

### Other Platforms

Follow the standard Superpowers installation instructions for your platform, substituting this fork's repository URL (`https://github.com/lancenewman/godot-superpowers`).

## Skill Workflow for Godot Projects

A full session with godot-superpowers looks like this:

1. **Brainstorm** (`superpowers:brainstorming`) — explore requirements and constraints before touching code
2. **Design scenes** (`superpowers:godot-scene-design`) — decompose the feature into scenes before any implementation
3. **Write tests first** (`superpowers:test-driven-development`) — RED-GREEN-REFACTOR with GUT or gdUnit4
4. **Check idioms** (`superpowers:gdscript-idioms`) — reference GDScript patterns while writing code
5. **Plan** (`superpowers:writing-plans`) — create a task-by-task implementation plan for larger features
6. **Execute** (`superpowers:subagent-driven-development`) — parallel subagent execution with review checkpoints
7. **Review** (`superpowers:requesting-code-review`) — verify before finishing

## Skills

### Godot-Specific

| Skill | When to use |
|-------|-------------|
| `godot-scene-design` | Before designing or implementing any Godot feature; before creating scenes |
| `gdscript-idioms` | When writing or reviewing GDScript |

### General (from upstream)

| Skill | When to use |
|-------|-------------|
| `brainstorming` | Before any feature work — explores requirements before implementation |
| `test-driven-development` | Before writing implementation code — RED-GREEN-REFACTOR |
| `writing-plans` | When you have a spec for a multi-step task |
| `executing-plans` | When executing a written plan inline |
| `subagent-driven-development` | For parallel task execution with review between tasks |
| `systematic-debugging` | When encountering bugs or unexpected behavior |
| `requesting-code-review` | Before completing significant work |
| `receiving-code-review` | When processing review feedback |
| `finishing-a-development-branch` | When implementation is complete and ready to merge |
| `dispatching-parallel-agents` | For independent tasks that can run concurrently |
| `verification-before-completion` | Before claiming work is done — requires evidence |
| `using-git-worktrees` | For feature isolation |

## Updates

To update to the latest version:

```bash
claude plugin update godot-superpowers
```

Or pull the latest from the repository and reinstall.

## Contributing

See [CLAUDE.md](CLAUDE.md) for contributor guidelines.

## License

MIT — see [LICENSE](LICENSE)
