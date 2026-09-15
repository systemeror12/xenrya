# 3. MVP & Phase Gating

## MVP Goal

Prove the complete **Developer Companion loop**:

```text
Git activity
    ↓
Event detected
    ↓
Activity Engine
    ↓
Companion reaction
    ↓
Useful information
```

## MVP Success Statement

> I can launch Xenrya manually on my Linux desktop, see my chosen character moving in a limited area, register multiple Git repositories, and have the companion visually react and show useful information when meaningful Git activity occurs—all without AI.

## MVP Local Git Events

Required:
- Repository registration
- Branch change
- New commit
- Dirty → clean
- Clean → dirty
- Merge conflict detection
- Ahead / behind

Deferred:
- Worktrees
- Stashes
- Dedicated unpushed-commit logic
- Deeper remote-state monitoring

## MVP Remote Platform

**GitHub first**

Potential read-only capabilities:
- PR notifications
- Review requests
- PR status updates
- GitHub Actions awareness
- GitHub milestone awareness
- Clickable links to resources

GitLab comes later and only after GitHub is mature.

## MVP Milestones

Local milestones only.

Fields / concepts:
- Title
- Description
- Progress
- Due date
- Status
- Items

## MVP Character Runtime

Required:
- Character rendering
- Idle animation
- At least one reaction animation
- Left click
- Right click
- Drag
- Hover
- Keyboard shortcut
- Limited autonomous movement
- Speech / information bubble
- Hide / show

## MVP Character Packs

Character-pack support starts in MVP.

Example:

```text
character/
├── character.toml
├── sprites/
├── sounds/
└── assets/
```

Packs are data-only.

## MVP Settings

GUI:
- Launch automatically
- Autonomous movement
- Activity level
- Notification toggles
- Sounds
- Repository registration
- Watched directories
- Character selection

Advanced configuration:
- TOML

## Not Required for MVP

- GitLab
- AI
- AI chat
- Source-code analysis
- Coding assistant
- System monitoring
- Music awareness
- Pomodoro
- Autonomous coding actions
- Repository writes
- Deep Hyprland integration
- Windows support
- Full project management

---

# Phase Gating

## Phase 0 — Foundation

Scope:
- Repository setup
- Coding standards
- Application shell
- Linux window prototype
- Character renderer prototype
- Event bus foundation
- Logging
- Configuration foundation
- Testing foundation
- Development packaging

Exit gate:

> Xenrya can launch reliably on Linux, load configuration, render a minimal companion window, and process internal events.

## Phase 1 — MVP

Includes the settled MVP scope.

Exit gate:
- MVP success statement satisfied
- Automated checks pass
- Resource usage acceptable
- Human UX review passes

## Phase 2 — Mature GitHub

GitHub must become reliable and mature before GitLab begins.

Potential scope:
- Authentication
- Multiple accounts
- Pull requests
- Issues
- Reviews
- Review requests
- GitHub Actions
- Milestones
- Rate-limit handling
- Offline/cache behavior
- Notification filtering
- Resource links

## Phase 3 — GitLab

Potential scope:
- Authentication
- Merge requests
- Issues
- Reviews
- Pipelines
- Milestones
- Resource links

## Phase 4 — Linux / Hyprland Integration

Potential scope:
- Workspace awareness
- Active-window awareness
- Monitor awareness
- Fullscreen state
- Compositor events
- Desktop placement integration

Read-only first.

## Phase 5 — Companion Ecosystem

Potential scope:
- Mature character-pack spec
- Third-party packs
- Pack validation
- Personality presets
- Sound/theme packs
- Community distribution
- Permissioned extension APIs

## Phase 6 — Optional Intelligence

Potential scope:
- Contextual dialogue
- Developer activity summaries
- PR/MR summaries
- Failure explanation
- OpenAI-compatible APIs
- ChatGPT subscription integration
- Local models
- Permission-aware context sharing

## Phase 7 — Advanced Productivity

Possible future capabilities:
- Pomodoro / focus
- Task management
- System monitoring
- Music awareness
- CPU/RAM widgets
- Natural-language computer control
- AI chat
- Coding-agent integration
- Permissioned shell execution
- Autonomous actions

## Gate Philosophy

```text
Implementation
    ↓
Automated checks
    ↓
Integration / quality checks
    ↓
Human review
    ↓
Gate passed
```

Experimental work on future phases is allowed in isolated branches/worktrees.

If a feature grows beyond the intent of its phase, split or defer it rather than expanding the phase indefinitely.
