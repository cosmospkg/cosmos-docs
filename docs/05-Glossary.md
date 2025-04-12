# 05 – Glossary

This document defines core terms used throughout Cosmos. It provides short, consistent definitions for quick reference or onboarding.

---

## ✨ Star
A single package. Contains metadata (`star.toml`), an optional source tarball, and optional install script or Nova logic.

## ☁️ Nebula
A special Star with `type = "nebula"`. It has no source or install script, only dependencies. Used to group other Stars under one installable unit.

## 🌌 Galaxy
A package repository. A folder containing Star definitions, source tarballs, and a `meta.toml` with metadata and a list of available Stars. Can be hosted over HTTP, Git, USB, or locally.

## 🌍 Constellation
A named group of Stars for preset installs. Defined in `constellation.toml`. Not tracked in the Universe, used only at install time.

## 🚀 Universe
The installed package state of the system. Tracked in `universe.toml`. Records Star name, version, and installed file paths.

## ✨ Stellar
The builder tool used by maintainers to scaffold, validate, and package Stars. Not required for system usage.

## 🔮 Nova
A Lua-based scripting engine embedded into Cosmos for safe, portable install/build scripts. Replaces shell scripts. Sandbox-enforced.

Nova scripts are executed from a temporary directory and receive the `install_root` variable injected automatically.  
They can use limited built-in helpers such as `run`, `copy`, `mkdir`, and `symlink` for common operations.

## 🌐 Galaxy Cache
Local directory of synced Galaxies used for install/update/search. Read by Cosmos CLI.

## 📁 Config
Main configuration file: `/etc/cosmos/config.toml`. Defines cache directory, install root, and Galaxy sources.

## 🫠 Real Root
The actual installation target directory. Passed into Nova and used as the base for all file operations.

## ☄️ Tarball
A `.tar.gz` archive containing the package's installable files. Built by Stellar. Referenced by `source` field in `star.toml`.

---

This glossary ensures all Cosmos tools, docs, and discussions refer to the same concepts consistently and clearly.