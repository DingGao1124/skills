# Planning and Governance

Use for whole-repository, multi-file, ambiguous, Level 2–4, rollout, cutover, or plan/RFC work.

## Operating modes

- **Assess**: inspect and report; do not edit.
- **Plan**: define target, decisions, sequence, verification, and rollback; do not edit product code.
- **Execute**: implement one approved, bounded step or vertical slice.
- **Review**: compare a plan or change against contracts and evidence.
- **Cut over/Retire**: transfer authority, reconcile, observe, then remove legacy paths.

For whole-codebase work, default to Assess and Plan, then stop for confirmation unless uninterrupted implementation was explicitly authorized.

## Investigate before asking

Read source, tests, configuration, schemas, builds, deployment, repository instructions, and documentation. Ask only questions that materially change behavior, scope, ownership, architecture, technology, security, rollout, or acceptance.

## Plan structure

```markdown
## Refactor Plan: Title

### Classification
Primary level, secondary levels, mode, and rationale.

### Current State
Observed behavior, modules, ownership, dependencies, tests, and evidence.

### Target State
Accepted behavior, boundaries, contracts, and nonfunctional goals.

### Dispositions
Keep/Change/Add/Remove/Unknown or Reuse/Port/Redesign/Replace/Remove/Unknown.

### Affected Areas
Files, modules, services, data, contracts, and dependencies.

### Decisions and Open Questions
Alternatives, tradeoffs, assumptions, and blockers.

### Execution Sequence
Small steps or vertical slices, each with verification and rollback.

### Test and Validation Strategy
Characterization, unit, contract, integration, E2E, build, type, static, performance, security, and operational checks as applicable.

### Rollout and Rollback
Flags, compatibility, migration, observation, reconciliation, and recovery.

### Out of Scope
Explicit exclusions.

### Completion Criteria
Evidence required to declare completion.
```

## Governance strength

| Level | Minimum evidence | Additional control |
|---|---|---|
| 1 | behavior baseline and before/after tests | small reversible edits |
| 2 | rule inventory, decision tables, accepted new behavior | data compatibility and controlled rollout |
| 3 | module/data-flow model and contract tests | ADRs, adapters, operational rollback |
| 4 | compatibility inventory, migration matrix, old/new comparison | data rehearsal, coexistence, cutover gates |

Use the highest affected level.

## Recommended order for mixed transformations

1. clarify business requirements;
2. design accepted target architecture;
3. perform technology migration by vertical slices;
4. finish with local code-quality cleanup.

Do not mix intentional business changes with mechanical migration without separate dispositions and acceptance.

## Execute and verify

For each step:

1. define observable success criteria;
2. run the relevant baseline;
3. make the smallest coherent change;
4. run focused validation and required stable-checkpoint suites;
5. inspect the diff and runtime evidence;
6. update decisions and plans;
7. preserve a tested rollback path.

Create branches, commits, GitHub issues, or external records only when the user asks or repository workflow requires them.
