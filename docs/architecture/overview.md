# 🧩 Godot Modular Game Template – Architecture Overview

## 🎯 Purpose

A **data-driven, modular architecture** for Godot that separates **logic**, **assets**, and **content** to maximize reusability, mod support, and maintainability across projects.

---

## 🏗️ High-Level Concept

**Strict separation of concerns** across four main domains, plus an external mod layer:

| Directory      | Role                                
| -------------- | ------------------------------------
| **`core/`**    | Game logic, systems, and definitions
| **`shared/`**  | Reusable assets, prefabs, and data  
| **`content/`** | Game-specific implementations       
| **`docs/`**    | Documentation and guides            
| **`mods/`**    | User-space overrides and extensions 

---

## 📁 Folder Structure

### **`core/` – Rules & Systems**

Defines what’s _possible_ in the game.
Contains all reusable systems, components, and schemas.

**Subfolders:**

- `autoloads/` – Global manager singletons (`AssetLoader`, `SaveManager`, etc.)
- `components/` – ECS-style reusable logic blocks
- `systems/` – Core game systems (Input, Localization, etc.)
- `resources/` – Data schemas (`CharacterStats`, `ItemData`, etc.)
- `scripts/` – Utility and helper classes

---

### **`shared/` – Library**

Holds reusable **assets**, **prefabs**, and **data**.

**Subfolders:**

- `2d-assets/`, `3d-assets/`, `audio/`, `fonts/`, `shaders/`
- `prefabs/` – Prebuilt UI and entity templates
- `data/` – JSON or CSV files for values (character stats, items, etc.)

---

### **`content/` – Game-Specific**

Implements this particular game’s characters, levels, and UI.

**Example:**

```
content/characters/player/
├── player.tscn
├── player.gd
├── sprites/
└── audio/
```

---

### **`docs/` – Documentation**

Contains all technical and creative documentation.

**Recommended subfolders:**

- `architecture/`
- `modding/`
- `content-authoring/`
- `assets/diagrams/`

---

### **`mods/` – External Override Layer**

Located in `user://mods/` (outside the main `.pck`).
Mods mirror the game's file structure to override or extend content.

Each mod includes a manifest:

```json
{
  "id": "author.modname",
  "version": "1.0.0",
  "override_priority": 100,
  "dependencies": ["other.mod"]
}
```

---

## ⚙️ Core Systems

### **Asset Loading**

All resource requests pass through `AssetLoader`, which searches:

1. `user://mods/` (highest priority)
2. `res://` (base game fallback)

→ Never use `load()` directly.

### **Mod Manager**

`ModManager` scans `user://mods/` and sorts mods by `override_priority`.

### **Component System**

Composition over inheritance.
Entities (players, enemies, items) combine reusable components.

### **Data-Driven Design**

Schema in `core/resources/`, values in `shared/data/`.
Modders can rebalance the game by editing JSON.

### **Event Bus Decoupling**

Managers communicate through signals, not direct calls.
Prevents hard dependencies and enables mod hooks.

---

## 🧩 Design Principles

1. **Separation of Concerns** – Logic ≠ Assets ≠ Content
2. **Single Entry Points** – Centralized management (`AssetLoader`, `SceneManager`, etc.)
3. **Dependency Injection** – Use exports, not hardcoded paths
4. **Configuration Over Code** – JSON or exported vars define behavior
5. **Composition Over Inheritance** – Reuse generic components across entities

---

## 🔄 Typical Workflow

### Adding a New Character

1. Create folder in `content/characters/`
2. Add `.tscn` and `.gd` files
3. Compose components from `core/components/`
4. Add stats JSON to `shared/data/characters/`
5. Load data through `AssetLoader`

### Creating a Reusable UI Element

1. Add behavior in `core/components/ui/`
2. Add visuals to `shared/2d-assets/ui/`
3. Build prefab in `shared/prefabs/ui/`
4. Use prefab in `content/ui/`

---

## 🚫 Rules of the Road

**Never:**

- Use `load()` directly (always use `AssetLoader`)
- Hardcode manager paths
- Place pure assets in `core/`
- Mix game-specific content in `shared/`

**Always:**

- Communicate via event buses
- Use `@export` for configuration
- Keep data definitions (`core/resources/`) separate from values (`shared/data/`)

---

## 🧠 Mental Model

```
core/     → defines the rules
shared/   → provides reusable tools and assets
content/  → builds the actual game
mods/     → overrides or extends the game
```

Everything flows **left → right** (rules → library → implementation → overrides).

---

## 🏁 Outcome

A **highly modular, moddable, and reusable Godot foundation**:

- Core logic portable across genres
- Mods override assets safely
- No code edits needed for balance tweaks
- Components and systems decoupled by design

---

## AI Notice

If you're reading this, I didn't rewrite this overview myself yet, which is why it is the way that it is. The above content is subject to scrutiny and must be tested with real projects and refined before it should be considered ***Not Slop™️***.