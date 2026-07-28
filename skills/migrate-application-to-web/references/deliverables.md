# Migration Deliverables

Use these compact templates. Keep evidence linked to code, tests, schemas, or observed behavior. Mark guesses as assumptions.

## Contents

- Repository authority map
- Capability inventory
- Runtime boundary ledger
- Behavior contract
- Migration matrix
- Architecture decision
- API/use-case contract
- Integration-readiness record
- Migration progress record
- Slice acceptance packet

## Repository authority map

| Repository/path | Role | Writable? | Baseline/branch | Protected surfaces | Dirty/untracked state | Toolchain and verification |
|---|---|---|---|---|---|---|
| Path | Source/target frontend/target backend/specs | yes/no | Commit or branch | Routes/APIs/tables/files | Preserve exactly | Runtime and commands |

Create this before implementation. Keep source and target specifications in their intended repositories; do not assume the current working directory is the write target.

## Capability inventory

| Capability | User outcome | Source entry point | Domain rules | Runtime dependencies | Data touched | Failure/recovery | Disposition |
|---|---|---|---|---|---|---|---|
| Name | Why it exists | UI/command/module | Invariants | FS/process/SDK/etc. | Entities/assets | Observable behavior | Preserve/change/retire/unknown |

Group by user workflow. Do not create one row per function.

## Runtime boundary ledger

| Boundary ID | Business purpose | Source caller/adapter | Inputs/outputs | Current authority | Security assumptions | Target owner/pattern | Open decision |
|---|---|---|---|---|---|---|---|
| B-001 | Purpose | Location | Contract | Source of truth | Trust/credentials | API/job/storage | Question |

Use stable boundary IDs in architecture decisions and migration-matrix dependencies.

## Behavior contract

For each capability, record:

```text
Given:
When:
Then:
Failure behavior:
Recovery behavior:
Performance or ordering constraint:
Evidence:
Classification: preserve | change | retire | unknown
```

Prefer externally observable behavior. Avoid freezing accidental implementation details.

## Migration matrix

| Slice | Source behavior | Target UI | API/use case | Data owner | Compatibility/data migration | Verification | Dependencies | Status | Risk/decision |
|---|---|---|---|---|---|---|---|---|---|
| S-001 | Capability | Route/feature | Contract | DB/blob/client draft | Rule | Tests | IDs | discovered/designed/building/verified/cut-over/retired | Note |

Use these status meanings:

- `discovered`: source behavior has evidence.
- `designed`: target ownership and contract are accepted.
- `building`: implementation is in progress.
- `verified`: slice passes contract, integration, E2E, and required parity checks.
- `cut-over`: target is authoritative for production use.
- `retired`: legacy path and data ownership are removed.

## Architecture decision

```markdown
# ADR: Decision title

## Context
Constraints, forces, and evidence.

## Decision
Chosen boundary, ownership, or technology.

## Alternatives
Viable options and why they were not selected.

## Consequences
Benefits, costs, operational impact, and migration impact.

## Validation
How to prove the decision works and when to revisit it.
```

Write an ADR for consequential choices such as backend reuse versus rewrite, durable storage, event transport, offline support, provider abstraction, or compatibility strategy.

## API/use-case contract

```text
Operation:
User/actor and authorization:
Request schema:
Response schema:
Error schema and status mapping:
Resource identity/version:
Authoritative mutation owner:
Idempotency/retry:
Concurrency/conflicts:
Upload/download limits:
Async progress/cancellation:
Audit/observability:
Compatibility/versioning:
Contract tests:
```

For errors, prefer a stable product envelope:

```json
{
  "code": "STABLE_PRODUCT_CODE",
  "message": "User-safe summary",
  "requestId": "correlation-id",
  "details": {}
}
```

Do not leak stack traces, provider payloads, or server paths.

## Integration-readiness record

When one side is not runnable, record what is and is not ready:

- contract/types/runtime parsers complete;
- formal service/query boundary complete;
- conflict, replay, cancellation, and refresh behavior covered;
- focused tests and build evidence;
- real external dependencies still unavailable;
- exact live-integration steps and acceptance checks.

Do not label this end-to-end verified.

## Migration progress record

Maintain one target-side document:

| Phase | Core outcome | Status | Verification | Review | Commit | Blocker/next step |
|---|---|---|---|---|---|---|
| Phase | Capability, not file list | planned/in progress/complete | Commands and results | Protected surfaces checked | Subject/hash | External dependency or next slice |

Update it before each core commit. Record failed checks and their resolution when they reveal a reusable environment or contract constraint.

## Slice acceptance packet

Before implementation, require:

- accepted behavior contract;
- accepted API/event schemas;
- explicit data authority;
- characterization coverage for preserved rules;
- failure, retry, cancellation, and conflict expectations;
- migration and rollback steps;
- commands or scenarios that prove completion.

After implementation, attach evidence and update the migration matrix instead of relying on a narrative status claim.
