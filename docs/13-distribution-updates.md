# 13. Distribution & Updates

## Initial Distribution

- AUR stable: `xenrya`
- AUR development: `xenrya-git`
- Generic release binaries

## Source Builds

Fully supported for power users.

## Flatpak

Post-MVP distribution target.

Must evaluate interactions with:
- Repository access
- Wayland/Layer Shell
- Secure credentials
- Hyprland integration
- Future shell execution
- Character/plugin directories

## AppImage

Possible later if demand justifies it.

## Update Philosophy

No automatic self-installing updater on Linux initially.

```text
AUR install
→ package manager handles update

Flatpak
→ Flatpak handles update

Manual binary
→ Xenrya may optionally notify about new release
```

## Release Checking

Manual installs may optionally check for new releases.

This is not telemetry.

## Versioning

Semantic Versioning:

```text
0.1.0
0.2.0
...
1.0.0
```

Character/plugin APIs may have their own compatibility versions.

## Config Migration

```text
old config
    ↓
backup
    ↓
migrate
    ↓
validate
    ↓
new config
```

Failure behavior:
- Preserve original
- Report error
- Use safe fallback

## Database Migration

Before destructive migrations:
- Backup DB
- Record schema version
- Validate migration
- Preserve rollback path where practical

Especially important during pre-1.0 development.
