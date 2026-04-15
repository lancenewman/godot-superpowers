---
name: gdscript-idioms
description: Use when writing or reviewing GDScript — typed annotations, @export/@onready, Godot 4 signal patterns, Resource-based data, GDScript 2.0 specifics
---

# GDScript Idioms

## Overview

Idiomatic GDScript is typed, signal-driven, and uses Godot 4's built-in patterns. Code that works but ignores these patterns loses editor autocompletion, makes Inspector integration harder, and is more fragile to refactor.

## When to Use

Always when writing or reviewing GDScript. Reference the relevant section before writing signals, exports, or data structures.

---

## @export and @onready

**Use `@onready` instead of `get_node()` in `_ready()`:**

```gdscript
# Bad
var health_bar: ProgressBar

func _ready() -> void:
    health_bar = get_node("HealthBar")

# Good
@onready var health_bar: ProgressBar = $HealthBar
```

**Use `@export` for designer-tunable values instead of hardcoded constants:**

```gdscript
# Bad — hardcoded, can't tune in Inspector
const MAX_SPEED: float = 200.0

# Good — designer can tune per-scene in the Inspector
@export var max_speed: float = 200.0
@export var jump_height: float = 400.0
@export var enemy_scene: PackedScene
```

---

## Typed GDScript Throughout

Always use type annotations. Untyped code generates warnings, gets no editor autocompletion, and breaks static analysis.

```gdscript
# Bad
var speed = 200
func move(direction):
    position += direction * speed

# Good
var speed: float = 200.0

func move(direction: Vector2) -> void:
    position += direction * speed
```

**Typed arrays and return types:**

```gdscript
# Typed array
var inventory: Array[Item] = []

# Function with typed parameters and return type
func get_nearest(enemies: Array[Node2D], from: Vector2) -> Node2D:
    var nearest: Node2D = enemies[0]
    for enemy: Node2D in enemies:
        if enemy.global_position.distance_to(from) < nearest.global_position.distance_to(from):
            nearest = enemy
    return nearest
```

**Typed class members:**

```gdscript
class_name Player extends CharacterBody2D

@export var max_health: int = 100
var current_health: int = max_health
var is_invincible: bool = false
```

---

## Signal Patterns

**Declare signals with types:**

```gdscript
# Bad — no type information
signal health_changed

# Good — typed, self-documenting
signal health_changed(new_health: int)
signal item_collected(item: ItemData, collector: Node)
```

**Connect with Godot 4 syntax:**

```gdscript
# Bad — Godot 3 style
connect("health_changed", self, "_on_health_changed")

# Good — Godot 4 style
func _ready() -> void:
    health_changed.connect(_on_health_changed)
```

**Emit with named parameters:**

```gdscript
# Emit — pass the meaningful value
health_changed.emit(current_health)
```

**Handler naming convention:**

```gdscript
# From a child node signal: _on_ChildName_signal_name
func _on_health_component_health_changed(new_health: int) -> void:
    health_bar.value = new_health
```

---

## Resource-Based Data

Use custom `Resource` subclasses instead of raw `Dictionary` for structured data.

```gdscript
# Bad — Dictionary: no type safety, no Inspector support, no reuse
var sword: Dictionary = {
    "name": "Iron Sword",
    "damage": 15,
    "icon": preload("res://assets/sword.png")
}

# Good — custom Resource: typed, Inspector-editable, version-controllable
```

```gdscript
# item_data.gd
class_name ItemData extends Resource

@export var item_name: String = ""
@export var damage: int = 0
@export var icon: Texture2D
```

```gdscript
# Using it
@export var item: ItemData

func apply_item(target: Node) -> void:
    target.take_damage(item.damage)
```

Resources can be saved as `.tres` files in the project, edited in the Inspector, and shared across multiple scenes without duplication.

---

## Autoload Singletons

**Appropriate for:** global game state, event bus, settings, scene transitions.

**Not appropriate for:** scene-specific logic, anything that should reset when a scene unloads.

```gdscript
# game_state.gd (add to Project > Project Settings > Autoload as "GameState")
extends Node

var score: int = 0
signal score_changed(new_score: int)

func add_score(points: int) -> void:
    score += points
    score_changed.emit(score)
```

---

## Godot 4 / GDScript 2.0 Specifics

**`super()` to call the parent method:**

```gdscript
func _ready() -> void:
    super()  # calls parent class _ready()
    setup_player()
```

**Lambdas:**

```gdscript
var double: Callable = func(x: int) -> int: return x * 2
var result: int = double.call(5)  # 10
```

**`await` for async operations and signals:**

```gdscript
# Wait for a one-shot timer
await get_tree().create_timer(1.0).timeout

# Wait for an animation to finish
await animation_player.animation_finished

# Wait for a signal with a value
var new_health: int = await health_component.health_changed
```

**`%NodeName` unique node shorthand (set the node's name as unique in the Scene dock):**

```gdscript
# Bad — fragile long path that breaks if hierarchy changes
@onready var health_bar = get_node("UI/HUD/StatusPanel/HealthBar")

# Good — percent sign prefix means "find this unique-named node anywhere in the scene"
@onready var health_bar: ProgressBar = %HealthBar
```

---

## Quick Reference

| Pattern | Prefer |
|---------|--------|
| Node refs in `_ready()` | `@onready var node: Type = $NodePath` |
| Designer-tunable values | `@export var value: Type = default` |
| All variable declarations | Always typed: `var x: float = 0.0` |
| Signal declarations | Typed params: `signal thing_happened(param: Type)` |
| Signal connections | `signal_name.connect(handler)` |
| Structured data | Custom `Resource` subclass, not raw `Dictionary` |
| Deep node access | `%UniqueName` instead of long `get_node()` path |
| Async/signal wait | `await signal_or_timer` |
| Parent method call | `super()` |
