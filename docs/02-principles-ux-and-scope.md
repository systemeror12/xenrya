# 2. Core Principles, UX & Scope

## Core Product Principles

1. **Local-first**
2. **Information first, personality second**
3. **User-controlled proactivity**
4. **Deep customization**
5. **Controlled hackability**
6. **AI optional by design**
7. **Advise, never police**
8. **Manual operation must always be possible**
9. **Developer activity awareness is the core capability**
10. **External platforms are integrations, not the core**
11. **Resource efficiency is a product requirement**
12. **Observation and mutation are separate**

## Product Personality & UX

### Visual direction
- Chibi characters
- Pixel-art characters
- Community-created characters

### Character identity
Xenrya is the platform. The active companion is user-selected.

### Placement
User-controlled. Potential modes:
- Desktop layer
- Floating / overlay
- Docked area
- Temporary appearance
- Hidden state

### Movement
Autonomous movement is allowed, but only within a limited/controlled area so it does not interfere with work.

### Core interactions
- Left click
- Right click
- Drag
- Hover
- Keyboard shortcut

### Personality
Strength: **7 / 7**

May include:
- Mannerisms
- Emotional states
- Humor
- Moods
- Expressive reactions
- Idle behavior
- Contextual dialogue

### Personality customization
Users may control:
- Reaction intensity
- Talk frequency
- Humor
- Sarcasm
- Developer jokes
- Celebration behavior
- Failure reactions
- Idle commentary

### Interruptions
User-configurable:
- Passive animation
- Speech bubbles
- Xenrya activity center
- Linux notifications
- Sound
- Prominent animations

Xenrya must not steal application focus by default.

### Sound
Supported but disabled by default. Users can choose or replace sound effects.

## Scope

### Local Git awareness
Supported:
- Repository detection
- Current branch
- Clean / dirty working tree
- Commits
- Branch changes
- Merge conflicts
- Worktrees
- Stashes
- Remote status
- Ahead / behind
- Unpushed commits

Explicitly excluded:
- Test results
- Build results
- Tags / releases

### Repository discovery
- Manual repository registration
- Watched directories
- Configurable recursive discovery depth

### Lightweight project management
Supported:
- Milestones
- Milestone items
- Progress
- Due dates
- Notes
- Lightweight priority/status

Not intended to become:
- Jira
- Linear
- GitHub Projects replacement
- Sprint planner
- Kanban suite
- Time-tracking system

### Source code
Xenrya Core does **not** inspect, index, embed, or understand source code.

### Repository mutation
Xenrya never performs repository mutations such as commit, checkout, merge, rebase, push, pull, reset, or stash mutation.

### Multiple projects
Multiple projects and repositories may be monitored simultaneously.

## Explicit Anti-Goals

Xenrya should not:
- Become a generic chatbot.
- Consume excessive system resources.
- Become annoying or intrusive.
- Require AI.
- Offer limited customization.
- Assume it must always run.
- Become a replacement IDE.
- Become a full Git client.
- Replace coding agents.
- Become a full project-management platform.
