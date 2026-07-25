# Migration Deliverables

Use these compact templates. Keep evidence linked to code, tests, schemas, or observed behavior. Mark guesses as assumptions.

## Contents

- Capability inventory
- Runtime boundary ledger
- Behavior contract
- Migration matrix
- Architecture decision
- API/use-case contract
- Target architecture summary
- Slice acceptance packet

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

## Target architecture summary

Document:

1. context diagram and trust boundaries;
2. frontend feature boundaries and state ownership;
3. backend application/domain/repository boundaries;
4. API, event, and binary transport contracts;
5. database/object storage ownership;
6. authentication and tenant isolation;
7. background jobs and provider adapters;
8. observability and operations;
9. compatibility, migration, rollback, and deletion strategy.

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
