# Execution and Cutover Gates

Use these gates to keep a migration incremental and reversible.

## Contents

- Gates 0–7: scope through legacy removal
- Vertical-slice commit rhythm
- Stop conditions

## Gate 0: Scope accepted

Require:

- repository authority map, writable targets, and protected surfaces;
- clean or explicitly preserved target baseline, including dirty/untracked user files;
- required runtime/toolchain versions and baseline verification commands;
- target users and deployment model;
- in-scope and retired capabilities;
- preserve/change/retire/unknown classification;
- nonfunctional constraints;
- named product decisions still blocking architecture.

Do not implement around unresolved decisions that alter data ownership, security, tenancy, offline behavior, or API shape.

## Gate 1: Source behavior evidenced

Require:

- capability inventory linked to source;
- representative legacy data and edge cases;
- characterization tests or reproducible manual scenarios;
- known failure and recovery behavior;
- runtime boundary ledger.

Compilation and type checking are not behavioral evidence.

## Gate 2: Target contract accepted

Require:

- architecture decision for major boundaries;
- authoritative owner for each data category;
- API and event schemas;
- authoritative mutation owner for each workflow;
- identity, versioning, idempotency, and conflict rules;
- transport status behavior where clients depend on HTTP semantics;
- import/export and binary transport rules;
- provider-neutral contracts for external model/runtime integrations.

## Gate 3: Slice ready

Select a slice that delivers one user outcome through all required layers. Confirm:

- acceptance criteria;
- small bounded file/module scope;
- dependencies already available or included;
- rollback path;
- tests to add before and after implementation;
- observable completion signal.

Avoid slices named “all backend,” “all components,” or “convert every command.”

## Gate 4: Slice verified

Run the applicable layers:

1. characterization tests against preserved source behavior;
2. domain unit tests;
3. API schema and contract tests;
4. repository/provider adapter integration tests;
5. frontend component tests for state transitions;
6. browser end-to-end test for the user outcome;
7. failure tests for timeout, retry, conflict, cancellation, and reconnect;
8. parity or explicitly accepted-difference checks.

Update the migration matrix to `verified` only with evidence.
If an external dependency is unavailable, mark the slice integration-ready rather than verified and record the blocker.
Run verification with the repository’s required toolchain version; results from an incompatible runtime are not valid evidence.

## Gate 5: Data migration rehearsed

Require:

- versioned, resumable, idempotent migration;
- representative production-scale sample;
- counts, checksums, or semantic reconciliation;
- invalid-record quarantine and reporting;
- backup and tested rollback;
- stable mapping from legacy identity to target identity;
- binary asset verification.

Never make a one-off destructive script the only migration path.

## Gate 6: Cutover ready

Require:

- target observability, alerts, and request correlation;
- support and incident recovery procedure;
- capacity and performance evidence;
- security and authorization review;
- feature flag, routing switch, or controlled release mechanism;
- rollback time objective and responsible owner;
- no unclassified parity differences.

For high-risk systems, use shadow traffic, read-only mirroring, dual writes with reconciliation, or cohort rollout. State which system is authoritative at every stage.

## Gate 7: Legacy removal ready

Remove a legacy path only when:

- its matrix rows are `cut-over`;
- no production reads, writes, jobs, or provider calls depend on it;
- target data is reconciled and backed up;
- compatibility/export obligations are met;
- rollback no longer requires executable legacy code, or the removal risk is accepted;
- documentation, deployment, secrets, and monitoring are updated.

Delete in a separate change from the initial cutover when practical.

## Vertical-slice commit rhythm

Prefer small working checkpoints:

1. baseline and isolated migration records;
2. contract/schema;
3. target shell or typed client;
4. backend domain, persistence, and adapters;
5. one vertical user flow;
6. integration and failure recovery;
7. migration evidence and later cutover/removal.

Each core checkpoint must leave the branch buildable, update progress evidence, and use the repository-configured Git identity. Do not include unrelated dirty files. Pushing, deployment, live migration, and feature enablement remain separate authorizations.

## Stop conditions

Pause and request direction when:

- observed behavior contradicts the stated requirement;
- two systems could both become authoritative;
- an API would expose server paths, secrets, or provider internals;
- a slice requires an unapproved schema or product change;
- source behavior lacks enough evidence to preserve safely;
- rollback cannot restore data ownership;
- the requested edit expands into a multi-capability rewrite.
