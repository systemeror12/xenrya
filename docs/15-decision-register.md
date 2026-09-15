# 15. Decision Register

This register summarizes the most consequential settled decisions from the discovery Q&A.

| ID | Decision | Status |
|---|---|---|
| D-001 | Product name is Xenrya | Settled |
| D-002 | Primary domain is Desktop Developer Companion | Settled |
| D-003 | Developer Awareness is the core use case | Settled |
| D-004 | AI is optional | Settled |
| D-005 | Linux native first; Windows only possible later | Settled |
| D-006 | Wayland only initially; no X11 support | Settled |
| D-007 | Hyprland first, but core must not require Hyprland | Settled |
| D-008 | GitHub first; GitLab only after GitHub is mature | Settled |
| D-009 | Git/GitHub/GitLab awareness is read-only initially | Settled |
| D-010 | Xenrya never modifies local repositories | Settled |
| D-011 | Xenrya Core does not analyze/index source code | Settled |
| D-012 | Multiple projects/repositories are supported | Settled |
| D-013 | Local milestones + imported remote milestones | Settled |
| D-014 | Strong personality (7/7), user-customizable | Settled |
| D-015 | Chibi and pixel-art character support | Settled |
| D-016 | Character packs are data-only | Settled |
| D-017 | User controls proactivity, notifications, sound, and movement | Settled |
| D-018 | Sounds supported but disabled by default | Settled |
| D-019 | No telemetry | Settled |
| D-020 | User owns local data | Settled |
| D-021 | AI/data sharing disabled by default | Settled |
| D-022 | Full AI context access is allowed only with explicit user permission | Settled |
| D-023 | Shell execution is future permissioned capability | Settled |
| D-024 | Core and UI are separated conceptually | Settled |
| D-025 | Single process initially with future split possible | Settled |
| D-026 | Normalized event bus is central architecture | Settled |
| D-027 | SQLite + TOML + secure secret store persistence split | Settled |
| D-028 | Rust is the core language | Settled |
| D-029 | Qt 6 / QtQuick / QML is the UI stack | Settled |
| D-030 | CXX-Qt bridges Rust and Qt | Settled |
| D-031 | Tokio is the async runtime | Settled |
| D-032 | rusqlite is the SQLite access layer | Settled |
| D-033 | Reqwest is the default HTTP client | Settled |
| D-034 | Serde + TOML / serde_json for serialization | Settled |
| D-035 | tracing ecosystem for diagnostics | Settled |
| D-036 | Modular Cargo workspace from the beginning | Settled |
| D-037 | AUR + generic binaries are initial distribution paths | Settled |
| D-038 | Flatpak is post-MVP | Settled |
| D-039 | Project is open-source | Settled |
| D-040 | Project license is MIT | Settled |

## Deferred / Open Items

These are intentionally not fully settled yet:

- Exact officially supported Linux distro matrix
- Hard memory budget
- Hard startup-time threshold
- Packaged `.xenrya` character archive format
- AppImage commitment
- Final Windows implementation strategy
- Exact plugin sandbox mechanism
- Exact migration library
- Detailed Layer Shell implementation
- Future Caelestia integration details
