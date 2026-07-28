---
name: migrate-application-to-web
description: Analyze, document, design, execute, and close migrations from desktop, native, CLI, local-first, or privileged runtimes into a browser frontend and API-backed server, including integration into an existing product. Use for cross-framework or cross-language migrations; additive routes and modules; moving filesystem, process, credential, model, asset, job, or event capabilities behind services; defining API/data contracts and migration counts; preparing one side while the other is unavailable; or executing reviewed vertical slices. Requires a pre-migration capability and logic-chain baseline, a continuously updated progress ledger, and an implementation-backed final migration summary. Do not use for ordinary behavior-preserving cleanup.
---

# Migrate Application to Web

Treat the task as a change of runtime, authority, and product integration—not source-language translation. Preserve selected user outcomes while replacing assumptions that are unsafe or unavailable in browsers.

## Load references only when needed

- Read [boundary-mappings.md](references/boundary-mappings.md) to map local/privileged capabilities to web-safe ownership.
- Read [deliverables.md](references/deliverables.md) to create the baseline, logic-chain map, contracts, progress ledger, or final summary.
- Read [execution-gates.md](references/execution-gates.md) before implementation, data migration, cutover, or legacy removal.

## Select the operating mode

Use the narrowest mode authorized by the request:

1. **Assess**: inspect and report; do not edit.
2. **Design**: define scope, target architecture, contracts, counts, phases, and verification; do not edit product code.
3. **Execute**: implement approved vertical slices and update migration records.
4. **Review/cut over**: verify readiness, data migration, operations, rollback, and removal gates.

For an ambiguous whole-project request, assess and design before implementing. Once the user approves a concrete implementation plan, execute it without repeatedly reopening settled choices.

## Establish repository and authority boundaries first

Before writing code:

- locate every source, target frontend, target backend, shared package, and specification directory;
- read repository instructions and build/test commands;
- record which repositories are read-only and which may be changed;
- inspect branches, configured Git identity, dirty files, and untracked user artifacts;
- identify protected routes, components, APIs, tables, queues, and behaviors;
- confirm whether integration is additive, replacing, or temporarily coexisting;
- capture runtime versions and package-manager/toolchain requirements.

Keep migration artifacts in one isolated `spec/<migration-name>/` or `specs/<migration-name>/` directory, following the repository convention. Do not mix them into unrelated specifications. Never “clean up” an existing dirty worktree or include unrelated files in a commit.

## Establish the migration contract

Classify each user capability:

- **Preserve**: observable behavior must remain equivalent.
- **Change**: redesign is intentional and documented.
- **Retire**: explicitly out of scope.
- **Unknown**: requires evidence or a product decision.

Also record target constraints: frontend and backend stacks, authentication, database/object storage/queue choices, tenancy, browser support, offline behavior, deployment, performance, compliance, and compatibility.

Do not silently replace a fixed stack or reinterpret an additive integration as permission to overwrite existing routes and features.

## Maintain the migration record as a lifecycle

Treat records as migration deliverables, not retrospective notes:

1. **Before implementation — baseline**: record repositories, protected surfaces, user capabilities, source and target logic chains, state authority, contracts, counts, decisions, risks, phases, and acceptance criteria.
2. **During implementation — progress**: keep one status ledger current for every core slice. Record actual outcome, contract changes, verification, review, commit, blockers, and next step before moving on.
3. **After implementation — final record**: reconcile plans with code and publish a concise functional summary covering frontend/backend changes, end-to-end flows, API/data/queue/route counts, preserved invariants, verification, deployment requirements, and unverified boundaries.

Do not start from a file-copy list. Do not reconstruct progress only at the end. Do not leave planning documents claiming that completed components are still missing.

## Follow the evidence-to-record workflow

### 1. Evidence the source behavior

Inspect entry points, UI flows, modules, tests, fixtures, schemas, configuration, build scripts, and operational docs. Produce a capability inventory grouped by user workflow, including:

