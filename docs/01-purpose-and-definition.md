# 1. Purpose & Project Definition

## Purpose

Xenrya is a Linux-native developer companion designed to help users stay organized and productive while making the desktop feel more personal, expressive, and alive.

It should reduce the monotony of long coding sessions, react meaningfully to development activity, and gradually integrate more deeply with the desktop environment without becoming intrusive or resource-heavy.

AI is optional and must never be required for core functionality.

### Primary motivations

- Help the user stay organized and productive.
- Make the desktop more personalized.
- Build something fun and expressive.
- Reduce the monotony of coding sessions.
- Support deeper Linux desktop integration over time.
- Remain useful without AI.
- Always allow manual launch and full shutdown.

## Product Name

**Xenrya**

Xenrya is the platform/application, not a single fixed companion character.

## Product Domain

### Primary
**Desktop Developer Companion**

### Secondary
- Developer tool
- Desktop utility
- Desktop pet / character application
- Optional AI application

## Target Users

### Primary
- Linux developers
- Hyprland users
- Linux customization / ricing community
- GitHub / GitLab power users

### Secondary
- Developers generally
- People interested in desktop pets / companions

### Skill level
Both normal users and power users:
- GUI and sensible defaults for normal users.
- Deep configuration and hackability for advanced Linux users.

## Primary Use Case

**Developer Awareness**

Xenrya observes meaningful development activity and presents it through a living desktop companion.

Examples:
- Git commits
- Branch changes
- Merge conflicts
- Dirty / clean repository state
- Ahead / behind state
- GitHub PR / review / Actions / milestone activity
- GitLab equivalents later

### Core loop

```text
Something happens
      ↓
Xenrya detects it
      ↓
Normalized developer event
      ↓
Policy Engine
      ↓
React / Notify
      ↓
Useful information
```

## Milestone Concept

Xenrya supports a normalized milestone concept:

```text
                    Xenrya Milestone
                          │
         ┌────────────────┼────────────────┐
         │                │                │
      Local             GitHub           GitLab
     editable          imported         imported
```

Remote milestones remain source-aware and read-only where appropriate.
