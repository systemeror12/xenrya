# 11. Configuration & Customization

## Configuration Precedence

```text
Built-in Defaults
       ↓
Global User Config
       ↓
Character Defaults
       ↓
Project Overrides
       ↓
Temporary Runtime Overrides
```

User settings always beat character defaults.

## XDG Paths

```text
$XDG_CONFIG_HOME/xenrya/
$XDG_DATA_HOME/xenrya/
$XDG_CACHE_HOME/xenrya/
$XDG_STATE_HOME/xenrya/
```

Typical:

```text
~/.config/xenrya/
~/.local/share/xenrya/
~/.cache/xenrya/
~/.local/state/xenrya/
```

## Main Config

Initially:

```text
~/.config/xenrya/config.toml
```

Prefer one main file until splitting is genuinely useful.

## Live Reload

Manual TOML edits should be detected and safely reloaded.

If invalid:
- Keep previous working configuration
- Show error
- Preserve user file
- Never overwrite silently

## GUI ↔ TOML

GUI changes should write corresponding TOML settings where practical.

## Per-Project Overrides

Allowed for settings that genuinely make sense per project.

## Character Overrides

Users can override pack defaults without modifying the original downloaded pack.

## Import / Export

Eventually support:
- Export config
- Import config

Secrets excluded by default.

Repository paths require portability-aware handling.

## Dotfile Friendliness

Design goals:
- Stable human-readable TOML
- No secrets in config
- Minimal machine-specific noise
- Backward-compatible migrations where practical
- Suitable for Git/dotfiles

## Reset Scopes

Separate operations:
- Reset appearance
- Reset notifications
- Reset companion behavior
- Reset config
- Clear activity history
- Remove project data
- Factory reset
