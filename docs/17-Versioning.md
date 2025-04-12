# 17 – Versioning and Release Policy

This document defines the versioning strategy for Cosmos Stars, Galaxies, and the overall ecosystem. It also outlines how packages should be updated, deprecated, or removed.

---

## 🔄 Versioning Philosophy
- **Stars follow SemVer** (`major.minor.patch`)
- **Galaxies are versioned by date** (`YYYY.MM.DD`, e.g., `2024.04.17`)
- **Cosmos itself** will follow SemVer but moves slowly

Versioning exists for **clarity and reproducibility**, not hype. If nothing changed, the version doesn't change.

---

## ✨ Star Versioning
Every Star must define:
```toml
version = "1.2.3"
```
- Stars are expected to follow SemVer
- Breaking changes to install logic = major bump
- Dependency changes = minor bump
- Tarball rebuild or checksum-only = patch bump

New versions must be uploaded with updated filenames:
```
zlib-1.2.3.tar.gz
```

Old versions may be kept for compatibility.

---

## 🌌 Galaxy Versioning
Each Galaxy has a `meta.toml` with:
```toml
version = "2024.04.17"
```
This version is used for:
- Identifying sync points
- Snapshotting a full system state
- Reference in freeze/lockfile scenarios

**Preferred format: `YYYY.MM.DD`** for clarity and traceability
- Easy to sort
- Helps with security updates or multiple changes in a month

Galaxies **must** update `meta.toml` if any of the following change:
- New Star added
- Existing Star updated
- Any Star removed

### 🔄 Galaxy Archival (Recommended)
- It is **not required** to archive every Galaxy version
- However, maintainers are **strongly encouraged** to retain the past 3–6 months of Galaxy snapshots
- For long-term reproducibility (e.g., distro builds), consider archiving stable versions **annually**
- Rolling releases can prune aggressively, but should still tag known good versions

Ultimately, archival policies are up to the maintainer. Cosmos recommends preserving history, but does not enforce it.

---

## 🌍 Constellation Stability
Constellations are install presets. If a Constellation references Stars that change, that's fine—but users should be encouraged to:
- Pin Constellation files to Galaxy versions
- Create locked snapshots of their system after install

Example:
```toml
members = [
  "core-utils@1.0.0",
  "busybox@1.36.0"
]
```

Version pinning is optional but strongly encouraged for reproducibility.

---

## ❌ Deprecation and Removal
### Stars:
- Deprecated Stars should remain in the Galaxy but clearly marked in `description`
- Removed Stars must be deleted from `stars/` and `packages/` but remain listed in old Galaxy versions

### Galaxies:
- Galaxies are never deleted, only archived with new versions (if the maintainer chooses to)
- The latest version is expected to be symlinked or aliased in mirrors

---

## 🔖 Release Recommendations
- Use Git tags for Galaxy releases (`core-2024.04.17`)
- Use signed commits if hosting public Galaxies
- Tarballs should include version in filename
- Include changelogs if maintaining official sets

---

Versioning is about trust and stability, not chasing numbers. Cosmos prefers boring, explicit, and stable packages over rapid churn.
