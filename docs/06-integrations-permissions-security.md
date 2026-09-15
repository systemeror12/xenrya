# 6. Integration Boundaries, Permissions, Privacy & Security

## Git

- Read-only
- Normalized events
- Installed Git CLI initially
- Adapter abstraction allows replacement later
- Companion/UI never invokes Git directly

## GitHub

- Read-only initially
- Multiple accounts
- Browser/OAuth-style auth plus supported manual methods where practical
- Repository account inference with user override
- Adaptive synchronization
- API rate-limit awareness

## Linux Notifications

Output only.

Xenrya does not intercept unrelated application notifications.

## Hyprland

Read-only first.

Possible observations:
- Workspace changes
- Active window
- Active monitor
- Fullscreen state
- Compositor events

Desktop control may be reconsidered later through an explicit decision.

## Character Packs

- Data-only
- Self-contained
- Relative asset paths
- No network
- No shell
- No external filesystem access
- No arbitrary code

## Plugin Permission Model

Capability-based and deny-by-default.

Examples:
- `media.read`
- `companion.react`
- `filesystem.read`
- `filesystem.write`
- `network.access`
- `shell.execute`
- `desktop.control`

Sensitive capabilities require explicit approval and stronger warnings.

## AI Access Model

AI is disabled by default.

Possible profiles:
- Minimal
- Developer Context
- Broad
- Full Access
- Custom

Full access is allowed only when explicitly selected by the user.

## AI Provider Direction

Current planned direction:
- OpenAI-compatible APIs
- ChatGPT subscription integration

This may evolve later.

## Permission Revocation

Users can revoke or disable:
- GitHub connections
- AI access
- Plugins
- Integration capabilities

without reinstalling Xenrya.

## Source Code Access

Source-code access is a separate capability.

`repository.metadata.read` does **not** imply `repository.source.read`.

## Shell Execution

Approved as a future permissioned capability.

Shell permission does not automatically grant unrelated Xenrya capabilities.

## User Data Controls

Eventually provide:
- Export Xenrya data
- Clear activity history
- Delete project data
- Remove integration data
- Reset Xenrya

## Privacy Rule

```text
Capability
   +
User permission
   +
Feature necessity
   =
Allowed access
```

Technical access alone never implies permission to use or transmit data.

## Telemetry

**None.**