- the complete logic chain from user trigger through UI, state, runtime/service, persistence/provider, event delivery, and recovery;
- domain entities and invariants;
- persistence and asset formats;
- long-running work, events, failure, retry, and recovery;
- source runtime dependencies;
- evidence and unresolved behavior.

Implementation details are evidence, not automatically requirements.

For every in-scope workflow, draw or tabulate both chains:

`source trigger → source orchestration → source authority → result/event → recovery`

`target trigger → frontend state → API/use case → durable authority/worker/provider → event/replay → UI reconciliation`

Attach code, schema, test, or observed-runtime evidence. Mark gaps as unknown instead of inferring them.

### 2. Inventory runtime boundaries and state ownership

Find filesystem access, local databases, shell/process calls, native APIs, credentials, embedded runtimes, provider SDKs, background tasks, custom protocols, and event emitters.

For each boundary, assign:

- current and target authority;
- caller, input/output, security assumptions, and failure modes;
- synchronous, async, streaming, or resumable behavior;
- target API, database, object storage, queue, browser API, or redesign.

Separate server-authoritative business state, binary assets, browser cache, recoverable drafts, and ephemeral interaction state. Do not use a client store or local storage as accidental business persistence.

### 3. Design contracts before implementation

Design APIs around resources and user outcomes, not one endpoint per legacy function. Specify:

- authorization and tenant/account scope;
- exact request, response, error, and event schemas;
- stable IDs, pagination, and filtering;
- version tokens and real conflict behavior;
- upload types, size limits, checksums, lineage, and URL lifetime;
- idempotency, retries, cancellation, timeouts, and restart recovery;
- SSE/WebSocket/polling cursor and replay semantics;
- which side mutates authoritative data.

Return true transport status where clients depend on it; for example, optimistic-lock conflicts must not be hidden inside an HTTP 200 envelope.

Count APIs, tables, queues, and new routes only after the contract is explicit. Treat counts as planning outputs, not architecture targets.

### 4. Design the target around ownership

Keep conceptual boundaries clear:

- domain rules and state transitions;
- application orchestration and transaction boundaries;
- ports for storage, assets, jobs, providers, events, clocks, and IDs;
- adapters for HTTP, database, object storage, queues, and external services;
- presentation and ephemeral editor state.

For rich editors or canvases, keep the rendering/geometry scene pure: pass data and callbacks; do not let it import networking, server-state caches, routers, or global business state.

Use stable object names or resource IDs as durable asset identity. Generate current authorized/signed URLs when reading. Validate actual content type, dimensions, size, hash, and ownership/lineage server-side.

### 5. Build a phased vertical-slice plan

Create one migration-matrix row per user capability:

`source outcome → target route/UI → API/use case → authority → compatibility → verification → status`

Order slices by dependencies and product value. For integration into an existing product:

- add new routes/navigation instead of changing protected flows;
- prefer feature-local copies or adapters when shared refactors risk regression;
- keep the application runnable after each slice;
- use feature flags, compatibility adapters, or strangler routing where useful.

Define a commit sequence before implementation when the work spans multiple core capabilities.

Freeze the approved baseline and initialize the progress ledger before the first product-code change.

### 6. Prepare one side without faking the other

When the backend is unavailable, the frontend can still be integration-ready if it has:

- exact domain/API types and runtime response validation;
- a service boundary and server-state query layer;
- explicit query keys, invalidation, loading, error, conflict, and reconnect behavior;
- formal API calls rather than development-only fake persistence;
- component and contract tests at critical boundaries.

Document that browser calls will fail until the real service exists. Do not claim end-to-end readiness or invent a mock server unless requested.

When starting the backend from a prepared frontend contract, first reconcile both documents and isolate changes in the target backend’s migration-spec directory.

### 7. Implement durable backend boundaries

For long-running work:

- persist Job, Step, and ordered Event state in the database as the truth source;
- use a queue for delivery, not as the only record of truth;
- define claim, lease renewal, ACK, visibility recovery, and poison-message handling;
- commit status transitions and their events atomically;
- make create operations idempotent with a client request ID;
- ignore or quarantine late results after cancellation;
- retry failed work without repeating successful steps.

