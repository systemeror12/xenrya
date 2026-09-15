# 9. Data Model

# Conceptual ERD

```mermaid
erDiagram
    PROJECT ||--|{ REPOSITORY : contains
    PROJECT ||--o{ MILESTONE : owns
    PROJECT ||--o{ ACTIVITY_EVENT : produces
    PROJECT }o--o| CHARACTER_PACK : overrides_with

    REPOSITORY ||--o{ REMOTE_REPOSITORY : has
    REPOSITORY ||--o{ ACTIVITY_EVENT : relates_to

    REMOTE_REPOSITORY }o--|| INTEGRATION_ACCOUNT : authenticated_by
    REMOTE_REPOSITORY ||--o{ REMOTE_RESOURCE : exposes
    REMOTE_REPOSITORY ||--o{ ACTIVITY_EVENT : relates_to

    MILESTONE ||--o{ MILESTONE_ITEM : contains

    REMOTE_RESOURCE ||--o{ ACTIVITY_EVENT : referenced_by

    INTEGRATION_ACCOUNT ||--o{ ACTIVITY_EVENT : source_of

    CHARACTER_PACK ||--o{ CHARACTER_REACTION : defines
```

## Conceptual Decisions

- Project and Repository are different concepts.
- One repository belongs to one Xenrya project at a time.
- Projects currently require at least one repository.
- Repositories may have multiple remotes.
- One remote may be primary.
- Milestones belong to Projects.
- Imported milestones remain source-aware.
- Activity events have a primary Project and optional related entities.
- PRs/MRs/issues/reviews/pipelines use a normalized `Remote Resource`.
- Character selection is global-default + optional project override.

---

# Actual ERD

```mermaid
erDiagram
    PROJECTS {
        integer id PK
        uuid uid UK
        text name
        text description
        datetime created_at
        datetime updated_at
    }

    REPOSITORIES {
        integer id PK
        uuid uid UK
        integer project_id FK
        text name
        text local_path UK
        boolean monitoring_enabled
        datetime created_at
        datetime updated_at
    }

    INTEGRATION_ACCOUNTS {
        integer id PK
        uuid uid UK
        text provider
        text display_name
        text external_account_id
        text username
        boolean enabled
        datetime created_at
        datetime updated_at
    }

    REPOSITORY_REMOTES {
        integer id PK
        integer repository_id FK
        integer integration_account_id FK
        text name
        text provider
        text remote_url
        boolean is_primary
        text provider_owner
        text provider_repo
        text provider_id
        datetime created_at
        datetime updated_at
    }

    MILESTONES {
        integer id PK
        uuid uid UK
        integer project_id FK
        integer remote_resource_id FK
        text source_type
        text title
        text description
        text status
        datetime due_at
        text progress_mode
        float manual_progress
        datetime created_at
        datetime updated_at
    }

    MILESTONE_ITEMS {
        integer id PK
        uuid uid UK
        integer milestone_id FK
        text title
        text note
        text status
        text priority
        integer position
        datetime created_at
        datetime updated_at
        datetime completed_at
    }

    REMOTE_RESOURCES {
        integer id PK
        uuid uid UK
        integer remote_repository_id FK
        text resource_type
        text provider_resource_id
        integer number
        text title
        text state
        text url
        json metadata_json
        datetime last_seen_at
        datetime created_at
        datetime updated_at
    }

    ACTIVITY_EVENTS {
        integer id PK
        uuid uid UK
        integer project_id FK
        integer repository_id FK
        integer remote_repository_id FK
        integer remote_resource_id FK
        integer integration_account_id FK
        text event_type
        text importance
        text summary
        json payload_json
        datetime occurred_at
    }

    CHARACTER_PACKS {
        integer id PK
        uuid uid UK
        text pack_id UK
        text name
        text version
        text path
        boolean enabled
        text validation_state
        datetime installed_at
        datetime updated_at
    }

    PROJECT_CHARACTER_OVERRIDES {
        integer project_id PK,FK
        integer character_pack_id FK
    }

    NOTIFICATION_POLICIES {
        integer id PK
        integer project_id FK
        text event_pattern
        boolean reaction_enabled
        boolean xenrya_notification_enabled
        boolean system_notification_enabled
        boolean sound_enabled
        text importance_override
        datetime updated_at
    }

    PROJECTS ||--|{ REPOSITORIES : contains
    PROJECTS ||--o{ MILESTONES : owns
    PROJECTS ||--o{ ACTIVITY_EVENTS : produces
    PROJECTS ||--o| PROJECT_CHARACTER_OVERRIDES : overrides
    PROJECTS ||--o{ NOTIFICATION_POLICIES : customizes

    REPOSITORIES ||--o{ REPOSITORY_REMOTES : has
    REPOSITORIES ||--o{ ACTIVITY_EVENTS : relates_to

    INTEGRATION_ACCOUNTS ||--o{ REPOSITORY_REMOTES : authenticates
    INTEGRATION_ACCOUNTS ||--o{ ACTIVITY_EVENTS : sources

    REPOSITORY_REMOTES ||--o{ REMOTE_RESOURCES : exposes
    REPOSITORY_REMOTES ||--o{ ACTIVITY_EVENTS : relates_to

    REMOTE_RESOURCES ||--o{ ACTIVITY_EVENTS : referenced_by
    REMOTE_RESOURCES ||--o| MILESTONES : backs

    MILESTONES ||--o{ MILESTONE_ITEMS : contains

    CHARACTER_PACKS ||--o{ PROJECT_CHARACTER_OVERRIDES : selected_by
```

## Database Rules

- Major user-owned entities use integer PK + stable UUID.
- Normal deletions use hard delete with confirmation where appropriate.
- Git remains source of truth for current branch/dirty/ahead/behind state.
- Credentials never live directly in SQLite.
- Character assets stay on disk.
- Character-pack DB records store metadata only.
- Activity retention permanently purges expired events.
- Remote resources are retained while active/relevant or referenced.
- Foreign keys are enabled.
- Project deletion removes only Xenrya-owned metadata, never local repos or remote provider data.
- Indexes are added based on actual query patterns.

## Likely Initial Indexes

```text
repositories(project_id)
repository_remotes(repository_id)
milestones(project_id, status)
milestone_items(milestone_id, position)
activity_events(project_id, occurred_at)
activity_events(event_type, occurred_at)
remote_resources(remote_repository_id, resource_type)
```
