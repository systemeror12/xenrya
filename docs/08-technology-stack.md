# 8. Approved Technology Stack

## Language
**Rust**

## UI
- Qt 6
- QtQuick
- QML

## Rust ↔ Qt
**CXX-Qt**

## Platform
- Linux native first
- Wayland only
- Hyprland-first

## Async Runtime
**Tokio**

Use async where appropriate; keep domain logic synchronous where possible.

## Database
- SQLite
- `rusqlite`
- Versioned forward migrations

## Configuration
- TOML
- Serde

## Git
Installed Git CLI behind a Rust adapter.

## HTTP
**Reqwest**

## API / Structured Serialization
- Serde
- `serde_json`

## Diagnostics
- `tracing`
- `tracing-subscriber`

## Error Handling
Typed domain errors.

## Character Animation
- Sprite sheets
- Frame sequences

## Project Organization
Modular Cargo workspace.

Suggested initial shape:

```text
xenrya/
├── crates/
│   ├── xenrya-core/
│   ├── xenrya-events/
│   ├── xenrya-git/
│   ├── xenrya-github/
│   ├── xenrya-storage/
│   └── xenrya-ui/
├── assets/
├── characters/
└── Cargo.toml
```

## Wayland Surface Abstraction

QML should use a platform-neutral `CompanionSurface` API.

```text
QML
 │
 ▼
CompanionSurface API
 │
 ├── Linux / Wayland implementation
 │
 └── Future Windows implementation
```

Linux-specific Layer Shell/compositor logic must remain behind the platform boundary.

## Testing Philosophy

Most product/domain behavior should be testable without launching Qt.
