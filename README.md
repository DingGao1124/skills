# Agent Skills

[简体中文](./README.zh.md)

Two reusable Agent Skills for codebase refactoring and application migration.

## Skills

### `refactor-codebase`

Classifies a refactoring task before loading the appropriate workflow:

- Code quality refactoring
- Business logic refactoring
- Architecture refactoring
- Technology migration

Use it to refactor an entire repository, reorganize business rules, redesign module boundaries, or plan a technology migration. Validation, compatibility, rollout, and rollback controls become stricter as the impact grows.

[View SKILL.md](./skills/refactor-codebase/SKILL.md)

### `migrate-application-to-web`

Migrates desktop, local-first, CLI, and other privileged-runtime applications to browser-based architectures, especially Vite/React frontends backed by network APIs.

It covers:

- Moving filesystem, process, credential, and OS capabilities behind backend services
- Designing API contracts, server-side storage, and consistent error formats
- Classifying features as reuse, port, redesign, replace, or remove
- Migrating one end-to-end vertical slice at a time
- Handling data compatibility, model-provider replacement, cutover, and rollback

[View SKILL.md](./skills/migrate-application-to-web/SKILL.md)

## Installation

After publishing the repository, replace `<owner/repo>` with its GitHub path:

```bash
# List available Skills
npx skills add <owner/repo> --list

# Install all Skills
npx skills add <owner/repo> --all

# Install one Skill
npx skills add <owner/repo> --skill refactor-codebase
npx skills add <owner/repo> --skill migrate-application-to-web
```

## Usage

```text
Use $refactor-codebase to classify this repository's refactoring work. Only provide analysis and a plan in the first pass.
```

```text
Use $migrate-application-to-web to plan the migration of this desktop application to Vite/React and backend APIs.
```

## Structure

```text
skills/
├── README.md
├── README.zh.md
└── skills
    ├── skills-lock.toml
    ├── refactor-codebase/
    │   ├── SKILL.md
    │   ├── agents/
    │   └── references/
    └── migrate-application-to-web/
        ├── SKILL.md
        ├── agents/
        └── references/
```

`skills-lock.toml` is a lightweight package manifest containing each Skill's name, purpose, entry point, resources, and content checksum.
