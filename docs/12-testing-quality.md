# 12. Testing & Quality Strategy

## Rust Core

Unit/integration tests for:
- Milestone progress
- Notification-policy resolution
- Permission decisions
- Config precedence
- Reaction fallback
- Event normalization
- Persistence
- Adapter contracts

## Git Adapter

Use real temporary Git repositories.

Test flows such as:
- init
- commit
- branch switch
- dirty state
- merge conflict
- ahead / behind
- normalized event output

## GitHub

Use:
- Mocked HTTP responses
- Controlled fixtures
- Optional live maintainer tests

Do not make normal tests depend on real GitHub accounts.

## SQLite Migrations

Every migration should be tested against:
1. A fresh database
2. The immediately previous schema version
3. Existing data preservation

## Character Pack Validator

Validate:
- Manifest correctness
- Missing assets
- Invalid paths
- Unsupported fields
- Reaction fallbacks
- External filesystem references
- Security restrictions

## QML/UI

Focus automated UI tests on behavior:
- Settings update state
- Hide/show
- Drag interaction
- Fallback behavior
- Notification actions

Visual regression testing is optional where it becomes genuinely useful.

## Performance

Release validation measures:
- Idle CPU
- Memory
- Startup responsiveness
- Repository watcher overhead
- Animation load

Hard thresholds are set only after Phase 0 measurements.

## Display Coverage

Test:
- Single monitor
- Multiple monitors
- Mixed scaling
- Monitor hotplug
- 100%
- 125%
- 150%
- 200%

## Human Review

A phase can fail even with green automated tests if Xenrya is:
- Annoying
- Distracting
- Visually broken
- Resource-heavy
- Poor to use

## Release Channels

Long-term:
- Nightly
- Beta
- Stable
