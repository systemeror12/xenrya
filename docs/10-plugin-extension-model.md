# 10. Plugin / Extension Model

## Extension Categories

1. **Character Pack**
   - Data only

2. **Integration Plugin**
   - Adds external awareness/capabilities

3. **UI Extension**
   - Adds optional Xenrya UI surfaces/widgets

Examples:
- Spotify awareness → Integration Plugin
- Docker awareness → Integration Plugin
- Custom status widget → UI Extension
- Pixel companion → Character Pack

## Execution Model

Hybrid:

```text
Trusted first-party module
        ↓
May run internally

Third-party executable plugin
        ↓
Prefer isolated / out-of-process
```

## Permissions

Deny-by-default.

A plugin gets only capabilities declared in its manifest and approved by the user.

## API Versioning

Example:

```toml
plugin_api = 1
```

Incompatible plugins should fail cleanly.

## Discovery

Potential methods:
- Local plugin folder
- UI import
- Future community registry/store

## Normalized Events

Plugins may emit events into the Activity Engine.

Example:

```text
Docker Plugin
    ↓
docker.container_failed
    ↓
Activity Engine
    ↓
Policy Engine
    ↓
Reaction / Notification
```

## Renderer Boundary

Plugins do not directly control character animations.

They emit events/semantic reactions; Character Runtime chooses the actual animation.

## High-Risk Capabilities

Allowed only with explicit warnings/approval:
- `shell.execute`
- `network.access`
- `filesystem.write`
- `desktop.control`

## First-Party Built-Ins

Remain built-in and do not depend on plugin runtime:
- Git
- GitHub
- GitLab
- Hyprland
