---
name: godot-scene-design
description: Use before designing or implementing any Godot feature; before creating new scenes or nodes — enforces scene-as-prefab, signal topology, and composition over inheritance
---

# Godot Scene Design

## Overview

Every scene is a reusable prefab. Every node tree has a single owner. Communication flows through signals.

**Core principle:** A scene that reaches upward or sideways to find other nodes is not a scene — it is a fragment.

## When to Use

**Always:**
- Before creating any new scene or node tree
- Before adding new functionality to an existing scene
- When a script is growing large

**Ask yourself before writing any code:**
1. What is this scene's single responsibility?
2. Can I instance this scene anywhere in the project without assuming what its parent is?
3. Where does data flow — downward to children via method calls, or upward via signals?

## The Iron Law

```
NO SCENE REACHES UPWARD OR SIDEWAYS
```

A scene that calls `get_parent()` to access game state, or uses `get_node("../../Player")` to reach siblings, is architecturally broken. Redesign before adding features.

## Scene-as-Prefab

Every scene must be independently instancable with no assumptions about its parent.

**Valid:** A `Player` scene that emits `health_changed(new_health: int)` — can be instanced anywhere.

**Invalid:** A `Player` scene that calls `get_parent().get_node("HUD").update_health()` — assumes a specific parent structure that may not exist.

## Composition Over Inheritance

Prefer a scene made of specialized child nodes over a single script doing everything.

| Condition | Decompose when... |
|-----------|-------------------|
| A script handles input, movement, AND health | Split into focused child scenes |
| A subtree has its own internal state | Make it its own scene |
| A subtree could be reused elsewhere | It should already be its own scene |
| A subtree has a clearly separate responsibility | Split it |

Large scripts are a scene design smell, not a script quality problem.

## Scene Split Heuristics

Split a node subtree into its own scene when:

- It has its own internal state (a health value, an inventory, a cooldown timer)
- It could be reused in another scene without modification
- It has a clearly separate responsibility from its parent
- The script managing it is growing beyond ~150 lines

**When in doubt, split.** A composed scene with five small child scenes is easier to maintain than a monolithic scene with one large script.

## Signal Topology

```
Parent → Child: call methods directly
Child → Parent: emit signals
Sibling ↔ Sibling: never directly — use parent as mediator, or an autoload event bus
```

**Valid:**
```gdscript
# Parent calling a child method (top-down)
$HealthComponent.set_invincible(true)

# Child signaling upward (bottom-up)
signal health_changed(new_health: int)
```

**Invalid:**
```gdscript
# Sideways reach — architectural smell
get_node("../Enemy").take_damage(10)

# Upward reach — breaks scene independence
get_parent().score += 10
```

## Ownership Rules

- A scene owns its **children** — it may call their methods and read their state
- A scene does **not own its parent** — never call `get_parent().some_method()`
- A scene does **not own its siblings** — never use `../SiblingNode` paths
- Cross-scene communication goes through **signals** or an **autoload event bus**

## Autoload Event Bus

When two scenes that don't share a direct parent-child relationship need to communicate, use an autoload singleton as an event bus:

```gdscript
# Events.gd (autoload singleton)
extends Node

signal enemy_died(enemy: Node)
signal player_health_changed(new_health: int)
```

```gdscript
# In Enemy.gd — emit the event
func die() -> void:
    Events.enemy_died.emit(self)
    queue_free()
```

```gdscript
# In HUD.gd — listen for the event
func _ready() -> void:
    Events.player_health_changed.connect(_on_player_health_changed)

func _on_player_health_changed(new_health: int) -> void:
    health_bar.value = new_health
```

**Autoload is appropriate for:** global game state, event bus, settings, scene transitions.

**Autoload is not appropriate for:** scene-specific logic, anything that should reset when a scene unloads.

## Red Flags — Stop and Redesign

- `get_parent()` in any signal handler or game logic method
- `get_node("../../SomeThing")` reaching beyond the current scene root
- `get_tree().get_nodes_in_group("enemies")` used as a substitute for proper scene composition
- A script file over 200 lines with multiple unrelated responsibilities
- "I need to access the HUD from the Player script" (use signals instead)
- "I need to access the Player from the Enemy script" (use signals or an event bus)

**All of these mean: redesign before implementing.**

## Self-Check Before Building

Before writing any implementation code for a Godot feature, confirm:

- [ ] This scene has one clear responsibility
- [ ] This scene can be instanced without knowing what its parent is
- [ ] Data flows downward (method calls) and upward (signals) — not sideways
- [ ] No script reaches outside its own scene root
- [ ] No script is growing large enough to warrant splitting
