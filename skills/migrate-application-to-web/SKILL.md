---
name: migrate-application-to-web
description: Analyze, plan, and incrementally migrate desktop, native, CLI, local-first, or other privileged-runtime applications to a browser frontend—especially Vite/React—backed by network APIs and server-side storage. Use when moving filesystem, process, OS, credential, event, or embedded-runtime capabilities behind backend services; changing source languages or backend stacks; replacing local or AI-provider integrations; creating migration matrices and API contracts; preserving selected behavior while redesigning platform-dependent features; or executing approved vertical migration slices. Do not use for ordinary behavior-preserving code cleanup.
---

# Migrate Application to Web

Treat this work as a platform migration and architecture redesign, not a mechanical language conversion. Preserve business semantics deliberately while replacing runtime assumptions that cannot exist in a browser.

## Load the relevant references

- Read [boundary-mappings.md](references/boundary-mappings.md) when inventorying local or privileged capabilities and selecting web replacements.
- Read [deliverables.md](references/deliverables.md) when producing the capability inventory, boundary ledger, migration matrix, architecture decision, or API contract.
- Read [execution-gates.md](references/execution-gates.md) before implementing a slice, migrating data, running parallel systems, cutting over, or deleting legacy code.

## Choose the operating mode

Infer the narrowest mode supported by the request:

1. **Assess**: inspect the source system and report its behavior, dependencies, boundaries, risks, and unknowns. Do not edit code.
2. **Design**: produce the migration matrix, target architecture, contracts, sequence, and verification plan. Do not edit product code.
3. **Execute one slice**: implement one approved end-to-end capability and update its migration artifacts.
4. **Review or cut over**: verify parity, operational readiness, data migration, and removal criteria.

For a whole-application request, default to Assess and Design. Stop after presenting the plan unless the user explicitly authorizes implementation without a review checkpoint. Never interpret “migrate the whole project” as permission for a big-bang rewrite.

## Establish the migration contract

Before designing code, classify every requirement:

- **Preserve**: externally observable behavior that must remain equivalent.
- **Change**: behavior intentionally redesigned for the target platform.
- **Retire**: capability that must not be migrated.
- **Unknown**: behavior requiring evidence or a product decision.

Record target constraints such as browser support, deployment model, tenancy, authentication, offline expectations, data residency, performance, scale, compliance, and whether the backend stack is fixed.

Do not silently choose a backend language or storage system. Prefer reuse only when it reduces migration risk without preserving unsuitable desktop assumptions.

## Follow the migration workflow

### Phase 1: Establish a behavioral baseline

Inspect source, tests, configuration, fixtures, data files, schemas, build scripts, and operational documentation.

Produce:

- user-visible capability inventory;
- entry points and module ownership;
- domain entities and invariants;
- persistence formats and compatibility requirements;
- background jobs, event flows, failure behavior, and recovery behavior;
- existing automated and manual verification;
- uncertain behavior that needs characterization tests or product input.

Do not equate current implementation details with required behavior.

### Phase 2: Inventory runtime boundaries

Find every capability that depends on the source runtime:

- filesystem paths, dialogs, watches, imports, exports, and atomic writes;
- local databases, caches, and application directories;
- child processes, shell commands, native libraries, devices, and clipboard;
- credentials, environment variables, machine identity, and OS permissions;
- custom URI schemes, local media serving, streaming events, and background tasks;
- direct model/provider SDK calls, local agents, and provider-specific payloads.

For each boundary, record the business purpose, caller, input/output, authority, failure modes, security assumptions, and proposed target owner. Use the boundary ledger in [deliverables.md](references/deliverables.md).

### Phase 3: Separate domain behavior from platform adapters

Extract the conceptual layers before deciding file structure:

- **Domain**: business rules, invariants, state transitions, and value objects.
- **Application**: use cases, orchestration, authorization intent, and transaction boundaries.
- **Ports/contracts**: persistence, files, jobs, model inference, events, clocks, and identifiers.
- **Adapters**: source runtime, HTTP handlers, databases, object storage, queues, and external providers.
- **Presentation**: browser UI, server state access, and ephemeral interaction state.

Reuse pure logic where practical. Reimplement platform adapters. Rewrite coupled logic only after characterizing the behavior that must survive.

### Phase 4: Define data ownership

Assign one authority for every state category:

- server-authoritative business data;
- object/blob storage for binary assets;
- browser cache of server state;
- recoverable local drafts or offline queue;
- ephemeral UI state;
- export files produced on demand.

Do not expose server filesystem paths to the browser. Exchange resource IDs, version tokens, signed or authorized URLs, and business metadata.

Use browser downloads for export, upload protocols for imports, and IndexedDB only for drafts, caches, or offline recovery unless the requirements explicitly make the browser the durable authority.

### Phase 5: Design the target architecture