For optimistic documents, use compare-and-swap revisions. Preserve generated artifacts when finalization conflicts; where safe, retry only finalization rather than repeating expensive generation.

For streaming, persist before emitting. Support replay after a sequence/cursor, deduplicate client-side, authorize before opening the stream, and send heartbeats.

### 8. Execute each approved slice

For each slice:

1. Mark the slice `in progress`; record its protected behavior, acceptance criteria, authority, and finalized API/event/data contract.
2. Implement the smallest complete backend and frontend path.
3. Cover loading, empty, error, retry, cancellation, conflict, refresh, and reconnect states that apply.
4. Add tests only at consequential boundaries; do not create one test file per implementation file.
5. Run focused checks, then the repository’s required full checks at phase boundaries.
6. Review the diff against repository and authority boundaries.
7. Update the ledger with actual behavior, verification evidence, review result, blockers, and next step.
8. When commits are authorized or expected, update the ledger before the core commit and use the configured identity.

Do not push, open a merge request, deploy, migrate live data, or enable a production feature without authorization.

### 9. Integrate and cut over honestly

Verify frontend/backend schema agreement, authentication, ownership isolation, conflict transport, upload limits, event replay, cancellation, refresh recovery, and representative user workflows.

If MySQL, Redis, object storage, provider credentials, or production-like services are unavailable, complete safe code/test/build work and record the real integration blocker. Never report a mocked or unexecuted external dependency as successful.

Apply schema/configuration before enabling a feature flag. Rehearse data migration, reconciliation, backup, rollback, and coexistence before cutover. Remove legacy code only after its traffic and authority reach zero.

### 10. Close with an implementation-backed summary

Reinspect the final frontend, backend, schema, configuration, tests, and deployment boundaries. Produce a final summary from implemented behavior, not from the original plan.

Include:

- source-to-target capability and responsibility mapping;
- final frontend routes, modules, state ownership, and user functions;
- final backend APIs, tables, assets, jobs, queues, events, and reused platform services;
- end-to-end success, failure, retry, cancellation, conflict, refresh, and reconnect chains;
- exact additive, replaced, retired, and deliberately unimplemented scope;
- actual counts and their code/schema evidence;
- verification commands and results;
- manual configuration, schema, gateway, credential, deployment, and rollback requirements;
- external integrations and E2E scenarios that remain unexecuted.

Use compact tables and flow/sequence diagrams when they clarify ownership or ordering. Distinguish `code complete`, `integration-ready`, `verified`, `cut-over`, and `retired`; never collapse them into “done.”

## Treat model/provider migration as an adapter

Keep provider credentials, SDK types, raw events, model names, and transport recovery behind a provider-neutral port. Normalize messages, assets, structured output, usage, errors, cancellation, retries, and request/job/result correlation.

Validate structured output before applying it to authoritative state. Scope partial edits to the requested resource or subresource so a model result cannot cross project, document, or component boundaries.

## Review checklist

Before declaring a phase complete, confirm:

- only authorized repositories and files changed;
- protected routes, APIs, tables, queues, and user behaviors remain unchanged;
- contracts and runtime parsers agree;
- server state is not duplicated into client interaction state;
- authoritative queries include account/tenant ownership;
- signed URLs and secrets are not durable identifiers;
- conflict, idempotency, cancellation, replay, and restart recovery are explicit;
- tests are proportional and verification commands passed in the required runtime;
- baseline, progress ledger, final summary, and known external blockers agree with the code;
- unrelated dirty/untracked files are preserved.

## Completion

The migration is complete only when every in-scope matrix row has accepted evidence; authority is unambiguous; contracts, assets, background work, failures, and recovery are verified; data migration and rollback are rehearsed; operational ownership exists; the final migration summary matches the implementation; and legacy paths are removed only after cutover criteria are met.
