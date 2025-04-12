# 04 – Nebula vs Constellation

Cosmos has two ways to group multiple packages: **Nebulae** and **Constellations**. While they may look similar, they serve *very different* purposes.

---

## 🌫️ What Is a Nebula?

A **Nebula** is a *real* Star package. It doesn’t install anything directly—it just defines a list of dependencies. Think of it as a metapackage that’s actually part of your system.

```toml
name = "system-core"
type = "nebula"
version = "1.0.0"

[dependencies]
musl = ">=1.2.4"
busybox = "^1.36"
zlib = "^1.2"
```

### ✅ Why use a Nebula?
- You want it to show up in `universe.toml`
- You want to version it over time
- You want other Stars to depend on it (e.g. `desktop-core` depends on `system-core`)
- You care that it exists *after* installation

---

## 🌌 What Is a Constellation?

A **Constellation** is a TOML file used to install a bunch of Stars in one shot. It is *not* tracked in your system. It doesn’t show up in `universe.toml`. It doesn’t do anything after the install finishes.

```toml
name = "desktop-tools"
description = "Common desktop applications"

members = [
  "firefox",
  "kde",
  "alacritty@0.12.0"
]
```

### ✅ Why use a Constellation?
- You want a one-time install preset
- You don’t care about uninstalling the group
- You don’t need it versioned or depended on
- You want to script a setup or ship a role profile (e.g. `developer.toml`)

---

## 🧠 Concrete Examples

| Use Case                      | Nebula (`type = "nebula"`) | Constellation |
|------------------------------|-----------------------------|----------------|
| Core system foundation       | ✅ `system-core`            | ❌             |
| Developer tools preset       | ❌                           | ✅ `dev-tools.toml` |
| Desktop environment bundle   | ❌ *(usually)*              | ✅ `desktop-tools.toml` |
| Shared dependency grouping   | ✅                           | ❌             |
| System roles / metapackages  | ✅                           | ❌             |
| Installer presets / profiles | ❌                           | ✅             |

---

## 📌 Version Pinning in Constellations

Constellations let you lock exact versions at install time:
```toml
members = [
  "firefox@124.0",
  "nano@^6.0"
]
```

That makes them ideal for reproducible install flows—even though they’re not tracked.

---

## 🧠 TL;DR

- **Nebula** = installable metapackage; tracked, versioned, and usable as a dependency
- **Constellation** = one-time install preset; not tracked, not versioned, not depended on

Use Nebulae for structure. Use Constellations for convenience.

---

Still not sure? Ask yourself:
> “Do I want this to show up in `universe.toml` or be part of a dependency tree?”

If yes → it’s a Nebula.  
If no → it’s a Constellation.