Choose components from requirements rather than habit:

- Vite/React application organized by product capability;
- typed API client and runtime validation at trust boundaries;
- backend application services and domain modules;
- repositories for database or file/object storage;
- job execution for long-running work;
- SSE, WebSocket, or polling for progress;
- shared or generated API types;
- authentication, authorization, observability, and deployment boundaries.

Separate server state from client interaction state. Use a server-state library when its caching and invalidation model helps; reserve a client store for editor state, selections, drafts, and transient UI.

Document consequential choices as architecture decisions with alternatives and tradeoffs.

### Phase 6: Design contracts before implementations

Design APIs around resources and use cases, not one endpoint per legacy function.

Specify:

- endpoint or RPC operation and authorization;
- request, response, and error schemas;
- resource identifiers and versioning;
- upload/download behavior and size limits;
- idempotency and retry semantics;
- optimistic concurrency or conflict behavior;
- pagination and filtering where relevant;
- cancellation, timeout, and progress semantics for long-running work;
- compatibility and deprecation policy.

Create or update OpenAPI, JSON Schema, protobuf, or an equivalent machine-checkable contract. Generate types where feasible; otherwise verify frontend and backend schemas against the same contract.

### Phase 7: Build the migration matrix

Create one row per user capability, not per source function. Map:

`source behavior → target UI → API/use case → data owner → compatibility rule → tests → status`

Include explicit dependencies, risks, decisions, and rollback. Rank slices by architectural learning, user value, and dependency order. Prefer a small slice that crosses all new boundaries over a broad horizontal layer.

### Phase 8: Establish the target foundation

Build only what the first slice needs:

- application shells and local development workflow;
- API error envelope and request correlation;
- authentication skeleton if required;
- first storage repository and migration mechanism;
- typed client and contract verification;
- observability needed to diagnose the first slice;
- CI checks for both sides.

Avoid building every repository, route, component, and abstraction upfront.

### Phase 9: Migrate vertical slices

For each approved slice:

1. Define observable acceptance criteria.
2. Add characterization tests around behavior to preserve.
3. Finalize the slice contract.
4. Implement backend domain/application behavior.
5. Implement persistence and external adapters.
6. Implement the typed frontend client.
7. Implement the React flow and state handling.
8. Cover loading, failure, retry, cancellation, and conflict states.
9. Run contract, integration, end-to-end, and parity checks.
10. Update the migration matrix and decision log.

Keep the system runnable after every slice. Use a strangler path, feature flag, compatibility adapter, or dual-run comparison when replacement risk is material.

### Phase 10: Migrate data and cut over

Define:

- import and transformation rules;
- stable identity mapping;
- binary asset transfer;
- schema version handling;
- resumability and idempotency;
- validation totals and reconciliation;
- rollback and backup;
- coexistence duration and final ownership.

Delete legacy code only after its matrix rows meet the removal gates in [execution-gates.md](references/execution-gates.md).

## Isolate model and provider migration

Treat model access as a port, not a UI or domain concern. Define a provider-neutral contract for:

- normalized messages and structured inputs;
- asset uploads and references;
- streaming text, progress, tool, and result events;
- structured outputs and validation;
- cancellation, retry, timeout, and idempotency;
- safety, quota, authentication, and error categories;
- correlation between a request, background job, and persisted result.

Keep provider model names, SDK objects, event names, credentials, and transport recovery inside adapters. Preserve product-visible behavior, not the previous provider protocol. Use recorded fixtures or contract tests to compare old and new adapters when possible.

## Enforce implementation discipline

- Change one business slice at a time.
- Keep public contracts explicit and versioned.
- Avoid speculative frameworks and abstractions.
- Do not mix unrelated cleanup with migration work.
- Preserve source code until replacement behavior and data recovery are verified.
- Surface assumptions and blockers instead of inventing product policy.
- Re-run the relevant baseline after every slice.
- Record deliberate behavior changes so parity failures are not hidden as “platform differences.”

## Reject common failure modes

Do not:

- translate each source function into an endpoint;
- send absolute server paths to the browser;
- copy server data into a client store without an ownership/invalidation model;
- use local browser storage as accidental durable business storage;
- rewrite the frontend and backend horizontally before completing one usable flow;
- couple domain code to HTTP, database, browser, or provider SDK types;
- claim parity from successful compilation alone;
- delete legacy code before cutover, reconciliation, and rollback are proven.

## Define completion

Consider the migration complete only when:

- every in-scope capability has an accepted matrix disposition;
- preserved behaviors have evidence, and changed behaviors are documented;
- API and event contracts are versioned and verified;
- authoritative data ownership is unambiguous;
- imports, exports, background work, failures, and recovery are tested;
- migration and rollback are rehearsed on representative data;
- observability and support procedures exist;
- legacy dependencies and code are removed only after their traffic and data ownership reach zero.
