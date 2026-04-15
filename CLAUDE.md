# godot-superpowers — Contributor Guidelines

## Project Context

This is a fork of [obra/superpowers](https://github.com/obra/superpowers) focused on Godot game development. The fork adds Godot-specific skills while keeping all upstream workflow infrastructure.

Core additions in this fork:
- `skills/godot-scene-design/` — scene architecture discipline skill
- `skills/gdscript-idioms/` — GDScript conventions reference skill
- TDD skill updated with GUT/gdUnit4 runner command examples

## Before Contributing

1. **Read the PR template** at `.github/PULL_REQUEST_TEMPLATE.md` and fill every section with specific answers — not summaries or placeholders.
2. **Search for existing PRs** (open and closed) for duplicates before opening a new one.
3. **Verify this is a real problem** you experienced in a Godot session. Speculative improvements will be rejected.
4. **Confirm the change belongs here** — Godot-specific skills belong in this fork; truly general skills belong in [upstream superpowers](https://github.com/obra/superpowers); skills for specific Godot addons or frameworks belong in separate plugins.
5. **Get human review** on the complete diff before submitting.

## What We Will Not Accept

- Changes that break or alter upstream workflow skills (brainstorming, planning, TDD, debugging, code review) without extensive evaluation evidence
- Skills for specific Godot addons, frameworks, or third-party tools — publish those as separate plugins
- Fork-specific customizations that don't benefit all Godot developers regardless of their project
- Unrelated changes bundled together — one problem per PR
- Fabricated problem statements or speculative fixes — every PR must solve a real problem someone actually experienced

## Godot-Specific Standards

When contributing Godot-specific skills or documentation:

- All GDScript examples must be typed (type annotations on variables, function parameters, and return types)
- Signal examples must use Godot 4 syntax (`signal_name.connect(handler)`, typed signal declarations)
- Scene examples must follow the scene-as-prefab principle (no `get_parent()`, no sideways node paths)
- Test runner commands must use GUT or gdUnit4, not npm or TypeScript

## Skill Changes Require Evaluation

Skills are not prose — they shape agent behavior. If you modify skill content:

- Use `superpowers:writing-skills` to develop and test changes
- Run adversarial pressure testing across multiple sessions
- Show before/after eval results in your PR
- Do not modify Red Flags tables, iron laws, or rationalization lists without evidence the change improves outcomes

## General

- One problem per PR
- Test on at least one harness and report results in the environment table
- Describe the problem you solved, not just what you changed
