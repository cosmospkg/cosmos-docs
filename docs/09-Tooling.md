# 09 – Tooling (Stellar)

This document defines the maintainer-side tooling that powers Cosmos package creation and validation. The tool is called **Stellar**, and it is used to create, build, verify, and lint Stars and Nebulae.

---

## ✨ What is Stellar?
Stellar is a CLI tool used by package authors and system maintainers. It performs tasks related to:

- Scaffolding new Stars
- Fetching remote sources for local builds
- Building package tarballs
- Running install logic (optionally using Nova)
- Validating dependencies
- Packaging and indexing for Galaxies

Stellar is not required for *using* Cosmos—only for *building* Stars and Galaxies.

---

## 🚀 Commands

### `stellar new-star <name>`
Scaffolds a new `star.toml` and optional build script.

- Prompts for version, description, type (normal/nebula)
- Creates folder with stub files and `files/` layout

### `stellar build-star <path>`
Builds a `.tar.gz` of the Star using `star.toml`

- Copies files into temp dir
- Includes optional `install.lua` Nova script if present
- Runs Nova script to prepare files (if needed) during build
- Outputs tarball in `./dist/` or configured output dir
- Produces a `.tar.gz` with the contents of `files/` and any `install.lua` script

### `stellar fetch <path>`
Fetches the *remote source* defined in `star.toml`
- Supports HTTP or local file URI (no Git yet)
- Useful when a Star's `source` points to a raw upstream tarball
- Downloads it for `build()` usage inside `install.lua`
- Does **not** modify `star.toml`
- Maintainer must later update `source` to the final tarball path post-build

### `stellar validate <path>`
Validates:

- Presence of required fields
- Correct semver and dependencies
- Script syntax (basic checks)
- Source URL accessibility (if specified)
- Ensures only one install script exists (`install.lua` or `install.sh`)

### `stellar lint <path>` *(future)*
Gives advice on naming conventions, doc clarity, and structure

### `stellar galaxy-init <name>`
Creates a new Galaxy repo with:

- `meta.toml`
- `stars/` directory
- `packages/` directory

### `stellar index-galaxy <path>` *(future)*
Generates/updates the `[stars]` table in `meta.toml`

- Scans `stars/` for `.toml` files
- Extracts name + version
- Sorts and deduplicates entries

---

## 🔍 Example Workflow
```bash
stellar new-star hello
# edit files and install.lua...
stellar fetch ./hello       # get source code
stellar build-star ./hello  # run build() inside install.lua
stellar validate ./hello/star.toml
# manually update star.toml source = "file://..."
# copy tarball to core-galaxy/packages/
```

---

## 📂 Layout and Defaults

```
stars/
  zlib/
    star.toml
    install.lua
    files/
      usr/
        include/
        lib/
```

- `files/` is the root of what will be tarred
- Either `install.lua` (Nova script) or `install.sh` may be present, but Nova is preferred
- Only one should be used per package; `install.lua` is executed by Cosmos during install and should target `install_root`
- If neither script is present, Cosmos defaults to copying `files/*`

---

## 🥰 Future Features
- Templates for common build systems (e.g. autotools, CMake, etc.)
- Nova scripting enhancements and validations
- Integration with `cosmos verify` for reproducible build hashing

Stellar helps standardize package creation across all Galaxies and ensures a repeatable, minimal workflow.
