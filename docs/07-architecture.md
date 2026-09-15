# 7. Settled Architecture

## Core/UI Separation

### Rust Core owns
- Activity Engine
- Policy Engine
- Reaction resolver
- Git adapter
- GitHub adapter
- Milestones
- Persistence
- Permissions
- Integration coordination

### Qt/QML owns
- Character rendering
- Animations
- Speech bubbles
- Settings UI
- Activity UI
- Presentation logic

Business/domain logic should not migrate into QML.

## Process Model

Start as a single process with strong internal boundaries.

The architecture should allow a future Core/UI process split if justified.

## Central Event Bus

All meaningful developer activity flows through normalized events.

Examples:

```text
repository.commit_detected
repository.branch_changed
repository.conflict_detected
github.pull_request.approved
milestone.completed
```

## Event Persistence

Each event type declares whether it should be persisted.

Examples:
- `character.hover` → transient
- `character.idle` → transient
- `repository.commit_detected` → persisted
- `github.pull_request.approved` → persisted

## Policy Engine

Responsible for:
- Reaction enable/disable
- Notification channels
- Quiet modes
- Grouping
- Sound policy
- Importance
- User overrides

Adapters do not decide UI behavior.

## Persistence Split

### SQLite
- Projects
- Repositories
- Milestones
- Activity
- Integration metadata
- Dynamic app state

### TOML
- Human-editable config
- Advanced behavior
- Portable preferences

### Secret Store
- GitHub/GitLab credentials
- AI API keys
- Sensitive tokens

## Character Runtime

Consumes semantic reactions, not raw Git/GitHub events.

```text
github.pull_request.approved
        ↓
Policy / Reaction Resolver
        ↓
reaction.success_major
        ↓
Character Runtime
        ↓
Character-specific animation
```

## Integration Adapters

Conceptual lifecycle:

```text
initialize
health
sync
events
shutdown
```

First-party adapters:
- Git
- GitHub
- GitLab
- Hyprland

## Plugin Runtime

Not implemented in MVP. Only clean boundaries are prepared.

## Runtime States

### Running
- Character visible
- Monitoring active
- Notifications active

### Hidden
- Character invisible
- Monitoring active
- Notifications active

### Paused
- Monitoring paused/reduced
- Reactions/normal notifications suppressed

### Quit
- Everything stops

## High-Level Diagram

```text
                         Qt / QML
                    Character + UI
                           │
                        CXX-Qt
                           │
                           ▼
                      Rust Core
                           │
          ┌────────────────┼────────────────┐
          │                │                │
     Activity Engine  Policy Engine      Storage
          │                │                │
          └────────────────┼────────────────┘
                           │
                        Event Bus
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
      Git CLI           GitHub             Milestones
        │                  │
        └──────────── Rust Adapters ──────────┘
                           │
                           ▼
                    SQLite / TOML /
                     Secret Storage
```
