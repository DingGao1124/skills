---
name: refactor-codebase
description: "Classify, plan, execute, and review repository refactoring across four levels: code quality, business logic, system architecture, and technology migration. Use for vague or broad 'refactor this codebase' requests; behavior-preserving cleanup; domain-rule or workflow redesign; module, service, dependency, or data-ownership changes; and language, framework, platform, runtime, storage, or provider migrations. Route the task to level-specific workflows and apply stronger planning, compatibility, rollout, and rollback controls as impact increases. Do not use for an isolated bug fix with no structural intent."
---

# Refactor Codebase

Classify the change before loading detailed instructions. Read only the references required by the selected level and operating mode.

## Classify the refactoring level

| Level | Category | Defining change | Key constraint |
|---|---|---|---|
| 1 | Code Quality Refactoring | Improve code without intentionally changing behavior, rules, interfaces, or data formats | Prove behavior preservation |
| 2 | Business Logic Refactoring | Change, clarify, consolidate, add, or remove domain rules and workflows | Make every rule disposition explicit |
| 3 | Architecture Refactoring | Change module boundaries, dependency direction, data ownership, communication, or deployment topology | Define and enforce contracts |
| 4 | Technology Migration | Replace a language, framework, platform, runtime, storage engine, protocol, vendor, or provider | Plan compatibility and cutover |

## Decide with evidence

Ask in order:

1. Will user-visible rules, outcomes, workflows, states, or domain meaning change? Mark that part Level 2.
2. Will responsibilities, data authority, module/service boundaries, dependency direction, or runtime communication change? Mark that part Level 3.
3. Will a language, framework, platform, runtime, storage technology, protocol, vendor, or provider be replaced? Mark that part Level 4.
4. If none apply and only readability, duplication, typing, error handling, decomposition, or testability changes, mark it Level 1.

Inspect the repository before accepting the user's label. Assign one primary level using the highest-impact change and record secondary levels.

Examples:

- Extract a long function and remove duplication: Level 1.
- Change discount eligibility and consolidate policies: Level 2.
- Split a module and transfer database ownership: Level 3.
- Upgrade a framework: Level 4, often with Level 1 secondary work.
- Move a native application to browser frontend plus backend APIs: Level 4 plus Level 3.

## Load only the required resources

- Level 1: read [code-quality-refactoring.md](references/code-quality-refactoring.md).
- Level 2: read [business-logic-refactoring.md](references/business-logic-refactoring.md).
- Level 3: read [architecture-refactoring.md](references/architecture-refactoring.md).
- Level 4: read [technology-migration.md](references/technology-migration.md).
- Whole-repository, multi-file, ambiguous, plan/RFC, Level 2–4, rollout, or cutover work: also read [planning-and-governance.md](references/planning-and-governance.md).

For mixed work, load every affected level resource and use the highest level to set governance strength. A small, bounded Level 1 edit may skip the shared planning reference.

## Select the operating mode

Identify whether the request asks to Assess, Plan, Execute, Review, or Cut over/Retire. For whole-codebase or high-impact work, first analyze without editing, produce the required design and plan, then stop for confirmation unless the user explicitly authorizes uninterrupted implementation.

## Reclassify when scope changes

Stop and surface the impact when:

- Level 1 work changes behavior;
- Level 2 work moves ownership or architecture;
- Level 3 work requires technology replacement;
- Level 4 work introduces an unapproved business-rule change;
- compatibility, data authority, or rollback becomes ambiguous.

Never hide a higher-level transformation inside a lower-risk “cleanup” label.
