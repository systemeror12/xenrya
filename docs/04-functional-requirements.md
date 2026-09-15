# 4. Functional Requirements

## Repository Monitoring

### FR-001 — Hybrid Git Monitoring
Use filesystem events, selective Git queries, and low-frequency reconciliation.

### FR-002 — Per-Repository Monitoring Control
Each registered repository can independently enable/disable monitoring.

### FR-003 — Repository Discovery
Support:
- Manual repository registration
- Watched directories
- Configurable recursion depth

### FR-004 — Recent Activity Timeline
Maintain a short local recent-activity history.

## Milestones

### FR-005 — Automatic Milestone Progress
Calculate progress from completed applicable items.

### FR-006 — Manual Progress Override
Allow users to override computed progress.

### FR-007 — Rich Milestone Item States
Initial states:
- Planned
- In Progress
- Blocked
- Complete
- Cancelled

### FR-020 — Multiple Concurrent Milestones
A project may have multiple active milestones.

### FR-021 — Configurable Due-Date Notifications
Users choose reminder thresholds.

### FR-022 — Lightweight Milestone Items
Milestone items remain intentionally simple.

## GitHub

### FR-008 — Multiple GitHub Accounts
Support multiple accounts architecturally.

### FR-009 — Configurable Notification Scope
Default to registered Xenrya projects, with broader account scope optional.

### FR-010 — Notification Grouping
Group related updates around the same remote resource where practical.

## Reactions & Notifications

### FR-011 — Independent Event Behaviors
Character reactions and notifications are separately configurable.

### FR-012 — Sensible Defaults
Routine events are subtle; important events notify; critical events are clearly visible.

### FR-013 — Built-In Activity Center
Provide a lightweight recent-activity center.

### FR-014 — Linux System Notifications
Support Linux notification daemon integration as a user-configurable output channel.

## Character Packs

### FR-015 — Manual Character Installation
Allow direct installation into Xenrya data directories.

### FR-016 — GUI Character Import
Allow importing packs through the UI.

### FR-017 — Character Packs Are Data-Only
Allowed:
- Images
- Sprites
- Animations
- Sounds
- Config
- Reaction mappings
- Dialogue
- Metadata

Disallowed:
- Arbitrary executable code

### FR-018 — Character-Specific Reaction Mapping
Character authors can map semantic events to character-specific reactions.

### FR-019 — Reaction Fallback

```text
Specific reaction
    ↓
Generic emotional reaction
    ↓
Idle / default
```
