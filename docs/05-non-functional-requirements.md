# 5. Non-Functional Requirements

### NFR-001 — Extremely Low Idle CPU
Idle operation should approach 0% CPU whenever practical.

### NFR-002 — Memory Budget From Benchmarking
Do not set arbitrary RAM limits before Phase 0 measurements.

### NFR-003 — Startup Should Feel Immediate
UI/character should appear quickly; integrations can initialize asynchronously.

### NFR-004 — Offline-First Degradation
Local features continue normally without internet.

### NFR-005 — Integration Failure Isolation
One failed integration must not crash Xenrya.

### NFR-006 — Character-Pack Failure Isolation
Invalid packs fall back to a known-good character.

### NFR-007 — Safe Configuration Recovery
Broken config:
- Preserve file
- Report error
- Use last-known-good/defaults
- Never silently overwrite

### NFR-008 — Modern Linux / Wayland First
Arch-based environments may be the primary development environment initially. Official distro support requires real testing.

### NFR-009 — Hyprland Is Not Required for Core Product
Hyprland enhances Xenrya but does not define it.

### NFR-010 — Battery Optimization Post-MVP
Battery-aware behavior is a later optimization.

### NFR-011 — Wayland Only Initially
X11 is not supported initially.

### NFR-012 — Multi-Monitor Support Required
Basic multi-monitor correctness is required.

### NFR-013 — HiDPI / Fractional Scaling Required
Support modern Wayland scaling.

### NFR-014 — Baseline Accessibility
At minimum:
- Disable autonomous movement
- Disable sounds
- Notification intensity control
- Keyboard-accessible settings
- Readable text
- UI scaling
- Reduced motion where practical

### NFR-015 — User Owns Their Data
Local data stays local by default.

### NFR-016 — AI Data Sharing Disabled by Default
AI and external data sharing start OFF.

### NFR-017 — No Telemetry
No usage analytics or hidden behavioral tracking.

### NFR-018 — Configurable Activity Retention
Users control local activity retention.

### NFR-019 — Secure Secret Storage
Credentials must not be stored in ordinary TOML/SQLite fields.

### NFR-020 — Minimal Logging by Default
Verbose diagnostics are opt-in.

### NFR-021 — Quit Means Quit
Quit stops UI, watchers, sync, and background processes completely.
