# Business Logic Refactoring

Use for Level 2 work. Business behavior may change, so expose every rule decision and do not disguise it as cleanup.

## First pass: analyze only

Do not edit code. Output:

1. core business entities;
2. entity states and state transitions;
3. principal business use cases;
4. permissions and data-validation rules;
5. exception, failure, and recovery rules;
6. business logic dispersed through UI, APIs, persistence, jobs, and integrations;
7. historical-data and compatibility constraints;
8. unknown or conflicting behavior.

Classify every affected rule:

- **Keep**: preserve;
- **Change**: alter an existing rule;
- **Add**: introduce a new rule;
- **Remove**: retire a rule;
- **Unknown**: require evidence or a product decision.

Never guess an Unknown rule.

## Build a decision table

Create one row per meaningful case:

| Use case | Current state | Operation | Preconditions | Success result | Failure result | State change | Data change | Rule disposition |
|---|---|---|---|---|---|---|---|---|

Include authorization, validation, precedence, time, locale, rounding, idempotency, and legacy-data cases where relevant.

## Design the target logic

- separate domain rules from UI, transport, database, clock, randomness, and provider details;
- use named policies, value objects, pure decisions, or explicit state machines when they clarify real concepts;
- define old and new behavior, errors, data meaning, audit needs, and effective dates;
- version rules or migrate data when old records must retain historical meaning.

## Execute by complete use case

1. Characterize Keep rules.
2. Add acceptance tests for Change, Add, and Remove.
3. Introduce the smallest target domain seam.
4. Migrate one complete business use case.
5. Update callers, persistence, and presentation for that use case.
6. Compare representative old/new outcomes.
7. Roll out controllably when impact is material.
8. Remove the old rule only after consumers and data compatibility are verified.

## Completion criteria

- every rule has a confirmed disposition;
- new or changed behavior has product acceptance evidence;
- preserved behavior has characterization evidence;
- decision tables and automated tests cover success, failure, and transitions;
- no unintended duplicate source of business truth remains.
